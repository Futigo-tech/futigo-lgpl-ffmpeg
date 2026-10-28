# futigo-lgpl-ffmpeg

Complete corresponding source code of **FFmpeg** as shipped inside the **Futigo** mobile app (iOS and Android), starting with app version 2.0.0.

License: **LGPL-3.0-or-later**.

FFmpeg is built by media-kit without GPL and non-free parts and with `--enable-version3` (needed for Mbed TLS), so the libraries are under the GNU LGPL version 3 or later. The LGPL v3 is a set of additional permissions on top of the GNU GPL v3, so both texts are included. FFmpeg's own `LICENSE.md` (which also mentions LGPL 2.1) is included for reference.

This repository contains only unmodified upstream source code, the upstream patches applied by the media-kit project, the media-kit build scripts, and the license texts. It contains no Futigo code.

## Source files

All files are attached to the GitHub release **`app-2.0.0-sources`** of this repository, together with `SHA256SUMS`.

| Component / version | Platform | License | Original source URL | Release file | SHA-256 |
|---|---|---|---|---|---|
| FFmpeg 6.0 | iOS | LGPL-3.0-or-later | https://ffmpeg.org/releases/ffmpeg-6.0.tar.xz | `ffmpeg-6.0.tar.xz` | `57be87c22d9b49c112b6d24bc67d42508660e6b718b3db89c44e47e289137082` |
| FFmpeg 6.0 (git tag n6.0, commit ea3d24bbe3c58b171e55fe2151fc7ffaca3ab3d2) | Android | LGPL-3.0-or-later | https://github.com/FFmpeg/FFmpeg/archive/refs/tags/n6.0.tar.gz | `FFmpeg-n6.0.tar.gz` | `9a1fa94608300e835bf89bdfcd7d77deceba67bff3494f609816f84bd69a459e` |

- `ffmpeg-6.0.tar.xz`: official release tarball; SHA-256 equals `downloads.lock` of libmpv-darwin-build v0.6.0. `ffmpeg-6.0.tar.xz.asc` is the upstream signature.
- `FFmpeg-n6.0.tar.gz`: the Android build clones tag `n6.0` from github.com/FFmpeg/FFmpeg; this is the GitHub archive of that exact tag (commit id checked in the archive).

## Build scripts (how the binaries were made)

| Build repository | Platform | Original URL | Release file | SHA-256 |
|---|---|---|---|---|
| media-kit/libmpv-darwin-build, tag v0.6.0 (commit 4286f5557bdccc0747030e3c376ce5cd160a96a0) | iOS | https://github.com/media-kit/libmpv-darwin-build/archive/refs/tags/v0.6.0.tar.gz | `libmpv-darwin-build-0.6.0.tar.gz` | `afade6a15e001700c6d92860bb33eba43020450001e7464595717ab9a222d28a` |
| media-kit/libmpv-android-video-build, tag v1.1.7 (commit fe8c3ac1a91c09aa6fb1deccbc833f1bafa54768) | Android | https://github.com/media-kit/libmpv-android-video-build/archive/refs/tags/v1.1.7.tar.gz | `libmpv-android-video-build-1.1.7.tar.gz` | `6624ea32232f7a8d6d07ab7a7bc7b42c16f85fc50c4e3ecfd174b52636d9252a` |

Where it is used: libavcodec, libavformat, libavfilter, libavutil, libswresample, libswscale; used by libmpv to decode video. iOS: separate dynamic frameworks (e.g. Avcodec.framework). Android: linked into `libmpv.so`.

The app ships the **prebuilt media-kit binaries unchanged**:

- iOS: Flutter package `media_kit_libs_ios_video` 1.1.4, which downloads `libmpv-xcframeworks_v0.6.0_ios-universal-video-default.tar.gz` built by libmpv-darwin-build v0.6.0 (variant `video`, flavor `default`). Exact source versions and checksums are in `downloads.lock` of that build repository.
- Android: Flutter package `media_kit_libs_android_video` 1.3.8, which downloads `default-<abi>.jar` from the v1.1.7 release of libmpv-android-video-build, built by libmpv-android-video-build v1.1.7 (`buildscripts/include/depinfo.sh`, `buildscripts/include/download-deps.sh`, `buildscripts/flavors/default.sh`).

## Patches

Copies are in `patches/` of this repository and in the release:

- `patches/ios/ffmpeg-fix-vp9-hwaccel.patch`
- `patches/ios/ffmpeg-fix-hls-mp4-seek.patch`
- `patches/ios/ffmpeg-fix-ios-hdr-texture.patch`
- `patches/ios/ffmpeg-fix-dash-base-url-escape.patch`
- `patches/android/dash_base_url_escape.patch`
- `patches/android/hls_mp4_seek.patch`

iOS: applied by `scripts/ffmpeg/build.sh` of libmpv-darwin-build (release files `ios-<name>`). Android: applied by `buildscripts/patch.sh` from `buildscripts/patches/ffmpeg/` of libmpv-android-video-build (release files `android-ffmpeg-<name>`). The two Android patches are byte-identical to the iOS DASH/HLS patches.

## How to rebuild

iOS (macOS host):

```sh
tar -xzf libmpv-darwin-build-0.6.0.tar.gz
cd libmpv-darwin-build-0.6.0
brew install cmake golang meson ninja
VERSION=v0.6.0 make
# result: build/output/libmpv-xcframeworks_v0.6.0_ios-universal-video-default.tar.gz
```

The build downloads the sources listed in `downloads.lock` (the same files as in this release) and verifies their SHA-256. To build offline, place the release tarballs where the build expects its downloads.

Android (Linux host, see the build repository's own workflow):

```sh
tar -xzf libmpv-android-video-build-1.1.7.tar.gz
cd libmpv-android-video-build-1.1.7/buildscripts
./bundle_default.sh   # download.sh + patch.sh + build.sh with flavors/default.sh
```

To replace the library in the app with a modified version, rebuild it and put the resulting framework / `libmpv.so` in place of the one in the app (iOS: the dynamic framework inside `Futigo.app/Frameworks`; Android: `lib/<abi>/libmpv.so` in the APK).

## Source on request

You can also ask for a copy of this source code: **info@futigo.app**. This offer is valid for at least 3 years from the last distribution of the app version that contains the library.

## Licenses

- [`licenses/LGPL-3.0.txt`](licenses/LGPL-3.0.txt)
- [`licenses/GPL-3.0.txt`](licenses/GPL-3.0.txt)
- [`licenses/LGPL-2.1.txt`](licenses/LGPL-2.1.txt)
- [`licenses/FFmpeg-LICENSE.md`](licenses/FFmpeg-LICENSE.md)

---

## Po polsku (skrót)

To repozytorium zawiera pełny kod źródłowy biblioteki FFmpeg (LGPL-3.0-or-later) w wersjach używanych w aplikacji Futigo (iOS i Android), od wersji 2.0.0: niezmienione źródła autorów, poprawki (patche) projektu media-kit i jego skrypty budowania. Aplikacja zawiera gotowe, niezmienione pliki binarne media-kit. Pliki są w wydaniu (release) `app-2.0.0-sources`, z sumami kontrolnymi `SHA256SUMS`. Kod źródłowy można też otrzymać na prośbę: **info@futigo.app** (co najmniej 3 lata od ostatniej dystrybucji danej wersji aplikacji).
