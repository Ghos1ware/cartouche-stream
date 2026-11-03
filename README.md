# Cartouche Stream

Cartouche Stream is a game-streaming host for Windows: it streams the games of a PC to a phone, a tablet or a TV running [Moonlight](https://moonlight-stream.org). It is a fork of [Vibeshine](https://github.com/Nonary/vibeshine) by Nonary, itself a fork of [Sunshine](https://github.com/LizardByte/Sunshine) by LizardByte, and stays under the same license: [GPL-3.0](LICENSE).

It is the streaming server used by [Cartouche](https://cartouche.borderline.games), a separate Windows game launcher. Cartouche installs Cartouche Stream when its player turns streaming on, and talks to it only through the server's local HTTP API, as any client would. Cartouche Stream itself contains only open-source code; everything it ships is built from this repository and its open-source dependencies by GitHub Actions.

What this fork changes from Vibeshine is listed in each release's notes ([release_notes](release_notes)). Vibeshine's own documentation, features and history are in [docs/VIBESHINE.md](docs/VIBESHINE.md).

## Download

Windows installers are published as [releases](https://github.com/Ghos1ware/cartouche-stream/releases) of this repository, named `CartoucheStreamSetup-v<version>.exe`. Most players never run it by hand: Cartouche downloads it, checks its SHA-256 fingerprint and runs it.

### What the installer changes on the PC

Cartouche explains these before it installs anything, and Windows asks for the administrator's consent:

- a Windows service, **Cartouche Stream** (`SunshineService`), that starts with Windows;
- a virtual display driver and a virtual gamepad driver, so a phone can have its own screen and controller;
- Windows Firewall rules for the streaming ports (local network);
- its files in `C:\Program Files\Sunshine`.

### Uninstall

Windows Settings → Apps → Installed apps → **Vibeshine** (publisher Borderline Games) → Uninstall. This removes the service, the drivers, the firewall rules and the program files. The product keeps Vibeshine's installer name so it upgrades and uninstalls cleanly over an existing Vibeshine.

## Privacy

Cartouche Stream sends nothing to its authors or to any analytics service. It talks to:

- the devices you pair with it, to stream to them;
- GitHub's public API, to check this repository's releases for updates (no user data is sent);
- IGDB or Steam image servers, only when you look up a game cover in its settings page.

Its settings page only opens from the PC itself by default.

## Code signing policy

Windows releases of Cartouche Stream are signed with free code signing provided by [SignPath.io](https://about.signpath.io), certificate by [SignPath Foundation](https://signpath.org).

- **Committers and reviewers:** [Ghos1ware](https://github.com/Ghos1ware)
- **Approvers:** [Ghos1ware](https://github.com/Ghos1ware)

Every release is built by GitHub Actions from this repository ([cartouche-release.yml](.github/workflows/cartouche-release.yml)) and each signing request is approved by hand. See [Privacy](#privacy) for what the software sends over the network.

## Credits

- [Vibeshine](https://github.com/Nonary/vibeshine) by Nonary: the server this fork is built on.
- [Sunshine](https://github.com/LizardByte/Sunshine) by LizardByte and its contributors.
- [Moonlight](https://moonlight-stream.org), the client.
