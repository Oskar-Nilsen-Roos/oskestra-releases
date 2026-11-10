# Oskio releases

Builds of Oskio, a desktop app for running AI coding agents across accounts and projects. This
repository holds only the releases: the source is private. Download from
[Releases](https://github.com/Oskar-Nilsen-Roos/oskio-releases/releases/latest).

Oskio was called Oskestra until 0.6. An Oskestra install updates itself to Oskio and keeps its
home, sessions and secrets.

Each release has:

| File | For |
| --- | --- |
| `Oskio-<version>-arm64.dmg` | macOS on Apple silicon |
| `Oskio-<version>.zip` | the same app, for the in-app updater |
| `Oskio-<version>-x86_64.AppImage`, `Oskio-<version>-aarch64.AppImage` | Linux, self-updating |
| `Oskio-<version>-x86_64.tar.gz`, `Oskio-<version>-aarch64.tar.gz` | Linux, self-updating |
| `oskio_<version>_amd64.deb`, `oskio_<version>_arm64.deb` | Debian, Ubuntu, Mint |
| `PKGBUILD` | Arch Linux, in a release that has one |
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

Open the `.dmg` and drag Oskio to Applications. The app is signed with a Developer ID and
notarized, and updates itself (Settings › Updates).

## Debian, Ubuntu, Mint

```sh
sudo apt install ./oskio_<version>_amd64.deb   # or _arm64.deb
```

A `.deb` install doesn't replace itself: when a newer release is out, the app says so in
Settings › Updates, and you install the new `.deb` the same way.

## Other Linux

```sh
mkdir -p ~/Applications
curl -fL -o ~/Applications/Oskio.AppImage https://github.com/Oskar-Nilsen-Roos/oskio-releases/releases/download/v<version>/Oskio-<version>-x86_64.AppImage
chmod +x ~/Applications/Oskio.AppImage
~/Applications/Oskio.AppImage
```

Use `aarch64` in place of `x86_64` on arm64. The AppImage updates itself: it downloads the new one
beside it, checks its signature and swaps it in when you quit or choose Restart to update. It needs
glibc 2.35 or newer (Ubuntu 22.04, Debian 12, Fedora 36 or later) and from the system: Wayland or
X11, Vulkan, WebKitGTK 4.1 and GTK 3, `secret-tool` (libsecret), `notify-send` and `xdg-open`, plus
FUSE 2 (`libfuse2`) to mount the AppImage; without FUSE, run it with `APPIMAGE_EXTRACT_AND_RUN=1`.

## Arch Linux

In a release that has a `PKGBUILD`:

```sh
cd "$(mktemp -d)" && curl -fLO https://github.com/Oskar-Nilsen-Roos/oskio-releases/releases/latest/download/PKGBUILD && makepkg -si
```

Run the same command to update; the app says when an update is available.
