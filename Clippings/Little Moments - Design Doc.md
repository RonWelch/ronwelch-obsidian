---
title: "Little Moments - Design Doc"
source: "https://refactoringenglish.com/excerpts/write-an-effective-design-doc/little-moments-design-doc/"
author:
published:
created: 2026-06-24
description:
tags:
  - "clippings"
---
## Little Moments Design Doc

## Metadata

- **Author**: Michael Lynch (michael@mtlynch.io)
- **Status**: Design-complete, ready for implementation
- **Created**: 2026-03-06
- **URL**: [https://codeberg.org/mtlynch/little-moments/src/branch/master/docs/design/DESIGN.md](https://codeberg.org/mtlynch/little-moments/src/branch/master/docs/design/DESIGN.md)

## Contents

## Objective

Create a web app that allows parents to share photos and videos of their children with close friends and family members.

## Background

I’m a paying customer of TinyBeans, a popular app for sharing family photos.

I subscribed to TinyBeans because they advertise it as privacy-friendly and ad-free, but after I signed up, I discovered that TinyBeans [still injects ads into your photo albums and spams paying customers with ads](https://mtlynch.io/retrospectives/2025/12/#building-a-free-tinybeans-alternative-out-of-spite).

I find it gross that a service for sharing baby photos injects ads into my family photos and collects ad-targeting information about my friends and family, especially when I pay for my usage.

I want to build an alternative photo sharing app that’s ad-free and easy for tech-savvy users to host for themselves privately.

## Goals

- Share family baby photos and videos with relatives and close friends.
- Minimize complexity so that less computer-literate relatives are comfortable using the app.
- Keep all data private and only accessible to authorized users.
- Keep high enough limits on photo / video size that parents perceive it as essentially unlimited storage.
	- Apply some upper limits for security reasons.
- Keep app free of all tracking / advertising.
- Allow users to control their level of engagement with the app.
	- They can get email summaries of activity daily.
		- They can’t get real-time updates because the app should feel slow and calm, not constantly pinging them.
		- Users can also turn off notifications entirely and only engage with the app when they proactively sign in.
- Migrate all data from TinyBeans (including media, comments, users, preferences).
- Keep the cost of hosting (including servers, storage, and dependent services) below US$10/month per single-family server.
- Minimize the complexity of hosting, so other tech-savvy users can host TinyBeans servers for their families.
- Make the migration from TinyBeans feel seamless for family members.
- Minimize v1 implementation cost.
	- A v1 implementation of this app should take less than 40 dev hours.

## Non-goals

- Create a commercial photo-sharing application.
	- Hosting other people’s private photos has too many catastrophic risks.
- Preserve URL links for past TinyBeans uploads.
	- I’d like to do it, but I can’t influence old TinyBeans URLs to add redirects.
- Allow multiple families to share the same Little Moments server.
	- The app assumes the server is for a single family.
		- There’s no concept of “multi-tenant” where multiple families share the same server but have privacy from one another.
- Create native mobile apps.
	- The cost of dealing with app stores is too high.
		- I’m not proficient in any technologies that compile cross-platform apps (e.g., Flutter, React Native)

## User roles

There are two types of Little Moments users with different permissions:

| Privilege | Subscriber | Owner |
| --- | --- | --- |
| Read photos and videos | ✅ | ✅ |
| Upload photos and videos | ❌ | ✅ |
| Download photos and videos | ✅ | ✅ |
| Delete photos and videos | ❌ | ✅ |
| Leave comments and emoji reactions | ✅ | ✅ |
| Edit / delete own comments and reactions | ✅ | ✅ |
| Edit / delete anyone’s comments and reactions | ❌ | ✅ |
| Invite users to join | ❌ | ✅ |
| Modify permission levels | ❌ | ✅ |

My wife and I will each have **Owner** permissions. Everyone else we invite will have **Subscriber** permissions.

Photos and videos do not have fine-grained permission levels. All photos are visible to all users.

Comments do not have fine-grained permission levels. All comments are visible to all users.

## User interface

### Browse media

![](https://refactoringenglish.com/excerpts/write-an-effective-design-doc/little-moments-design-doc/docs/design/browse.webp)

The photos show as a grid of square thumbnails flowing left to right, top to bottom.

There is no pagination. The page loads all thumbnails on the initial page load, though images are lazy-loaded to avoid downloading all thumbnails immediately.

### View individual photo / video

![](https://refactoringenglish.com/excerpts/write-an-effective-design-doc/little-moments-design-doc/docs/design/view-image.webp)

On mobile, the user can swipe right and left to view the previous and next photo, respectively.

On desktop, the user can click next or previous to get to the next photo.

All users can download the photo at full resolution.

### Upload media

For owners, the home page has an upload button that is the main call-to-action on the screen. The “Upload” button should always be visible in the navbar on both desktop and mobile views. On mobile, even when the navbar is collapsed, the “Upload” button should still be visible outside of any tap-to-expand area.

After the user uploads a photo, they have the option to either add a caption or publish the photo without a caption. When the user publishes the photo, it’s visible to other users and queued for mention in the next email announcement.

All users can add comments to a post. The comments may include text and emoji.

The app does not have comment subthreads, so all comments are at the same level.

The app does not maintain a comment history (see [Data retention](#data-retention)).

The user who added the comment can edit or delete their comments. Owners can edit or delete any comment.

### Add reaction

The user can post a comment or react with one of these emoji: ♥️ 😂 🎉 🙀 🤨

Users can only add a single reaction to each media item.

Emoji reactions appear directly below the image, one per line of:

- `<emoji> <username>, <relative time>`

For example:

- `😂 michael, 2 weeks ago`

The user who added the comment can edit or delete their reactions. Owners can edit or delete any reaction.

## Scenarios

### Parent adds a photo

1. The parent opens the web app.
2. The main eye-catching UI element on the main page (the CTA) is an “Upload” button, which the parent clicks.
3. The parent selects a photo to upload.
4. The app prompts the parent to optionally add a caption.
5. The parent clicks “Publish” to publish the photo.
6. The parent sees the photo on the view media interface, as other users will see it.
7. The next morning at 9AM ET, subscribers to the Little Moments server receive an email that contains the photo and a link to view it on the web app.

### Parent adds a video

1. The parent opens the web app.
2. The main eye-catching UI element on the main page (the CTA) is an “Upload” button, which the parent clicks.
3. The parent selects a video to upload.
4. The app prompts the parent to optionally add a caption while the video is uploading.
5. The parent clicks “Publish” to publish the video.
6. The parent sees the video on the view media interface, as other users will see it.
	- While the app transcodes the video, the parent sees a notice that says, “Video processing in progress. This video will be visible to other users in a few minutes.”
		- If they reload the page after transcoding completes, they’ll see the video available, and it will be visible to other users.
7. The next morning at 9AM ET, subscribers to the Little Moments server receive an email that contains the video thumbnail and a link to view the video.

### Subscriber comments on photo

1. A family member receives an email notifying them of a new Little Moments photo.
2. The family member clicks the link to comment on the photo.
3. The family member writes a comment on the photo and hits the “Publish” button.
4. The next morning at 9AM ET, parents on the Little Moments server receive an email that contains the comment. Anyone else who commented on the same photo also receives the email.

### Import data from TinyBeans

Little Moments uses data from [tinybeans-export](https://codeberg.org/mtlynch/tinybeans-export) to populate the initial set of data (see [appendix](#appendix-tinybeans-export-format)). A parent would only do this once when initially setting up their Little Moments server. This is an operation that happens before the server is live and serving user requests.

1. Parent runs `tinybeans-export` on their current TinyBeans account.
2. tinybeans-export extracts photos, videos, and JSON metadata files (including captions, timestamps, and comments) from TinyBeans.
3. Parent manually runs a script in the source repo called `./dev-scripts/import-from-tinybeans` that translates the data to Little Moments and populates the local data store.
	- For simplicity, `import-from-tinybeans` requires the local data store to be empty.
4. Little Moments migrates all users specified in the `tinybeans-export` data to the Little Moments app as registered users
	- Users with relationship `MOTHER` or `FATHER` in `tinybeans-export` have parent access by default (see [User roles](#user-roles)).
		- Everyone else has Subscriber access.
5. Little Moments displays all users, photos, videos, captions, and comments from the parents’ TinyBeans account.
6. Users from `tinybeans-export` data can access Little Moments without having to sign up with Little Moments beyond requesting a magic login link.

Little Moments’ data model is simpler than TinyBeans, so the importer will simplify some TinyBeans data to fit Little Moments. In particular:

- Treat a subscriber’s email preference as “opt-out” if:
	- `user.emailOptOut` is `true` (they’ve opted out globally) OR
		- `user.emailFrequencyOnNewComment.name` is `NONE` AND `user.emailFrequencyOnNewEmotion.name` is `NONE` and `emailFrequencyOnNewEvent` is `NONE` (they’ve opted out of all other types of emails)

## Missing features

For simplicity, I’m deliberately omitting these features from the v1 implementation:

- Users can’t reply to comments via email.
	- Notifications will come from a `noreply@` email address.
- Users can’t set profile photos / avatar images.
	- TinyBeans offers this, but nobody in my family has ever used this.
- Users can’t set a date on a photo manually.
	- We’ll infer the date from media metadata or filename and default to the upload date.
- Only parents can upload photos.
	- I never allowed anyone but parents to add media on TinyBeans.
- No calendar view.
	- We won’t display photos on a calendar.
- No support for albums.
	- There’s no special grouping for photos. They just all appear in the same photostream.
- No support for per-item privacy settings.
	- Little Moments supports parent-only privacy on media items. I never used this feature.
- No support for specifying photo subjects.
	- TinyBeans offers this, but this is not a feature I’ve ever found useful.
		- Similarly, we don’t allow the users to catalog their pets.
- No localization to other languages or locales.
	- No support for changing the notification email time or time zone.
- No progressive web app (PWA) support.
	- I don’t see much value in PWA over a basic web app, but if users to do more offline, it might be a potential feature in the future.

## Users

Assumptions I’m making about Little Moments users:

- They use modern web browsers.
	- They’re using browser versions released after January 1, 2025.
- They can read English.
- They live in the US, but in different timezones.
- They use desktops, tablets, and mobile phones.
- They use MacOS, Windows, Linux, Android, and iOS operating systems.
- They use Chrome, Safari, Edge, and Firefox as their web browsers.
- Some users have limited vision, but they all have enough vision to read text and view images.
	- Some users may be using device-wide settings to increase text size or zoom on their devices.

## Notifications

By default, users receive daily email notifications when there is new activity on Little Moments. The app sends summary emails out every day at 9 AM ET.

### Managing notifications

All notification emails contain a one-click unsubscribe link. Users do not need to sign in to unsubscribe from email updates.

The app also has a user preferences page that allows the user to disable email notifications.

### Notification contents

The notification email summarizes all user activity in the last day that’s relevant to the recipient.

Notifications to parents (owners) summarize all activity, including:

- All new uploads
- All new comments
- All new emoji reactions

Notifications to subscribers (non-parents) are limited to:

- All new uploads
- Comments in threads they’ve participated (for simplicity, this includes the user’s own comments, unless that user is the only comment in the thread)
- No emoji reactions

Photo/video thumbnails are included from earliest to latest by upload time. Clicking a photo leads to the page on Little Moments.

Thumbnails are embedded images and not email attachments. They point to S3-style URLs that do not expire.

The app does not send users notification emails when there is no new activity to report to them.

#### Example email (parents)

```
Hi Ned,

Here are today's Little Moments updates:

Marge added a new photo

<photo 1 thumbnail>

Homer added a new video

- Uncey Herb: What a sharp outfit!
- 🤨 Patty

<video 1 thumbnail>

There was new activity on this upload from June 3rd:

<photo 2 thumbnail>

- Grandma: He gets cuter every day
- Grandpa: I agree!
- 😂 Moe
- ♥️ Maude

There was new activity on this upload from June 8th:

<video 2 thumbnail>

- Maude: How precious!
- ♥️ Maude
```

## Service level objectives (SLOs)

### Uptime

Target: 99% availability (two nines)

This allows up to 3.65 days of outages per year.

This is not a critical app that must be online at all times, but it should be online enough that family members can rely on it to view photos.

### Latency

All thumbnails within the viewport should load in under 400ms on a desktop with a 1 Gbps network connection.

### Scale

#### Users

The app should support up to 50 users with up to 15 users accessing the site simultaneously.

It likely can scale higher, but this is all it needs to achieve for the audience I expect.

#### Media sizes

The app supports photos up to 50 Megapixels and up to 50 MB each.

The app supports videos up to 8K resolution, up to 5 GB in size, and up to 30 minutes of duration.

## Architecture

![](https://refactoringenglish.com/excerpts/write-an-effective-design-doc/little-moments-design-doc/docs/design/architecture/architecture.svg)

### Backend language: Go

Go is the language I’m most comfortable in, and I find it especially good for writing backend-heavy web apps.

### Frontend language: Vanilla HTML5 / JavaScript / CSS

Vanilla HTML5, JavaScript, and CSS is the web stack I know best and feel most comfortable in. I might use htmx if it fits.

Rendering will happen server side as much as possible, as I find this simpler to reason about and easier to test. There isn’t much functionality in the app that requires the responsiveness client-side rendering provides. If a user comments or reacts to an image, it’s not important that we render the comment in real time on every other user’s screen. It’s fine to wait until the next reload or page navigation to show it.

See [Alternative frontend stacks considered](#alternative-frontend-stacks).

### Database: SQLite

SQLite minimizes cost and complexity of hosting because it runs within the app’s process rather than as a separate process or host.

The downside of SQLite is that it doesn’t support strong types like Postgres does, but I think the tradeoff is worthwhile. SQLite is harder to scale to millions of users, but we only need to scale to tens of users, so SQLite will handle it fine.

### Video conversion: ffmpeg

Little Moments’ only computationally expensive task is re-encoding video to play natively in the browser and stripping metadata. It will use ffmpeg to re-encode videos.

I considered using external video encoding services, but I decided to just use ffmpeg within the same app container (see [“Closed Issue: Video encoding.”](#video-encoding))

The app will also use ffmpeg to extract thumbnails from videos.

### Hosting: fly.io

The service will run on fly.io.

I have the most hosting experience with Fly.io. Most of my services run there. I like their simplicity.

Nothing in the app depends on fly.io specifically. Other owners deploying Little Moments can easily swap out fly.io for another host, especially hosts that support Go or Docker containers.

### Media storage and serving: Bunny

Bunny has low prices and a simple interface. They recently added support for storage via S3 APIs, which makes it easy to switch vendors if I don’t like their storage experience.

The next best choice is Backblaze, though they don’t support custom domains, so if I wanted custom domains, I’d need to put a CDN like Bunny or Cloudflare in front of Backblaze anyway, and using a single vendor for storage and CDN simplifies things.

### Email delivery: Sendamatic

The app will use Sendamatic to send notification and magic login emails.

The app does not depend on any Sendamatic-specific features. It will only use Sendamatic’s SMTP interface. Other owners deploying Little Moments can easily swap out this dependency for any vendor that supports SMTP access.

See closed issue [“SMTP Vendor.”](#smtp-vendor)

### Database replication: Litestream + Backblaze B2

I’ve used Litestream and Backblaze B2 for all my web apps for the last four years. It’s reliable, simple, and cost-effective.

I would use Bunny for this, as I’m already using them for media serving, but Bunny’s S3 implementation currently does not support Batch delete or ETag for HeadObject, both of which Litestream 0.5.x depend on.

### Continuous Integration: NixCI

NixCI is the only production-grade, managed CI vendor compatible with Codeberg.

### Monitoring / alerting: Cronitor

Little Moments itself will be monitoring-agnostic.

I’ll add basic liveness checks on my personal Little Moments server with Cronitor. Cronitor is the monitoring tool I use for other apps.

I will set a basic monitor to check the login page every hour to check that the app server is still online and reachable. If three consecutive checks fail, Cronitor will send me an email alert.

### Source hosting: Codeberg

I’ve been using them for the last year, and I like them. They’re user-owned and open-source.

Most open-source developers don’t have Codeberg accounts, so there’s additional friction to participating there, but I don’t mind that, and I’d prefer to stop centralizing everything around the dominant git forge.

### Job scheduling: Roll my own

I need a way to manage scheduled jobs within the app for sending notifications and converting media in the background. If a user subscribes to daily summaries, we need a way to look at all the events that have occurred since we last emailed them and include it in the summary. We also need to persist results of the job to SQLite so that we avoid a situation where we’re accidentally emailing a subscriber hundreds of times for the same job.

Options considered:

- [gocron](https://github.com/go-co-op/gocron): A bit more complex than I need but seems to support simple needs. Doesn’t have [persistence](https://github.com/go-co-op/gocron/issues/533).
- [go-quartz](https://github.com/reugn/go-quartz): Doesn’t seem to have persistence.
- [Dagu](https://dagu.cloud/): Seems to be too complex and is a standalone server rather than a Go module.

### Style

The app will use Bootstrap 5 as the base CSS library. It’s a CSS library I know and use regularly.

### Icons

The app will use [Lucide](https://lucide.dev/) icons.

I’ve never used Lucide, but it’s free and open-source, and the design matches the aesthetic I have in mind for Little Moments.

See [Alternative icon libraries considered](#alternative-icon-libraries).

## Privacy

All photos and videos are treated as private and should not be visible to anyone who is not an authenticated user.

### Exif metadata

Photos contain metadata called [Exif data](https://en.wikipedia.org/wiki/Exif) that can include the time the photo was taken, details about the camera that took the photo, and GPS coordinates.

Exif metadata is a privacy issue because the information is in the photo but its presence is not obvious to the average user. There are [famous incidents](https://www.vice.com/en/article/on-the-lam-with-john-mcafee/) where people catastrophically leaked information through Exif data by mistake.

Video files can also contain sensitive metadata, so we need to avoid leaking information in videos.

To prevent accidental privacy leaks of metadata, the app will strip metadata and generate random filenames for all photos and videos that parents upload before presenting it to other users.

The app will store the original uploaded media and filenames as-is, but in v1, it will not offer ways for parents to access this data or download the non-sanitized version through the web UI. Owners can get it by inspecting the filesystem and database.

## Data retention

The app does not maintain change history for any user-editable fields. When a user changes a comment or a media caption, it overwrites the previous caption.

All deletes are hard deletes that delete the underlying data and clear associated database entries. For example, deleting a photo also deletes all comments associated with that photo. Data persists in Litestream backups for the duration of Litestream’s retention period.

The retention policy on Litestream SQLite backups is:

- Snapshot interval: Every 60 minutes
- Data retention: 2400h (100 days)

## Security

### Attack surface

We do not consider any attacks that require arbitrary code execution with Owner permissions. It is assumed that if the Owner achieves arbitrary code execution from an Owner account, it’s game over.

#### Login page

The login page is a target for attacks because it is unauthenticated.

Associated threats:

- [Creating spam with email login](#creating-spam-with-email-login)
- [Exfiltrating email addresses through brute force](#exfiltrating-email-addresses-through-brute-force)

#### S3 bucket

One way for an attacker to exfiltrate data is to bypass the app and read data from the S3 bucket directly.

Associated threats:

- [Leaks via email forwarding](#unauthorized-attacker-discovers-photos-in-s3-bucket)

### Authentication

Users will authenticate through magic login links over email. There are no username/password credentials. There is no multi-factor authentication.

The only way for new users to get access to the site is to receive an invitation from a parent. There is no self-serve way for anonymous users to sign up on a server or request access unless their email address is already registered.

Once a user authenticates with a magic login link, the session stays valid for five years. The server stores a session expiration time, and the user’s browser stores an authentication cookie with a 5-year lifetime. Users may invalidate their session token by clicking a “Log out” button from the navbar, which requests token invalidation from the server and deletes the authentication cookie.

The authentication strategy is partly to accommodate less technical users who might lose/forget their credentials. It minimizes implementation cost, as email-based authentication is easier to implement than username/password.

### Threats

#### Unauthorized attacker discovers photos in S3 bucket

Scenario: An authorized family member forwards an email or accidentally links to private media in a public place. An attacker sees a single image URL and is able to infer its S3 host, S3 bucket name, and path.

- Bucket listing will be disabled
- We’ll use unpredictable filenames
- We won’t expose the bucket names publicly, though I believe the attacker has ways of discovering it.
- We’ll use a subdirectory in the bucket
- Recipient cannot access the web app without logging in.
- The recipient can leak the URL of a single image, but receiving an email forward shouldn’t give a malicious user enough information to enumerate all media.
	- e.g., if an attacker sees a valid link to `media.littlemoments.example.com/IMG_0001.jpg`, they can’t explore other images by guessing filenames like `IMG_0002.jpg`.

Mitigations:

- Storing media in a Public Backblaze B2 bucket but rename filenames to be UUIDv4 values.
- Keep the S3 bucket name in secret environment variables.
- Use Backblaze B2, which requires authorization for listing bucket contents by default.

See [Alternative media access control options considered](#alternative-media-access-control).

#### Unauthorized attacker discovers database backups in S3 bucket

Scenario: An attacker discovers the S3 bucket for metadata and enumerates contents to exfiltrate private metadata in our Litestream backups of the SQLite database.

Litestream’s backups have predictable filenames, which means that if an attacker knows the bucket name and path, they can identify the S3 URL to the backup files.

Mitigations:

- Store SQLite backup in a dedicated Backblaze B2 bucket set to Private.
- Keep the S3 bucket name in secret environment variables.
- Keep the Litestream backup path in a secret environment variable.

#### Scraper Bots DoS the site

Scenario: Scraper bots exhaust server resources by sending thousands of requests to the server from different residential IP addresses with the expectation that some data on the site will be useful.

There are frequent reports of automated scraper bots indiscriminately hammering sites with traffic.

The only publicly-accessible page will be the login page, and that’s cheap to render.

Mitigations:

- Keep publicly accessible pages (landing page and login page) inexpensive to render.
- (if it becomes an issue) Add a rate limit per IPv4 address, similar to what [LogPaste does](https://github.com/mtlynch/logpaste/blob/20750ec29e52502af449f5d462d51ac864738104/limit/limit.go).

#### Creating spam with email login

With email-based login, an unauthorized user can trigger the app to send a login to an email address associated with an authenticated user.

Mitigations:

- Rate limit password resets to 2 per email address and 4 per unique IPv4 in the trailing 24 hour window.
	- See [a similar implementation in ScreenJournal](https://github.com/mtlynch/screenjournal/blob/df0fcbc77573f89e0a6d02d27e4242e840cb5bce/ratelimit/ratelimit.go).

#### User accidentally forwards their magic login to an attacker

Scenario: A user accidentally forwards their magic login link to another person.

Mitigations:

- Expire the magic login URL after first use or 30 minutes (whichever is first).
- The magic login link warns users not to forward it to anyone.
- Don’t include anything in the login link email that would tempt users to forward it (e.g., cute photos)

Don’t try to defend against this so much because it’s unlikely and most potential mitigations severely impact user experience. If this happens, the site owner could manually delete the compromised user’s session tokens from the database, but if a malicious user took over the account, we should assume the attacker can download everything before a site owner can react and revoke access.

#### External site crafts malicious requests (CSRF)

Scenario: An attacker tricks a victim user into visiting a malicious website. The website includes JavaScript or HTML that performs actions on Little Moments with the victim’s session token. This is a common attack known as [cross-site request forgery (CSRF)](https://en.wikipedia.org/wiki/Cross-site_request_forgery).

Mitigations:

- Don’t allow any GET requests to change server state
- Set session token cookie to `SameSite: Lax`
	- I considered `Strict` but it means clicks from email clients will lead to a logged out page, which is annoying, and it’s easy to just not do any state changes on GET.
		- I’m relying on users to have browsers recent enough to [support SameSite](https://caniuse.com/same-site-cookie-attribute).

#### External site secretly embeds app (Clickjacking)

Scenario: An attacker creates a site that secretly embeds Little Moments in an iframe but disguises it to trick a victim user into clicking an unintended button on Little Moments. This is an attack known as [clickjacking](https://en.wikipedia.org/wiki/Clickjacking).

Mitigations:

- Set HTTP header of `Content-Security-Policy: frame-ancestors 'none'`
- Set HTTP header of `X-Frame-Options: DENY`
	- If the browser is [new enough to support `frame-ancestors`](https://caniuse.com/mdn-http_headers_content-security-policy_frame-ancestors), it will use that. Otherwise, it will fall back to legacy `X-Frame-Options`.

This is an extremely unlikely attack given the size of the expected userbase, so this isn’t something I’m especially worried about. I also don’t expect to use `iframe` s for any purpose.

#### Exfiltrating email addresses through brute force

Scenario: An attacker wants to discover the email addresses of app users, so they keep guessing email addresses on the login page in the hopes that the login response indicates whether the email belongs to a registered user.

Mitigations:

- When a user requests a magic login link, don’t announce whether or not the email they requested is associated with a valid user.

A determined attacker can likely discover email validity via [a timing attack](https://en.wikipedia.org/wiki/Timing_attack), but it’s too unlikely an attack to defend against. With 20ish users per server, it’s not attractive for an attacker to discover emails this way.

#### Registered user exploits site with malicious inputs

As with all web apps, there are risks from users submitting malicious data in input fields that have unintended effects on the app’s behavior (e.g., SQL injection, cross-site scripting).

These risks are unlikely in our app, as all users are trusted family members who are unlikely to attack each other. There’s also a small chance that an attacker compromises a subscriber account and exploits vulnerabilities to elevate their privileges, but that’s also unlikely for this app.

Still, we will mitigate risks of malicious inputs as a matter of good practice.

Mitigations:

- Render user-supplied content in Go HTML templates, which are good at encoding everything to proper HTML
- Use strict Content Security Policy to prevent inline JavaScript and CSS
- Reject HTML tags in comments
- Use parameterized SQLite queries to prevent SQL injection

## Licensing

Little Moments will use the [PolyForm-Noncommercial](https://github.com/polyformproject/polyform-licenses/blob/1.0.0/PolyForm-Noncommercial-1.0.0.md). Users are welcome to submit code changes, but their contributions will under the [Zero-Clause BSD License](https://opensource.org/license/0bsd). Inspiration for this license setup [comes from Komorebi](https://github.com/LGUG2Z/komorebi/blob/master/CONTRIBUTING.md).

See [Alternative software licenses considered](#alternative-software-licenses).

### Considerations

- I don’t want the license to interfere with people using Little Moments for non-commercial purposes (e.g., hosting a server, customizing it, and redistributing their changes).
- I probably [won’t turn Little Moments into a commercial application](#non-goals), but I also don’t want to restrict myself from that path.
- I don’t want anyone to monetize my work on Little Moments in their own commercial product without negotiating an agreement with me for a paid license.
- In my other open-source projects, even popular ones, I get a fairly small number of contributions, so even if a restrictive license discourages contributions, the downside is relatively small.

## Implementation timeline

My strategy for ordering tasks is primarily to serve these two goals:

- Maximize ROI
	- Given the work required, what subsequent feature provides the most value?
		- For example, I would never make the first milestone just designing SQL database schemas because that has no value on its own. I would rather make a real web app that serves four hardcoded media files with no database.
		- Mitchell Hashimoto articulates this strategy in [“My Approach to Building Large Technical Projects.”](https://mitchellh.com/writing/building-large-technical-projects)
- Minimize waste
	- I’m willing to add some temporary scaffolding that exists purely to support early development, but I want to minimize how much work is dev-only or temporary throwaway code.

### Milestone 1: Read-only TinyBeans mirror

Parent can import their data from TinyBeans and host it on a test server that isn’t exposed to the Internet.

What’s implemented:

- **View photos and videos imported from TinyBeans**
- **View comments imported from TinyBeans**

What’s not implemented:

- SQLite database integration (use in-memory datastores and ad-hoc files to store state)
- Cloud storage for media files
- User authentication
- Live, web-accessible server
- Upload new photos and videos
- Add comments / reactions to media
- Email notifications
- Database replication

### Milestone 2: Add real database

Switch from the in-memory / file-based data store to a proper SQLite database. There’s not much user-visible impact here, but I accrue more technical debt the longer I defer a real database.

What’s implemented:

- **SQLite database integration**
- View photos and videos imported from TinyBeans
- View comments imported from TinyBeans

What’s not implemented:

- Cloud storage for media files
- User authentication
- Live, web-accessible server
- Upload new photos and videos
- Add comments / reactions to media
- Email notifications
- Database replication

### Milestone 3: Use cloud storage

Parent can import their data from TinyBeans to cloud storage to see what performance will feel when images are not local.

What’s implemented:

- **Cloud storage for media files**
- View photos and videos imported from TinyBeans
- View comments imported from TinyBeans
- SQLite database integration

What’s not implemented:

- User authentication
- Live, web-accessible server
- Upload new photos and videos
- Add comments / reactions to media
- Email notifications
- Database replication

### Milestone 4: User authentication

Users can sign in using real magic login links that they receive via email.

What’s implemented:

- **User authentication**
- View photos and videos imported from TinyBeans
- View comments imported from TinyBeans
- SQLite database integration
- Cloud storage for media files

What’s not implemented:

- Live, web-accessible server
- Upload new photos and videos
- Add comments / reactions to media
- Email notifications
- Database replication

### Milestone 5: Deployment to the web

The app is deployed to a real, web accessible URL protected with email magic link authentication. Family members can preview the app in read-only mode.

What’s implemented:

- **Live, web-accessible server**
- **Database replication**
- View photos and videos imported from TinyBeans
- View comments imported from TinyBeans
- SQLite database integration
- Cloud storage for media files
- User authentication

What’s not implemented:

- Upload new photos and videos
- Add comments / reactions to media
- Email notifications

### Milestone 6: Media uploads

Parent can upload new photos and videos.

What’s implemented:

- **Upload new photos and videos**
- View photos and videos imported from TinyBeans
- View comments imported from TinyBeans
- SQLite database integration
- Cloud storage for media files
- User authentication
- Live, web-accessible server
- Database replication

What’s not implemented:

- Add comments / reactions to media
- Email notifications

### Milestone 7: User interaction

Users can leave comments and reactions to photos and videos.

What’s implemented:

- **Add comments / reactions to media**
- View photos and videos imported from TinyBeans
- View comments imported from TinyBeans
- SQLite database integration
- Cloud storage for media files
- User authentication
- Live, web-accessible server
- Upload new photos and videos
- Database replication

What’s not implemented:

- Email notifications

### Milestone 8: Full functionality

The app is deployed to a real, web accessible URL and has full functionality. Parents can upload photos and videos, and their families can leave comments, reactions, and receive email updates.

What’s implemented:

- **Email notifications**
- Database replication
- View photos and videos imported from TinyBeans
- View comments imported from TinyBeans
- SQLite database integration
- Cloud storage for media files
- User authentication
- Live, web-accessible server
- Upload new photos and videos
- Add comments / reactions to media

## Open issues

- None

## Closed issues

### SMTP vendor

**Decision**: Use Sendamatic. If they’re a poor match, use Amazon SES. From the code side, changing SMTP vendors is a trivial change, as it’s just modifying a few environment variables.

Criteria:

- Supports SMTP interface (standard interface, limits vendor lock-in).
- Allows at least 500 emails per month.
- Minimizes costs.
- Doesn’t inject ads into messages.

Candidates (in descending order of appeal):

- [Sendamatic](https://www.sendamatic.net/pricing)
	- Pro: Pay as you go ($0.000088/email)
		- Seems to be a one-person shop
		- Tried on 2026-01-21
		- Good: Simple interface
				- Good: Activation email came through without spam filter flagging it
				- Bad: Seems like I have to wait up to 2 days for manual verification before I can even send test emails
				- Good: Actual manual activation was in about 12 hours
				- Good: Founder is responsive to emails
				- Bad: Web dashboard doesn’t have much information, though you can download a report of raw logs.
			- The developer says they’re working on improving this
- [Amazon SES](https://aws.amazon.com/ses/pricing/)
	- Pro: Basically free at my usage level ($0.0001/email)
		- Con: Need to jump through annoying Amazon hoops to get verified.
		- Con: Don’t like giving money to Amazon.
- [Mailersend](https://www.mailersend.com/pricing?currency=usd&billing=monthly)
	- $7/mo for 5k emails
- [Resend](https://resend.com/pricing?product=transactional)
	- Pro: 3,000 emails/month under free tier
		- Con: Don’t want to rely on a free tier
		- Con: Next cheapest tier is $20/mo
- [Mailgun](https://www.mailgun.com/pricing/)
	- Pro: 100 emails/day under the free tier
		- Con: Don’t want to rely on a free tier
		- Con: Owned by Twilio, who I hear negative things about
- [SendGrid](https://sendgrid.com/en-us/pricing)
	- Seems to be the exact same thing as Mailgun, as Twilio bought both companies.

Definitely not:

- Postmark
	- Loved them when they were indie, now owned by spam company.
		- Minimum paid tier is $15/mo.
- [Mailjet](https://www.mailjet.com/pricing/)
	- Free tier has ads
		- Next lowest tier is $17/mo
- Mailchimp
	- I refuse to [give money to Intuit](https://www.propublica.org/article/inside-turbotax-20-year-fight-to-stop-americans-from-filing-their-taxes-for-free)
- [Senderr](https://senderr.dev/#pricing-section)
	- Seems to be focused on selling templates rather than delivering emails.
- Buttondown
	- I use them for my newsletters, and they kind of support SMTP, but it doesn’t seem like a scenario they’d be a good match for.
- [Plunk](https://www.useplunk.com/pricing)
	- Pro: Pay as you go ($0.001/email)
		- Pro: [Open-source](https://github.com/useplunk)
		- Seems to be a one-person shop
		- Tried them (2026-01-21)
		- Bad: Their onboarding email went to spam in my Fastmail account
				- Dealbreaker: When I completed onboarding, they told me that I was on a legacy app and I had to create a whole new account on their next generation platform.
				- Bad: They add tracking pixels to email by default
- [More options](https://docs.google.com/spreadsheets/d/1vxsC4DI_jOF0yQS3_iZA_6ICIacoEqOwqpjIVj54JS0/edit?gid=672304209#gid=672304209) …

### Video encoding

**Decision**: Convert videos using ffmpeg in a background process in the same Docker container as the Go app. All other solutions increased complexity and exposed video data to external vendors.

I want to encode videos so that they have WebM encoding so that [all major browsers](https://caniuse.com/?search=webm) can natively play them without relying on a third-party video player or proprietary codecs.

- **Use ffmpeg within my Docker container**
	- Pros
		- Keeps deployment simple, as there are no dependencies on an external service
		- Cons
		- It’s [ugly](https://docs.docker.com/engine/containers/multi-service_container/) to run an additional process in a Docker container (although I’m doing that anyway with Litestream)
- Roll my own custom cloud function for converting videos
	- Pros
		- Preserves privacy
				- Keeps costs low
		- Cons
		- Increases complexity of deploying
				- Headache of maintaining a separate video encoding microservice
				- Have to implement authentication too, to prevent anonymous strangers from using up server resources
- Use a third-party video-encoding API like [rendi](https://www.rendi.dev/) or [zzmpeg](https://zzmpeg.com/)
	- Pros
		- Don’t have to implement video encoding logic
		- Cons
		- Increases complexity of deploying

## Alternatives considered

### Alternative products

#### Google Photos

- Pros
	- Free if we can fit photos
- Cons
	- Don’t like taking dependencies on Google
		- Google’s privacy policies tend to not be aligned with their users’ interests
		- Doesn’t support email workflow I want

#### Momatu

- Pros
	- Privacy-focused
		- No ads
- Cons
	- No web app (mobile-only)
		- No obvious way for them to stay in business, as they don’t charge money or have any way of making money.
		- No updates to the website since 2022.
		- Doesn’t support email workflow I want

#### PhotoCircle

It’s like TinyBeans but worse.

- Pros
	- Designed for sharing photos
		- Mobile app is easy to use
- Cons
	- Not focused on family photos
		- No way to turn off ads for family members, even on paid plans

#### Immich, Ente, zeitkapsl, Photoprism, NextCloud

- Pros
	- Popular in self-hosting community
		- Open-source
- Cons
	- None of them support email-driven workflows I want.
		- They’re all too heavyweight to patch in to do what I want.

### Alternative media access control

Keeping media files on public S3-style storage buckets increases attack surface of the media files and limits the app from performing access control or logging on media file access. I thought about alternatives, but I couldnt find anything that offers a net improvement overall on aggregate security, UX, and performance.

#### Signed URLs

We could make everything in the S3 bucket private and require signed URLs to view them, but that adds a lot of complexity and degrades user experience.

For example, if a user reads an email we sent a year ago, the image should load successfully and not fail because of an expired URL signature.

#### Proxying media

We could proxy media through Little Moments, but that would reduce performance, as it forces an additional hop in every round trip for media requests (browser -> app -> S3 vs. browser -> S3). I tried something on a similar app several years ago and [the performance impact was severe](https://mtlynch.io/digitizing-2/#mediagoblin-and-the-video-storage-problem).

Another disadvantage is that images in emails break when the Little Moments app is not running, whereas uptime on direct S3 URLs from a cloud storage vendor will rarely go down.

Proxying also means that image embeds in emails break when the Little Moments server is down, whereas direct links to cloud storage will rarely go down.

Proxying would likely also cause issues in email clients that are separate from web browsers where the user is not signed in, so images would fail to load on authentication failure.

#### Signed URLs + redirects

We could expose native Little Moments URLs to the user but respond with a redirect to the signed S3 URL:

1. User’s browser requests a media file route on Little Moments like `/media/photo/2026-03-25-a.jpg`
2. Server verifies that the user is signed in and can view the photo
3. Server generates a signed URL for the S3 copy of that image for that user
4. Server sends a HTTP 302 Found redirect to the signed URL

That way, images embedded in emails will live as long as Little Moments is running.

The disadvantage is that it combines the disadvantages of signed URLs and proxying, but we still end up with dead links if the user bookmarks a direct image URL or shares it via email. What the user sees in their browser will be the signed S3 URL.

We could try to prevent the user from ever seeing an S3 URL by using service workers or iframes, but that feels like way too much complexity.

### Alternative frontend stacks

I’ve tried several frontend frameworks (Vue, Angular, React, Svelte), but I [work better in plain HTML, JavaScript, and CSS](https://refactoringenglish.com/blog/software-essays-that-shaped-me/#-by-julia-evans-2020). Every time I use a frontend framework, I’m impressed at how quickly I can get to v1, but then I’m forever dealing with gotchas and subtleties of the framework, and there’s never a clean path out.

I find vanilla HTML and JavaScript easier to reason about, and I never have to deal with package managers, library compatibility, or SPA-specific issues like page navigation and routing.

### Alternative icon libraries

#### FontAwesome

I use FontAwesome out of habit, but I haven’t looked into alternatives recently. I like it okay, but I don’t love it.

#### Bootstrap Icons

I considered using Bootstrap’s icons since I’m already using their CSS, but I found the style a little too businesslike, whereas Lucide feels warmer and more personable.

### Alternative software licenses

#### MIT License

I use it on a lot of projects, but it fails to prevent third-parties from commercializing the code. Same with Apache2, BSD, etc. and other permissive licenses.

#### GPLv3

The GPLv3 prevents me from changing the license once other people contribute to the project, even if I’ve done 99% of the work.

## Appendix: tinybeans-export format

### File structure

tinybeans-export creates a file structure like the following:

```
.
├── 2024-08-18_650648406
│   ├── c77f0d7f-6d8f-429e-be50-9bf161d68d1d-o.jpg
│   └── metadata.json
├── 2025-12-27_714502829
│   ├── 01038d5e-5f16-46ed-980b-1a06bed27058thumbnail-o.jpg
│   ├── a9a574bf-94b7-4b5c-a19a-b5ab74bda55d.mp4
│   └── metadata.json
├── ...
├── followers.json
└── journal.json
```

Each folder is a piece of media containing the original size photo, original size video + thumbnail, and a metadata JSON file.

### journal.json

The `journal.json` contains data like the following, where Homer Simpson is the parent and Bart Simpson is the child:

```json
{
  "id": 9876123,
  "timestamp": 1724014779666,
  "title": "Bart Simpson",
  "user": {
    "id": 1112221,
    "timestamp": 1724014779485,
    "lastUpdatedTimestamp": 1769876901930,
    "fullName": "Homer Simpson",
    "firstName": "Homer",
    "lastName": "Simpson",
    "hasMemoriesAccess": true
  },
  "children": [
    {
      "id": 8882233,
      "timestamp": 1724026487665,
      "lastUpdatedTimestamp": 1724026487665,
      "firstName": "Leo",
      "lastName": "Simpson",
      "fullName": "Bart Simpson",
      "gender": "MALE",
      "dob": "1987-04-01",
      "avatars": {
        "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-o.png",
        "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-s.png",
        "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-m.png",
        "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-l.png"
      },
      "user": {
        "id": 1112221,
        "timestamp": 1724014779485,
        "lastUpdatedTimestamp": 1769876901930,
        "fullName": "Homer Simpson",
        "firstName": "Homer",
        "lastName": "Simpson",
        "hasMemoriesAccess": true
      }
    }
  ]
}
```

### followers.json

The `followers.json` file has data like the following:

```json
[
  {
    "id": 5587986,
    "URL": "https://tinybeans.com/api/1/journals/9876123/followers/5587986",
    "timestamp": 1724014779668,
    "journalId": 9876123,
    "user": {
      "id": 1112221,
      "URL": "https://tinybeans.com/api/1/users/1112221",
      "timestamp": 1724014779485,
      "lastUpdatedTimestamp": 1769876901930,
      "fullName": "Homer Simpson",
      "firstName": "Homer",
      "lastName": "Simpson",
      "username": "parent@example.com",
      "publicUsername": "",
      "emailAddress": "parent@example.com",
      "avatars": {
        "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-o.png",
        "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-s.png",
        "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-m.png",
        "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-l.png"
      },
      "timeZone": {
        "name": "America/New_York",
        "label": "(GMT-5:00) America/New_York",
        "offset": 0
      },
      "timeZoneOffset": -18000000,
      "hasMemoriesAccess": true,
      "deleted": false,
      "emailOptOut": false,
      "emailMarketingOptOut": true,
      "emailWeeklySummary": false,
      "emailFrequencyOnNewComment": {
        "name": "NONE",
        "label": "Do not send"
      },
      "emailFrequencyOnNewEmotion": {
        "name": "NONE",
        "label": "Do not send"
      }
    },
    "relationship": {
      "name": "FATHER",
      "label": "Father"
    },
    "viewEntries": true,
    "addEntries": true,
    "viewMilestones": true,
    "editMilestones": true,
    "coOwner": false,
    "sortOrder": 0,
    "sendFlashback": false,
    "emailFrequencyOnNewEvent": {
      "name": "DAILY",
      "label": "Send once a day"
    }
  },
  {
    "id": 1234890,
    "URL": "https://tinybeans.com/api/1/journals/9876123/followers/1234890",
    "timestamp": 1724026924544,
    "journalId": 9876123,
    "user": {
      "id": 2221112,
      "URL": "https://tinybeans.com/api/1/users/2221112",
      "timestamp": 1724026924424,
      "lastUpdatedTimestamp": 1769626595834,
      "fullName": "Marge Simpson",
      "firstName": "Marge",
      "lastName": "Simpson",
      "username": "parent2@example.com",
      "publicUsername": "",
      "emailAddress": "parent2@example.com",
      "avatars": {
        "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-o.png",
        "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-s.png",
        "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-m.png",
        "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-l.png"
      },
      "timeZone": {
        "name": "America/Chicago",
        "label": "(GMT-6:00) America/Chicago",
        "offset": 0
      },
      "timeZoneOffset": -21600000,
      "hasMemoriesAccess": true,
      "deleted": false,
      "emailOptOut": false,
      "emailMarketingOptOut": true,
      "emailWeeklySummary": true,
      "emailFrequencyOnNewComment": {
        "name": "IMMEDIATE",
        "label": "Send immediately"
      },
      "emailFrequencyOnNewEmotion": {
        "name": "IMMEDIATE",
        "label": "Send immediately"
      }
    },
    "relationship": {
      "name": "MOTHER",
      "label": "Mother"
    },
    "viewEntries": true,
    "addEntries": true,
    "viewMilestones": true,
    "editMilestones": true,
    "coOwner": true,
    "sortOrder": 0,
    "sendFlashback": true,
    "emailFrequencyOnNewEvent": {
      "name": "NONE",
      "label": "Do not send"
    }
  },
  {
    "id": 3953212,
    "URL": "https://tinybeans.com/api/1/journals/9876123/followers/3953212",
    "timestamp": 1724167761726,
    "journalId": 9876123,
    "user": {
      "id": 9992223,
      "URL": "https://tinybeans.com/api/1/users/9992223",
      "timestamp": 1724167761595,
      "lastUpdatedTimestamp": 1768396343391,
      "fullName": "Grampa Simpson",
      "firstName": "Grampa",
      "lastName": "Simpson",
      "username": "grampa@example.com",
      "publicUsername": "",
      "emailAddress": "grampa@example.com",
      "avatars": {
        "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-o.png",
        "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-s.png",
        "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-m.png",
        "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-l.png"
      },
      "timeZone": {
        "name": "America/Detroit",
        "label": "(GMT-5:00) America/Detroit",
        "offset": 0
      },
      "timeZoneOffset": -18000000,
      "hasMemoriesAccess": true,
      "deleted": false,
      "emailOptOut": false,
      "emailMarketingOptOut": true,
      "emailWeeklySummary": true,
      "emailFrequencyOnNewComment": {
        "name": "IMMEDIATE",
        "label": "Send immediately"
      },
      "emailFrequencyOnNewEmotion": {
        "name": "IMMEDIATE",
        "label": "Send immediately"
      }
    },
    "relationship": {
      "name": "GRANDFATHER",
      "label": "Grandfather"
    },
    "viewEntries": true,
    "addEntries": false,
    "viewMilestones": false,
    "editMilestones": false,
    "coOwner": false,
    "sortOrder": 0,
    "sendFlashback": true,
    "emailFrequencyOnNewEvent": {
      "name": "DAILY",
      "label": "Send once a day"
    }
  },
  {
    "id": 5444455,
    "URL": "https://tinybeans.com/api/1/journals/9876123/followers/5444455",
    "timestamp": 1743948015529,
    "journalId": 9876123,
    "user": {
      "id": 1112223,
      "URL": "https://tinybeans.com/api/1/users/1112223",
      "timestamp": 1743948015380,
      "lastUpdatedTimestamp": 1769517178722,
      "fullName": "Maude Flanders",
      "firstName": "Maude",
      "lastName": "Flanders",
      "username": "Maude.Flanders@example.com",
      "publicUsername": "",
      "emailAddress": "Maude.Flanders@example.com",
      "avatars": {
        "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-o.png",
        "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-s.png",
        "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-m.png",
        "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-l.png"
      },
      "timeZone": {
        "name": "America/New_York",
        "label": "(GMT-5:00) America/New_York",
        "offset": 0
      },
      "timeZoneOffset": -18000000,
      "hasMemoriesAccess": true,
      "deleted": false,
      "emailOptOut": false,
      "emailMarketingOptOut": false,
      "emailWeeklySummary": true,
      "emailFrequencyOnNewComment": {
        "name": "IMMEDIATE",
        "label": "Send immediately"
      },
      "emailFrequencyOnNewEmotion": {
        "name": "IMMEDIATE",
        "label": "Send immediately"
      }
    },
    "relationship": {
      "name": "FRIEND",
      "label": "Friend"
    },
    "viewEntries": true,
    "addEntries": true,
    "viewMilestones": true,
    "editMilestones": false,
    "coOwner": false,
    "sortOrder": 0,
    "sendFlashback": true,
    "emailFrequencyOnNewEvent": {
      "name": "DAILY",
      "label": "Send once a day"
    }
  }
]
```

### metadata.json (photo)

Photo metadata files have a structure like the following:

```json
{
  "id": 777773331,
  "journalId": 9876123,
  "userId": 2221112,
  "URL": "https://tinybeans.com/api/1/journals/9876123/entries/777773331",
  "timestamp": 1761779844168,
  "lastUpdatedTimestamp": 1761829948040,
  "year": 2025,
  "month": 10,
  "day": 20,
  "caption": "Baby's first milkshake!",
  "privateMode": false,
  "uuid": "77722211-5666-4939-af12-aaaaaaabbbbb",
  "type": "PHOTO",
  "blobs": {
    "o": "https://tinybeans.com/pv/e/777773331/44332211-6340-4b34-9770-cccccdddd111-o.jpg",
    "o2": "https://tinybeans.com/pv/e/777773331/44332211-6340-4b34-9770-cccccdddd111-o2.jpg",
    "t": "https://tinybeans.com/pv/e/777773331/44332211-6340-4b34-9770-cccccdddd111-t.jpg",
    "s": "https://tinybeans.com/pv/e/777773331/44332211-6340-4b34-9770-cccccdddd111-s.jpg",
    "s2": "https://tinybeans.com/pv/e/777773331/44332211-6340-4b34-9770-cccccdddd111-s2.jpg",
    "m": "https://tinybeans.com/pv/e/777773331/44332211-6340-4b34-9770-cccccdddd111-m.jpg",
    "l": "https://tinybeans.com/pv/e/777773331/44332211-6340-4b34-9770-cccccdddd111-l.jpg",
    "p": "https://tinybeans.com/pv/e/777773331/44332211-6340-4b34-9770-cccccdddd111-p.jpg"
  },
  "sortOrder": 1,
  "totalCommentsCount": 2,
  "comments": [
    {
      "id": 111222111,
      "entryId": 777773331,
      "URL": "https://tinybeans.com/api/1/journals/9876123/entries/777773331/comments/111222111",
      "timestamp": 1761825170399,
      "lastUpdatedTimestamp": 1761825170399,
      "user": {
        "id": 4791052,
        "timestamp": 1735939747014,
        "lastUpdatedTimestamp": 1769606164702,
        "fullName": "Waylon Smithers",
        "firstName": "Waylon",
        "lastName": "Smithers",
        "hasMemoriesAccess": true
      },
      "details": "Everyone looks so happy!",
      "repliesCount": 0
    },
    {
      "id": 333333311,
      "entryId": 777773331,
      "URL": "https://tinybeans.com/api/1/journals/9876123/entries/777773331/comments/333333311",
      "timestamp": 1761829948041,
      "lastUpdatedTimestamp": 1761829948041,
      "user": {
        "id": 4795311,
        "timestamp": 1737211058213,
        "lastUpdatedTimestamp": 1767285698024,
        "fullName": "Barney Gumble",
        "firstName": "Barney",
        "lastName": "Gumble",
        "hasMemoriesAccess": true
      },
      "details": "What a great memory!",
      "repliesCount": 0
    }
  ],
  "children": [
    {
      "URL": "https://tinybeans.com/api/1/children/8882233",
      "avatars": {
        "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-l.png",
        "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-m.png",
        "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-o.png",
        "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-s.png"
      },
      "deleted": false,
      "dob": "1987-04-01",
      "firstName": "Bart",
      "fullName": "Bart Simpson",
      "gender": "MALE",
      "id": 8882233,
      "lastName": "Simpson",
      "lastUpdatedTimestamp": 1724026487665,
      "timestamp": 1724026487665,
      "user": {
        "avatars": {
          "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-l.png",
          "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-m.png",
          "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-o.png",
          "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-s.png"
        },
        "deleted": false,
        "firstName": "Homer",
        "fullName": "Homer Simpson",
        "hasMemoriesAccess": true,
        "id": 1112221,
        "lastName": "Simpson",
        "lastUpdatedTimestamp": 1769876901930,
        "timestamp": 1724014779485
      }
    }
  ],
  "emotions": [
    {
      "id": 751954309,
      "URL": "https://tinybeans.com/api/1/journals/9876123/entries/777773331/emotions/751954309",
      "type": {
        "name": "LOVE",
        "label": "Love"
      },
      "timestamp": 1761823202980,
      "lastUpdatedTimestamp": 1761823202980,
      "deleted": false,
      "entryId": 777773331,
      "userId": 4748247
    },
    {
      "id": 751956133,
      "URL": "https://tinybeans.com/api/1/journals/9876123/entries/777773331/emotions/751956133",
      "type": {
        "name": "LOVE",
        "label": "Love"
      },
      "timestamp": 1761823886664,
      "lastUpdatedTimestamp": 1761823886664,
      "deleted": false,
      "entryId": 777773331,
      "userId": 4791061
    },
    {
      "id": 751957695,
      "URL": "https://tinybeans.com/api/1/journals/9876123/entries/777773331/emotions/751957695",
      "type": {
        "name": "LOVE",
        "label": "Love"
      },
      "timestamp": 1761824428849,
      "lastUpdatedTimestamp": 1761824428849,
      "deleted": false,
      "entryId": 777773331,
      "userId": 4748246
    },
    {
      "id": 751962461,
      "URL": "https://tinybeans.com/api/1/journals/9876123/entries/777773331/emotions/751962461",
      "type": {
        "name": "LOVE",
        "label": "Love"
      },
      "timestamp": 1761826117001,
      "lastUpdatedTimestamp": 1761826117001,
      "deleted": false,
      "entryId": 777773331,
      "userId": 4780152
    },
    {
      "id": 751973108,
      "URL": "https://tinybeans.com/api/1/journals/9876123/entries/777773331/emotions/751973108",
      "type": {
        "name": "LOVE",
        "label": "Love"
      },
      "timestamp": 1761829939537,
      "lastUpdatedTimestamp": 1761829939537,
      "deleted": false,
      "entryId": 777773331,
      "userId": 4795311
    }
  ]
}
```

### metadata.json (video)

Video metadata files have a structure like the following:

```json
{
  "id": 888883331,
  "journalId": 9876123,
  "userId": 2221112,
  "URL": "https://tinybeans.com/api/1/journals/9876123/entries/888883331",
  "timestamp": 1724170887200,
  "lastUpdatedTimestamp": 1724198805332,
  "year": 2024,
  "month": 8,
  "day": 20,
  "caption": "Bart's first visit to the zoo",
  "privateMode": false,
  "uuid": "88888895-12c6-4858-a9f0-1ba4f5a82915",
  "type": "PHOTO",
  "blobs": {
    "o": "https://tinybeans.com/pv/e/888883331/88888895-12c6-4858-a9f0-1ba4f5a82915thumbnail-o.jpg",
    "o2": "https://tinybeans.com/pv/e/888883331/88888895-12c6-4858-a9f0-1ba4f5a82915thumbnail-o2.jpg",
    "t": "https://tinybeans.com/pv/e/888883331/88888895-12c6-4858-a9f0-1ba4f5a82915thumbnail-t.jpg",
    "s": "https://tinybeans.com/pv/e/888883331/88888895-12c6-4858-a9f0-1ba4f5a82915thumbnail-s.jpg",
    "s2": "https://tinybeans.com/pv/e/888883331/88888895-12c6-4858-a9f0-1ba4f5a82915thumbnail-s2.jpg",
    "m": "https://tinybeans.com/pv/e/888883331/88888895-12c6-4858-a9f0-1ba4f5a82915thumbnail-m.jpg",
    "l": "https://tinybeans.com/pv/e/888883331/88888895-12c6-4858-a9f0-1ba4f5a82915thumbnail-l.jpg",
    "p": "https://tinybeans.com/pv/e/888883331/88888895-12c6-4858-a9f0-1ba4f5a82915thumbnail-p.jpg"
  },
  "sortOrder": 2,
  "attachmentType": "VIDEO",
  "attachmentUrl": "https://tinybeans.com/pv/e/888883331/a/aaaaa28-3a06-4852-9a61-e0551ef7f227.mp4",
  "attachmentUrl_mp4": "https://tinybeans.com/pv/e/888883331/a/aaaaa28-3a06-4852-9a61-e0551ef7f227-mp4.mp4",
  "attachmentUrl_webm": "https://tinybeans.com/pv/e/888883331/a/aaaaa28-3a06-4852-9a61-e0551ef7f227-webm.mp4",
  "totalCommentsCount": 0,
  "comments": [],
  "children": [
    {
      "URL": "https://tinybeans.com/api/1/children/8882233",
      "avatars": {
        "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-l.png",
        "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-m.png",
        "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-o.png",
        "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-child-s.png"
      },
      "deleted": false,
      "dob": "1987-04-01",
      "firstName": "Bart",
      "fullName": "Bart Simpson",
      "gender": "MALE",
      "id": 8882233,
      "lastName": "Simpson",
      "lastUpdatedTimestamp": 1724026487665,
      "timestamp": 1724026487665,
      "user": {
        "avatars": {
          "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-l.png",
          "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-m.png",
          "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-o.png",
          "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-s.png"
        },
        "deleted": false,
        "firstName": "Homer",
        "fullName": "Homer Simpson",
        "hasMemoriesAccess": true,
        "id": 2221112,
        "lastName": "Simpson",
        "lastUpdatedTimestamp": 1774227600456,
        "timestamp": 1724014779485
      }
    }
  ],
  "emotions": []
}
```

### comment-thread-${id}.json

Comment thread files have a structure like the following:

```json
{
  "status": "ok",
  "message": null,
  "comment": {
    "id": 111222333,
    "deleted": false,
    "timestamp": 1771080030512,
    "lastUpdatedTimestamp": 1771080030512,
    "replies": [
      {
        "id": 999922222,
        "deleted": false,
        "timestamp": 1771082378181,
        "lastUpdatedTimestamp": 1771082378181,
        "user": {
          "id": 2222232,
          "deleted": false,
          "timestamp": 1724167810486,
          "lastUpdatedTimestamp": 1769606506976,
          "fullName": "Maude Flanders",
          "firstName": "Maude",
          "lastName": "Flanders",
          "publicUsername": null,
          "avatars": {
            "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-s.png",
            "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-l.png",
            "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-m.png",
            "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-o.png"
          },
          "hasMemoriesAccess": true
        },
        "userAgent": "[Tinybeans/500.83.1 (165) - iPhone 13 mini/26.2.1]",
        "details": "Me too!",
        "parentId": 111222333,
        "repliesCount": 0
      }
    ],
    "user": {
      "id": 3331111,
      "deleted": false,
      "timestamp": 1732465623693,
      "lastUpdatedTimestamp": 1773575382686,
      "fullName": "Lisa Simpson",
      "firstName": "Lisa",
      "lastName": "Simpson",
      "publicUsername": null,
      "avatars": {
        "s": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-s.png",
        "l": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-l.png",
        "m": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-m.png",
        "o": "https://tinybeans-public.s3-us-west-2.amazonaws.com/images/avatar-user-o.png"
      },
      "hasMemoriesAccess": true
    },
    "entry": {
      "id": 111222333,
      "deleted": false,
      "timestamp": 1770997872469,
      "lastUpdatedTimestamp": 1771170106905,
      "year": 2026,
      "url": "https://tinybeans.com/api/1/journals/1818181/entries/111222333",
      "month": 1,
      "day": 31,
      "userId": 3339333,
      "uuid": "92929292-44b2-4c9d-ab3d-88888661b0a1",
      "journalId": 1818181,
      "privateMode": false,
      "blobs": {
        "o": "https://tinybeans.com/pv/e/111222333/92929292-44b2-4c9d-ab3d-88888661b0a1thumbnail-o.jpg",
        "o2": "https://tinybeans.com/pv/e/111222333/92929292-44b2-4c9d-ab3d-88888661b0a1thumbnail-o2.jpg",
        "t": "https://tinybeans.com/pv/e/111222333/92929292-44b2-4c9d-ab3d-88888661b0a1thumbnail-t.jpg",
        "s": "https://tinybeans.com/pv/e/111222333/92929292-44b2-4c9d-ab3d-88888661b0a1thumbnail-s.jpg",
        "s2": "https://tinybeans.com/pv/e/111222333/92929292-44b2-4c9d-ab3d-88888661b0a1thumbnail-s2.jpg",
        "m": "https://tinybeans.com/pv/e/111222333/92929292-44b2-4c9d-ab3d-88888661b0a1thumbnail-m.jpg",
        "l": "https://tinybeans.com/pv/e/111222333/92929292-44b2-4c9d-ab3d-88888661b0a1thumbnail-l.jpg",
        "p": "https://tinybeans.com/pv/e/111222333/92929292-44b2-4c9d-ab3d-88888661b0a1thumbnail-p.jpg"
      },
      "type": "PHOTO"
    },
    "userAgent": "[Mozilla/5.0 (iPhone; CPU iPhone OS 18_3_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.3.1 Mobile/15E148 Safari/604.1]",
    "details": "Love it!",
    "entryId": 111222333,
    "repliesCount": 1,
    "URL": "https://tinybeans.com/api/1/journals/1818181/entries/111222333/comments/111222333"
  }
}
```

Comment thread files only exist when the associated media file contains a threaded conversation. It only appears when at least one reply to a media item contains its own reply.

`metadata.json` contains all root-level comments, but it omits threaded replies (sub-comments).