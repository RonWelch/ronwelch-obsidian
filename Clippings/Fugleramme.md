---
title: Fugleramme
source: https://arnegiacomo.dev/fugleramme/
author:
published:
created: 2026-09-21
description: E-ink bird frame for Raspberry Pi
tags:
  - clippings
  - raspberrypi
---
E-ink bird frame for Raspberry Pi - real-time bird detection by audio, fully local AI, rendered as real, hand-cut 1800s bird illustrations.

A mic feeds [BirdNET-Go](https://github.com/tphakala/birdnet-go), which runs the BirdNET classifier and owns all detection config. Fugleramme reads its detections and renders the recently-seen birds as a collage on an [Inky Impression](https://shop.pimoroni.com/products/inky-impression) e-ink panel, and serves the same view as a web kiosk.

Fugleramme can install BirdNET-Go for you, or read from one you already run - on the same machine or elsewhere.

| No detections | A few visitors | A full garden |
| --- | --- | --- |
| ![No birds detected](https://arnegiacomo.dev/fugleramme/assets/empty.png) | ![A few garden birds](https://arnegiacomo.dev/fugleramme/assets/few.png) | ![Many garden birds](https://arnegiacomo.dev/fugleramme/assets/many.png) |

> [!tip] Tip
> Live on **[fugleramme.arnegiacomo.dev](https://fugleramme.arnegiacomo.dev/)** running from my kitchen window and displaying the actual birds currently heard in my garden (Bergen, Norway).

The birds are cut-outs from historic, public-domain natural-history drawings, hand-curated for this project - over 800 of them across more than 400 species. Each detected species is matched to its illustration and packed onto a textured paper page - larger birds toward the centre, sized by real body mass.

For more display options see [Display](https://arnegiacomo.dev/fugleramme/display/).

> [!note] Note
> Still in early development: expect the odd bug and a few unpolished edges, with plenty more features to come. Bug reports and suggestions are very welcome on [GitHub](https://github.com/arnegiacomo/fugleramme/issues).

## Docs

- **[FAQ](https://arnegiacomo.dev/fugleramme/faq/)** - Frequently asked questions
- **[Hardware](https://arnegiacomo.dev/fugleramme/hardware/)** - the parts list with alternatives
- **[Install](https://arnegiacomo.dev/fugleramme/install/)** - from a blank SD card to a running frame
- **[Configuring BirdNET-Go](https://arnegiacomo.dev/fugleramme/birdnetgo-config/)** - the mic, your location, and avoiding incorrect detections
- **[Display](https://arnegiacomo.dev/fugleramme/display/)** - modes, settings and names
- **[Operations](https://arnegiacomo.dev/fugleramme/operations/)** - buttons, services, logs and updates
- **[Container](https://arnegiacomo.dev/fugleramme/container/)** - running Fugleramme with Docker
- **[Species coverage](https://arnegiacomo.dev/fugleramme/species/)** - searchable list of currently supported species
- **[Adding artwork](https://arnegiacomo.dev/fugleramme/adding-artwork/)** - cutting a bird the frame can't draw yet
- **[Troubleshooting](https://arnegiacomo.dev/fugleramme/troubleshooting/)** - symptom to cause

> [!note] Note
> These docs are written from macOS. Everything on the Pi itself is the same whatever you drive it from - it's the host-side steps, like Internet Sharing over the USB cable, that differ. If you set yours up from Linux or Windows, a PR extending them is very welcome.

## Contact

Questions and ideas about the project belong in [Discussions](https://github.com/arnegiacomo/fugleramme/discussions). For anything else, you can reach me through [arnegiacomo.dev](https://arnegiacomo.dev/). I've built a few of these frames, but I currently don't have the capacity to build them for others.

---

Source: [github.com/arnegiacomo/fugleramme](https://github.com/arnegiacomo/fugleramme)