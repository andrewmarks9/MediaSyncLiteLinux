## iBroadcast Media Sync Lite v0.4.2

A Linux-native graphical uploader for iBroadcast

## Build
### Requirements overview
- gcc
- make
- gtk+3.0 development files >= 3.10
- libcurl
- openssl
- libssl-dev
- libjansson (also available in ./jansson-2.7)

### Debian
The easist way to install for Debian is using the DEB package available in releases.

You can build the package manually as well:
```bash
sudo apt install build-essential libgtk-3-dev libcurl4-openssl-dev libjansson-dev
```
### Fedora
```bash
sudo dnf groupinstall "C Development Tools and Libraries"
sudo dnf install jansson-devel libcurl-devel gtk3-devel
```

## Compile
Just execute:
```bash
make
```

## Run
Now you can run MediaSync Lite from the directory where it was built:
```bash
./mediasynclite
```

## Install
You can optionally install MediaSync Lite so it's available system-wide:
```bash
sudo make install
```
The application will be installed in /usr/local/bin directory.

## Nix
Nix users on the unstable branch can install by adding `pkgs.mediasynclite` to their system packages or in an ephemeral shell by
```nix
nix-shell -p mediasynclite
```

Alternatively, you can clone this directory and run `nix-build` to build the derivation.

## Duplicate detection & tagging
MediaSync Lite now reads basic audio metadata (ID3v2/ID3v1 tags for MP3-family
files, Vorbis comments for FLAC/Ogg/Opus) while scanning:
- The file list and upload screens show a friendlier "Artist - Title" label
  next to the path when tags are available.
- In addition to MD5-based duplicate detection (comparing against the
  hashes iBroadcast already has on file), a normalized
  title/artist/album/track "signature" is used as a secondary duplicate
  check, catching re-encoded or re-tagged copies of a track that a pure
  MD5 comparison would miss.
- Duplicate lookups now use hash sets instead of linear scans, so scanning
  large libraries against a large existing collection is significantly
  faster.
- Files uploaded during the current run are recorded immediately, so
  scanning the same folder again in the same session correctly skips them
  as duplicates without needing to restart the app.

## Known issues
Tag-based duplicate detection only covers files uploaded earlier in the
current app run (the iBroadcast API used here only exposes existing
server-side MD5 hashes, not full track metadata) - it does not retroactively
catch re-tagged duplicates of files uploaded in a previous session.
