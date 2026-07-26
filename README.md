# CamStack — Unraid templates

Community Applications templates for [CamStack](https://camstack.io), a self-hosted
camera management platform: a **hub** plus optional **agents**, with ONVIF / RTSP /
Reolink / Hikvision device providers, WebRTC and RTSP restreaming, recording, and a
motion + ML detection pipeline that can run on an Intel iGPU, an Intel NPU or a Coral
Edge TPU.

| Template | What it runs |
| --- | --- |
| `templates/camstack.xml` | the **hub** — install this once |
| `templates/camstack-agent.xml` | an **agent** — add one on any other machine whose hardware you want to put to work |

Both run the same image; the role is selected by `CAMSTACK_ROLE`.

## Installing

Once this repository is listed in Community Applications, search for **CamStack** in
the *Apps* tab. Until then, add it manually in CA under *Settings → Community
Applications → Manage Repositories*, or install a template directly from its raw URL.

Start with the hub. Point an agent at the hub's address after the hub is up.

## ⚠️ These files are generated — do not edit them here

They are produced from the container image's real contract (image reference, the
environment variables the server actually reads, volumes, device passthroughs, ports)
by a generator in the CamStack server repository, and a CI guard fails the build when
a template and the image contract disagree. That is the point: a hand-maintained
template drifts from the image it is supposed to launch, silently, and the first
symptom is an install that pulls nothing or a mount the server ignores.

An edit made in this repository will be overwritten by the next copy. Fixes belong
upstream, in the generator.

## Reporting a problem

Open an issue here. Include your Unraid version, the template you used, and the
container log — for a template problem the useful part is usually the very first
lines, before the application starts.

## Licence

MIT.
