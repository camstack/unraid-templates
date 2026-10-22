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

## Accelerators

Every accelerator entry ships **empty**: Docker refuses to start a container that
declares a device the host does not have, so enter only what this machine has. Full
guide: [Accelerator passthrough](https://camstack.io/docs/installation/accelerators).

| Entry | Enter | Host side |
| --- | --- | --- |
| Intel/AMD iGPU | `/dev/dri` | in the kernel |
| Intel NPU / AMD XDNA | `/dev/accel` | in the kernel (XDNA: detected, no inference runtime yet) |
| Coral USB | `/dev/bus/usb` (a path: the stick re-enumerates) | the udev rule in the [Unraid guide](https://camstack.io/docs/installation/unraid#coral-usb-the-device-must-be-writable-by-camstack); the `--device-cgroup-rule` is already in Extra Parameters |
| Coral PCIe/M.2 unit 1, 2 | `/dev/apex_0`, `/dev/apex_1` | the gasket/apex driver (Coral driver plugin); image newer than v1.2.583 |
| NVIDIA | — | Nvidia Driver plugin, `--runtime=nvidia` in Extra Parameters, variables `NVIDIA_VISIBLE_DEVICES=all` and `NVIDIA_DRIVER_CAPABILITIES=all` |

No group setting is needed: the image entrypoint makes every passed node openable by
CamStack. A Coral is never used until you enable it per node (Pipeline → the node →
Inference devices), after installing its runtime once from inside the container.

## Reporting a problem

Open an issue here. Include your Unraid version, the template you used, and the
container log — for a template problem the useful part is usually the very first
lines, before the application starts.

## Licence

MIT.
