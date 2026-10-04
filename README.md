# DreamCatcher Base Config — EFP Builder

A single-page, browser-only tool for generating `.efp` deployment packages that apply a base configuration to one or more Evertz DreamCatcher servers.

Open `dc-configurator.html` in a browser (or the hosted GitHub Pages link). No install, no server, no network calls — everything is built locally in the page.

## What it configures

Each section can be toggled on or off. Only enabled sections are written into the package.

| Section | Target |
|---|---|
| Timezone | `/etc/timezone` |
| Hostname | `hostnamectl` |
| Network interfaces (eth0–3, eth20/21) | `/etc/netplan/01-netcfg.yaml` |
| Matrox network ports (mvkEthernet0–3) | Matrox interface config |
| `[System]` settings | `/etc/dreamcatcher.ini` |
| Features | `/etc/features.conf` |
| Render delay | `/etc/default/render_delay.json` |
| Vue layouts | `/etc/vue/layouts/` |
| Vue images | `/etc/vue/images/` |
| Linux firewall | Stops and disables `ufw` / `firewalld` |

## Bulk deployment

Set a hostname prefix, start number, and server count, and the tool cascades hostnames (e.g. `REPLAY-01`, `REPLAY-02`, …) and increments interface IPs per server. Any value can be overridden per server in the **Per-server settings** card.

## Usage

1. Set the timezone and bulk deployment values (prefix, start #, count).
2. Enable and set base IPs for the interfaces and Matrox ports you need.
3. Adjust `[System]`, features, and render delay as needed.
4. Drag in any Vue layout or image files.
5. Review the install script in the preview pane.
6. Click **Generate deployment package** to download a zip containing one `.efp` per server.
7. Install each `.efp` on its matching server.
8. Run `netplan apply` or reboot to activate networking changes.

## Saving and reusing a setup

- **Save config (.json)** exports the full tool state, including uploaded layouts and images.
- **Load config** restores it for editing or redeploying later.

## Package contents

Each `.efp` is a gzipped tar containing:

- `info` — name, version, description, build date
- `install` — generated bash install script (executable)
- Any uploaded layout and image files
- `cksum.md5` — MD5 checksums of the above

## Dependencies

Loaded from cdnjs:

- [JSZip 3.10.1](https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js)
- [pako 2.1.0](https://cdnjs.cloudflare.com/ajax/libs/pako/2.1.0/pako.min.js)

## Notes

- Default IPs in the page are placeholders. Verify all addresses against the site's IP plan before deploying.
- Disabling the firewall is on by default. Turn that section off if the site requires a firewall.
- Always review the generated install script before running it on production systems.
