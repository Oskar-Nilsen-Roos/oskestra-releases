# Oskestra releases

Builds of Oskestra, a desktop app for running AI coding agents across accounts and projects. This
repository holds only the releases: the source is private. Download from
[Releases](https://github.com/Oskar-Nilsen-Roos/oskestra-releases/releases/latest).

Each release has:

| File | For |
| --- | --- |
| `Oskestra-<version>.zip` | macOS on Apple silicon |
| `Oskestra-<version>-x86_64.AppImage` | Linux x86_64 |
| `Oskestra-<version>-aarch64.AppImage` | Linux arm64, when built |
| `PKGBUILD` | Arch Linux |
| `SHA256SUMS` | checksums of the files above |
| `appcast.json` | the in-app updater's feed |

## Check the download

```sh
sha256sum -c SHA256SUMS --ignore-missing     # Linux
shasum -a 256 -c SHA256SUMS --ignore-missing # macOS
```

The app checks more than this when it updates itself: every archive in `appcast.json` carries an
ed25519 signature made with a key that never leaves the release machine, and the app refuses an
update whose signature doesn't match the public key built into it.

## macOS

Unzip and move `Oskestra.app` to Applications. Until releases are notarized, macOS blocks the first
launch: right-click the app, choose Open, then Open again. After that, Oskestra updates itself
(Settings › Updates).

## Arch Linux

```sh
cd "$(mktemp -d)" && curl -fLO https://github.com/Oskar-Nilsen-Roos/oskestra-releases/releases/latest/download/PKGBUILD && makepkg -si
```

This installs the AppImage's contents in `/opt/oskestra` with its dependencies from the repos. Run
the same command to update; the app says when an update is available.

## Other Linux

```sh
mkdir -p ~/Applications
curl -fL -o ~/Applications/Oskestra.AppImage https://github.com/Oskar-Nilsen-Roos/oskestra-releases/releases/latest/download/Oskestra-<version>-x86_64.AppImage
chmod +x ~/Applications/Oskestra.AppImage
~/Applications/Oskestra.AppImage
```

The AppImage updates itself: it downloads the new one beside it, checks its signature and swaps it
in when you quit or choose Restart to update. It needs from the system: Wayland or X11, Vulkan,
WebKitGTK 4.1 and GTK 3, `secret-tool` (libsecret), `notify-send` and `xdg-open`, plus FUSE 2
(`libfuse2`) to mount the AppImage; without FUSE, run it with `APPIMAGE_EXTRACT_AND_RUN=1`.
