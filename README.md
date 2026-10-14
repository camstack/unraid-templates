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

## Container contract

These templates are maintained by hand and mirror the image's contract: ports, volumes,
device passthroughs and the environment variables the server reads. When the image's
contract changes, update both files here in the same piece of work.

- Hub: `4443/tcp` (HTTPS admin UI), `6443/tcp` (mutual-TLS cluster mesh, multi-machine
  clusters only), `8554/tcp` (RTSP restream).
- Agent: `4444/tcp` (status dashboard), `6443/tcp` (mutual-TLS listener).
- The old clear mesh port `6000` and the UDP discovery port `4445` are gone.
- Hubs announce themselves over mDNS (UDP `5353` multicast). That reaches the LAN with a
  dedicated container IP (`br0`) or host networking, and not in Docker bridge mode.

## Reporting a problem

Open an issue here. Include your Unraid version, the template you used, and the
container log — for a template problem the useful part is usually the very first
lines, before the application starts.

## Licence

MIT.
