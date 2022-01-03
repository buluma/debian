# Debian Base Images

[![Build bookworm](https://github.com/buluma/debian/actions/workflows/build-bookworm.yml/badge.svg)](https://github.com/buluma/debian/actions/workflows/build-bookworm.yml) [![Build bullseye](https://github.com/buluma/debian/actions/workflows/build-bullseye.yml/badge.svg)](https://github.com/buluma/debian/actions/workflows/build-bullseye.yml) [![Build buster](https://github.com/buluma/debian/actions/workflows/build-buster.yml/badge.svg)](https://github.com/buluma/debian/actions/workflows/build-buster.yml) [![Build experimental](https://github.com/buluma/debian/actions/workflows/build-sid.yml/badge.svg)](https://github.com/buluma/debian/actions/workflows/build-sid.yml) [![Build jessie](https://github.com/buluma/debian/actions/workflows/build-jessie.yml/badge.svg)](https://github.com/buluma/debian/actions/workflows/build-jessie.yml) [![Build stretch](https://github.com/buluma/debian/actions/workflows/build-stretch.yml/badge.svg)](https://github.com/buluma/debian/actions/workflows/build-stretch.yml) [![Build testing](https://github.com/buluma/debian/actions/workflows/build-testing.yml/badge.svg)](https://github.com/buluma/debian/actions/workflows/build-testing.yml) [![Build unstable](https://github.com/buluma/debian/actions/workflows/build-unstable.yml/badge.svg)](https://github.com/buluma/debian/actions/workflows/build-unstable.yml)

## Quick reference
Maintained by: Debian Developers tianon and paultag

Where to get help: the Docker Community Forums, the Docker Community Slack, or Stack Overflow

## Supported tags and respective Dockerfile links
bookworm, bookworm-20211220
bookworm-backports
bookworm-slim, bookworm-20211220-slim
bullseye, bullseye-20211220, 11.2, 11, latest
bullseye-backports
bullseye-slim, bullseye-20211220-slim, 11.2-slim, 11-slim
buster, buster-20211220, 10.11, 10
buster-backports
buster-slim, buster-20211220-slim, 10.11-slim, 10-slim
experimental, experimental-20211220
oldoldstable, oldoldstable-20211220
oldoldstable-backports
oldoldstable-slim, oldoldstable-20211220-slim
oldstable, oldstable-20211220
oldstable-backports
oldstable-slim, oldstable-20211220-slim
rc-buggy, rc-buggy-20211220
sid, sid-20211220
sid-slim, sid-20211220-slim
stable, stable-20211220
stable-backports
stable-slim, stable-20211220-slim
stretch, stretch-20211220, 9.13, 9
stretch-backports
stretch-slim, stretch-20211220-slim, 9.13-slim, 9-slim
testing, testing-20211220
testing-backports
testing-slim, testing-20211220-slim
unstable, unstable-20211220
unstable-slim, unstable-20211220-slim

## Quick reference (cont.)
Where to file issues: https://github.com/debuerreotype/docker-debian-artifacts/issues

Supported architectures: (more info) amd64, arm32v5, arm32v7, arm64v8, i386, mips64le, ppc64le, riscv64, s390x

Published image artifact details: repo-info repo's repos/debian/ directory (history) (image metadata, transfer size, etc)

Image updates: official-images repo's library/debian label
official-images repo's library/debian file (history)

Source of this description: docs repo's debian/ directory (history)

## What is Debian?
Debian is an operating system which is composed primarily of free and open-source software, most of which is under the GNU General Public License, and developed by a group of individuals known as the Debian project. Debian is one of the most popular Linux distributions for personal computers and network servers, and has been used as a base for several other Linux distributions.

wikipedia.org/wiki/Debian

## About this image
The debian:latest tag will always point the latest stable release (which is, at the time of this writing, debian:buster). Stable releases are also tagged with their version (ie, debian:9 is an alias for debian:stretch, debian:8 is an alias for debian:jessie, etc).

The rolling tags (debian:stable, debian:testing, etc) use the rolling suite names in their /etc/apt/sources.list file (ie, deb http://deb.debian.org/debian testing main).

The mirror of choice for these images is the deb.debian.org CDN pointer/redirector so that it's as reliable as possible for the largest subset of users (and is also the default mirror for debootstrap as of 2016-10-20). See the deb.debian.org homepage for more information.

If you find yourself needing a Debian release which is EOL (and thus only available from archive.debian.org), you should check out the debian/eol image, which includes tags for Debian releases as far back as Potato (Debian 2.2), the first release to fully utilize APT.

## Locales
Given that it is a faithful "minbase" install of Debian, this image only includes the C, C.UTF-8, and POSIX locales by default. For most uses requiring a UTF-8 locale, C.UTF-8 is likely sufficient (-e LANG=C.UTF-8 or ENV LANG C.UTF-8).

For uses where that is not sufficient, other locales can be installed/generated via the locales package. PostgreSQL has a good example of doing so, copied below:

RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8
How It's Made
The rootfs tarballs for this image are built using the reproducible-Debian-rootfs tool, debuerreotype, with an explicit goal being that they are transparent and reproducible. Using the same toolchain, it should be possible to regenerate (clean-room!) the same tarballs used for building the official Debian images. The examples/debian.sh script in that debuerreotype repository (and the debian-all.sh companion/wrapper) is the canonical entrypoint used for creating the artifacts published in this image (via a process similar to the docker-run.sh included in the root of that repository).

Additionally, the scripts in https://github.com/debuerreotype/docker-debian-artifacts are used to create each tag's Dockerfile and collect architecture-specific tarballs into dist-ARCH branches on the same repository, which also contain extra metadata about the artifacts included in each build, such as explicit package versions included in the base image (rootfs.manifest), the exact snapshot.debian.org timestamp used for debuerreotype invocation (rootfs.debuerreotype-epoch), the sources.list found in the image (rootfs.sources-list) and the one used during image creation (rootfs.sources-list-snapshot), etc.

For convenience, the SHA256 checksum (and full build command) for each of the primary rootfs.tar.xz artifacts are also published at docker.debian.net.

## Image Variants
debian:<suite>-slim
These tags are an experiment in providing a slimmer base (removing some extra files that are normally not necessary within containers, such as man pages and documentation), and are definitely subject to change.

See the debuerreotype-slimify script (debuerreotype linked above) for more details about what gets removed during the "slimification" process.
