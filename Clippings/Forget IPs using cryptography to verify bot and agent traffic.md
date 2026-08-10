---
title: "Forget IPs: using cryptography to verify bot and agent traffic"
source: https://blog.cloudflare.com/web-bot-auth/
author:
published: 2025-05-15
created: 2026-08-09
description: Bots now browse like humans. We're proposing bots use cryptographic signatures so that website owners can verify their identity. Explanations and demonstration code can be found within the post.
tags:
  - clippings
  - cloudflare
  - agentic
  - ArtificialIntelligence
---
With the rise of traffic from [AI agents](https://www.cloudflare.com/learning/ai/what-is-agentic-ai/), what’s considered a bot is no longer clear-cut. There are some clearly malicious bots, like ones that DoS your site or do [credential stuffing](https://www.cloudflare.com/learning/bots/what-is-credential-stuffing/), and ones that most site owners do want to interact with their site, like the bot that indexes your site for a search engine, or ones that fetch RSS feeds.

Historically, Cloudflare has relied on two main signals to verify legitimate web crawlers from other types of automated traffic: user agent headers and IP addresses. The [`User-Agent` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/User-Agent) allows bot developers to identify themselves, i.e. `MyBotCrawler/1.1`. However, user agent headers alone are easily spoofed and are therefore insufficient for reliable identification. To address this, user agent checks are often supplemented with [IP address validation](https://developers.cloudflare.com/bots/concepts/bot/verified-bots/policy/#ip-validation), the inspection of published IP address ranges to confirm a crawler's authenticity. However, the logic around IP address ranges representing a product or group of users is brittle – connections from the crawling service might be shared by multiple users, such as in the case of [privacy proxies](https://blog.cloudflare.com/icloud-private-relay/) and VPNs, and these ranges, often maintained by cloud providers, change over time.

Cloudflare will always try to block malicious bots, but we think our role here is to also provide an affirmative mechanism to authenticate desirable bot traffic. By using well-established cryptography techniques, we’re proposing a better mechanism for legitimate agents and bots to declare who they are, and provide a clearer signal for site owners to decide what traffic to permit.

**Today, we’re introducing two proposals – HTTP message signatures and request mTLS – for [friendly bots](https://blog.cloudflare.com/friendly-bots/) to authenticate themselves, and for customer origins to identify them.** In this blog post, we’ll share how these authentication mechanisms work, how we implemented them, and how you can participate in our closed beta.

## Existing bot verification mechanisms are broken

Historically, if you’ve worked on ChatGPT, Claude, Gemini, or any other agent, you’ve had several options to identify your HTTP traffic to other services:

1. You define a [user agent](https://www.rfc-editor.org/rfc/rfc9110#name-user-agent), an HTTP header described in [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110.html#name-user-agent). The problem here is that this header is easily spoofable and there’s not a clear way for agents to identify themselves as semi-automated browsers — agents often use the Chrome user agent for this very reason, which is discouraged. The RFC [states](https://www.rfc-editor.org/rfc/rfc9110.html#section-10.1.5-9):  
	*“If a user agent masquerades as a different user agent, recipients can assume that the user intentionally desires to see responses tailored for that identified user agent, even if they might not work as well for the actual user agent being used.”*
2. You publish your IP address range(s). This has limitations because the same IP address might be shared by multiple users or multiple services within the same company, or even by multiple companies when hosting infrastructure is shared (like [Cloudflare Workers](https://www.cloudflare.com/developer-platform/products/workers/), for example). In addition, IP addresses are prone to change as underlying infrastructure changes, leading services to use ad-hoc sharing mechanisms like [CIDR lists](https://www.cloudflare.com/ips-v4).
3. You go to every website and share a secret, like a [Bearer](https://www.rfc-editor.org/rfc/rfc6750) token. This is impractical at scale because it requires developers to maintain separate tokens for each website their bot will visit.

We can do better! Instead of these arduous methods, we’re proposing that developers of bots and agents cryptographically sign requests originating from their service. When protecting origins, [reverse proxies](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/) such as Cloudflare can then validate those signatures to confidently identify the request source on behalf of site owners, allowing them to take action as they see fit.

![BLOG-2820 Image 1](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01KW46D3CRTBJ5JCH4P4PTCXRJ.png&w=1430&h=714&f=webp&fit=cover&position=center)

BLOG-2820 Image 1

A typical system has three actors:

- User: the entity that wants to perform some actions on the web. This may be a human, an automated program, or anything taking action to retrieve information from the web.
- Agent: an orchestrated browser or software program. For example, Chrome on your computer, or OpenAI’s [Operator](https://operator.chatgpt.com/) with ChatGPT. Agents can interact with the web according to web standards (HTML rendering, JavaScript, subrequests, etc.).
- Origin: the website hosting a resource. The user wants to access it through the browser. This is Cloudflare when your website is using our services, and it’s your own server(s) when exposed directly to the Internet.

In the next section, we’ll dive into HTTP Message Signatures and request mTLS, two mechanisms a browser agent may implement to sign outgoing requests, with different levels of ease for an origin to adopt.

## Introducing HTTP Message Signatures

[HTTP Message Signatures](https://www.rfc-editor.org/rfc/rfc9421.html) is a standard that defines the cryptographic authentication of a request sender. It’s essentially a cryptographically sound way to say, “hey, it’s me!”. It’s not the only way that developers can sign requests from their infrastructure — for example, AWS has used [Signature v4](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html), and Stripe has a framework for [authenticating webhooks](https://docs.stripe.com/webhooks#verify-webhook-signatures-with-official-libraries) — but Message Signatures is a published standard, and the cleanest, most developer-friendly way to sign requests.

We’re working closely with the wider industry to support these standards-based approaches. For example, OpenAI has started to sign their requests. In their own words:

> *"Ensuring the authenticity of Operator traffic is paramount. With HTTP Message Signatures ([RFC 9421](https://www.rfc-editor.org/rfc/rfc9421.html)), OpenAI signs all Operator requests so site owners can verify they genuinely originate from Operator and haven’t been tampered with”* – Eugenio, Engineer, OpenAI

Without further delay, let’s dive in how HTTP Messages Signatures work to identify bot traffic.

### Scoping standards to bot authentication

Generating a message signature works like this: before sending a request, the agent signs the target origin with a public key. When fetching `https://example.com/path/to/resource`, it signs `example.com`. This public key is known to the origin, either because the agent is well known, because it has previously registered, or any other method. Then, the agent writes a **Signature-Input** header with the following parameters:

1. A validity window (`created` and `expires` timestamps)
2. A Key ID that uniquely identifies the key used in the signature. This is a [JSON Web Key Thumbprint](https://www.rfc-editor.org/rfc/rfc7638.html).
3. A tag that shows websites the signature’s purpose and validation method, i.e. `web-bot-auth` for bot authentication.

In addition, the `Signature-Agent` header indicates where the origin can find the public keys the agent used when signing the request, such as in a directory hosted by `signer.example.com`. This header is part of the signed content as well.

Here’s an example:

```
GET /path/to/resource HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 Chrome/113.0.0 MyBotCrawler/1.1
Signature-Agent: signer.example.com
Signature-Input: sig=("@authority" "signature-agent");\
                  created=1700000000;\
                  expires=1700011111;\
                  keyid="ba3e64==";\
                  tag="web-bot-auth"
Signature: sig=abc==
```

For those building bots, [we propose](https://datatracker.ietf.org/doc/draft-meunier-web-bot-auth-architecture/) signing the authority of the target URI, i.e. www.example.com, and a way to retrieve the bot public key in the form of [signature-agent](https://datatracker.ietf.org/doc/draft-meunier-http-message-signatures-directory/), if present, i.e. [crawler.search.google.com](http://crawler.search.google.com/) for Google Search, [operator.openai.com](http://operator.openai.com/) for OpenAI Operator, workers.dev for Cloudflare Workers.

The `User-Agent` from the example above indicates that the software making the request is Chrome, because it is an agent that uses an orchestrated Chrome to browse the web. You should note that `MyBotCrawler/1.1` is still present. The `User-Agent` header can actually contain multiple products, in decreasing order of importance. If our agent is making requests via Chrome, that’s the most important product and therefore comes first.

At Internet-level scale, these signatures may add a notable amount of overhead to request processing. However, with the right cryptographic suite, and compared to the cost of existing bot mitigation, both technical and social, this seems to be a straightforward tradeoff. This is a metric we will monitor closely, and report on as adoption grows.

### Generating request signatures

We’re making several examples for generating Message Signatures for bots and agents [available on Github](https://github.com/cloudflareresearch/web-bot-auth/) (though we encourage other implementations!), all of which are standards-compliant, to maximize interoperability.

Imagine you’re building an agent using a managed Chromium browser, and want to sign all outgoing requests. To achieve this, the [webextensions standard](https://github.com/w3c/webextensions) provides [chrome.webRequest.onBeforeSendHeaders](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/webRequest/onBeforeSendHeaders), where you can modify HTTP headers before they are sent by the browser. The event is [triggered](https://developer.chrome.com/docs/extensions/reference/api/webRequest#life_cycle_of_requests) before sending any HTTP data, and when headers are available.

Here’s what that code would look like:

```
chrome.webRequest.onBeforeSendHeaders.addListener(
  function (details) {
    // Signature and header assignment logic goes here
      // <CODE>
  },
  { urls: ["<all_urls>"] },
  ["blocking", "requestHeaders"] // requires "installation_mode": "force_installed"
);
```

Cloudflare provides a [web-bot-auth](https://www.npmjs.com/package/web-bot-auth) helper package on npm that helps you generate request signatures with the correct parameters. `onBeforeSendHeaders` is a Chrome extension hook that needs to be implemented synchronously. To do so, we `import {signatureHeadersSync} from “web-bot-auth”`. Once the signature completes, both `Signature` and `Signature-Input` headers are assigned. The request flow can then continue.

```
const request = new URL(details.url);
const created = new Date();
const expired = new Date(created.getTime() + 300_000)

// Perform request signature
const headers = signatureHeadersSync(
  request,
  new Ed25519Signer(jwk),
  { created, expires }
);
// \`headers\` object now contains \`Signature\` and \`Signature-Input\` headers that can be used
```

This extension code is available on [GitHub](https://github.com/cloudflareresearch/web-bot-auth/), alongside a debugging server, deployed at [https://http-message-signatures-example.research.cloudflare.com](https://http-message-signatures-example.research.cloudflare.com/).

### Validating request signatures

Using our [debug server](https://http-message-signatures-example.research.cloudflare.com/), we can now inspect and validate our request signatures from the perspective of the website we’d be visiting. We should now see the Signature and Signature-Input headers:

![BLOG-2820 Image 2](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01KW44AP06C96DKSP1HP4DWQWP.png&w=1430&h=954&f=webp&fit=cover&position=center)

BLOG-2820 Image 2

<sup><em>In this example, the homepage of the debugging server validates the signature from the RFC 9421 Ed25519 verifying key, which the extension uses for signing.</em></sup>

The above demo and code walkthrough has been fully written in TypeScript: the verification website is on Cloudflare Workers, and the client is a Chrome browser extension. We are cognisant that this does not suit all clients and servers on the web. To demonstrate the proposal works in more environments, we have also implemented bot signature validation in Go with a [plugin](https://github.com/cloudflareresearch/web-bot-auth/tree/main/examples/caddy-plugin) for [Caddy server](https://caddyserver.com/).

## Experimentation with request mTLS

HTTP is not the only way to convey signatures. For instance, one mechanism that has been used in the past to authenticate automated traffic against secured endpoints is [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/), the “mutual” presentation of [TLS certificates](https://www.cloudflare.com/application-services/products/ssl/). As described in our [knowledge base](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/):

> *Mutual TLS, or mTLS for short, is a method for [mutual authentication](https://www.cloudflare.com/learning/access-management/what-is-mutual-authentication/). mTLS ensures that the parties at each end of a network connection are who they claim to be by verifying that they both have the correct private [key](https://www.cloudflare.com/learning/ssl/what-is-a-cryptographic-key/). The information within their respective [TLS certificates](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/) provides additional verification.*

While mTLS seems like a good fit for bot authentication on the web, it has limitations. If a user is asked for authentication via the mTLS protocol but does not have a certificate to provide, they would get an inscrutable and unskippable error. Origin sites need a way to conditionally signal to clients that they accept or require mTLS authentication, so that only mTLS-enabled clients use it.

### A TLS flag for bot authentication

TLS flags are an efficient way to describe whether a feature, like mTLS, is supported by origin sites. Within the IETF, we have proposed a new TLS flag called [`req mTLS`](https://datatracker.ietf.org/doc/draft-jhoyla-req-mtls-flag/) to be sent by the client during the establishment of a connection that signals support for authentication via a client certificate.

This proposal leverages the [tls-flags](https://www.ietf.org/archive/id/draft-ietf-tls-tlsflags-14.html) proposal under discussion in the IETF. The TLS Flags draft allows clients and servers to send an array of one bit flags to each other, rather than creating a new extension (with its associated overhead) for each piece of information they want to share. This is one of the first uses of this extension, and we hope that by using it here we can help drive adoption.

When a client sends the [`req mTLS`](https://datatracker.ietf.org/doc/draft-jhoyla-req-mtls-flag/) flag to the server, they signal to the server that they are able to respond with a certificate if requested. The server can then safely request a certificate without risk of blocking ordinary user traffic, because ordinary users will never set this flag.

Let’s take a look at what an example of such a req mTLS would look like in [Wireshark](https://www.wireshark.org/), a network protocol analyser. You can follow along in the packet capture [here](https://github.com/cloudflareresearch/req-mtls/tree/main/assets/demonstration-capture.pcapng).

```
Extension: req mTLS (len=12)
    Type: req mTLS (65025)
    Length: 12
    Data: 0b0000000000000000000001
```

The extension number is 65025, or 0xfe01. This corresponds to an unassigned block of [TLS extensions](https://www.iana.org/assignments/tls-extensiontype-values/tls-extensiontype-values.xhtml#tls-extensiontype-values-1) that can be used to experiment with TLS Flags. Once the standard is adopted and published by the IETF, the number would be fixed. To use the `req mTLS` flag the client needs to set the 80 <sup>th</sup> bit to true, so with our block length of 12 bytes, it should contain the data 0b0000000000000000000001, which is the case here. The server then responds with a certificate request, and the request follows its course.

### Request mTLS in action

*Code for this section is available in GitHub under [cloudflareresearch/req-mtls](https://github.com/cloudflareresearch/req-mtls)*

Because mutual TLS is widely supported in TLS libraries already, the parts we need to introduce to the client and server are:

1. Sending/parsing of TLS-flags
2. Specific support for the `req mTLS` flag

To the best of our knowledge, there is no complete public implementation of either scheme. Using it for bot authentication may provide a motivation to do so.

Using [our experimental fork of Go](https://github.com/cloudflare/go), a TLS client could support req mTLS as follows:

```
config := &tls.Config{
        TLSFlagsSupported:  []tls.TLSFlag{0x50},
        RootCAs:           rootPool,
        Certificates:      certs,
        NextProtos:        []string{"h2"},
}
trans := http.Transport{TLSClientConfig: config, ForceAttemptHTTP2: true}
```

This example library allows you to configure Go to send `req mTLS 0x50` bytes in the `TLS Flags` extension. If you’d like to test your implementation out, you can prompt your client for certificates against [req-mtls.research.cloudflare.com](http://req-mtls.research.cloudflare.com/) using the Cloudflare Research client [cloudflareresearch/req-mtls](https://github.com/cloudflareresearch/req-mtls). For clients, once they set the TLS Flags associated with `req mTLS`, they are done. The code section taking care of normal mTLS will take over at that point, with no need to implement something new.

## Two approaches, one goal

We believe that developers of agents and bots should have a public, standard way to authenticate themselves to CDNs and website hosting platforms, regardless of the technology they use or provider they choose. At a high level, both HTTP Message Signatures and request mTLS achieve a similar goal: they allow the owner of a service to authentically identify themselves to a website. That’s why we’re participating in the standardizing effort for both of these protocols at the IETF, where many other authentication mechanisms we’ve discussed here — from TLS to OAuth Bearer tokens –— been developed by diverse sets of stakeholders and standardized as RFCs.

Evaluating both proposals against each other, we’re prioritizing [HTTP Message Signatures for Bots](https://datatracker.ietf.org/doc/html/draft-meunier-web-bot-auth-architecture) because it relies on the previously adopted [RFC 9421](https://datatracker.ietf.org/doc/html/rfc9421) with several [reference implementations](https://httpsig.org/), and works at the HTTP layer, making adoption simpler. [request mTLS](https://datatracker.ietf.org/doc/draft-jhoyla-req-mtls-flag/) may be a better fit for site owners with concerns about the additional bandwidth, but [TLS Flags](https://datatracker.ietf.org/doc/html/draft-ietf-tls-tlsflags) has fewer implementations, is still waiting for IETF adoption, and upgrading the TLS stack has proven to be more challenging than with HTTP. Both approaches share similar discovery and key management concerns, as highlighted in a [glossary](https://datatracker.ietf.org/doc/draft-meunier-web-bot-auth-glossary/) draft at the IETF. We’re actively exploring both options, and would love to [hear](https://www.cloudflare.com/lp/verified-bots/) from both site owners and bot developers about how you’re evaluating their respective tradeoffs.

## The bigger picture

In conclusion, we think request signatures and mTLS are promising mechanisms for bot owners and developers of AI agents to authenticate themselves in a tamper-proof manner, forging a path forward that doesn’t rely on ever-changing IP address ranges or spoofable headers such as `User-Agent`. This authentication can be consumed by Cloudflare when acting as a reverse proxy, or directly by site owners on their own infrastructure. This means that as a bot owner, you can now go to content creators and discuss crawling agreements, with as much granularity as the number of bots you have. You can start implementing these solutions today and test them against the research websites we’ve provided in this post.

Bot authentication also empowers site owners small and large to have more control over the traffic they allow, empowering them to continue to serve content on the public Internet while monitoring automated requests. Longer term, we will integrate these authentication mechanisms into our [AI Audit](https://blog.cloudflare.com/cloudflare-ai-audit-control-ai-content-crawlers/) and [Bot Management](https://developers.cloudflare.com/bots/get-started/bot-management/) products, to provide better visibility into the bots and agents that are willing to identify themselves.

Being able to solve problems for both origins and clients is key to helping build a better Internet, and we think identification of automated traffic is a step towards that. If you want us to start verifying your message signatures or client certificates, have a compelling use case you’d like us to consider, or any questions, please [reach out](https://www.cloudflare.com/lp/verified-bots/).