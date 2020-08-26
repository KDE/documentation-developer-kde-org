---
title: "Building applications for Android"
linkTitle: "Building applications for Android"
weight: 2
description: >
  Learn how to build your applications for Android
---


## Building Applications

Building .apk files from Qt Applications requires a cross-compiling toolchain, which is hard to setup. For these reasons, there is a ready-to-use Docker Container for building KDE applications. While we recommend using this container, it is possible to build applications on the host environment by setting up a cross-compiling toolchain.

The container contains Qt binaries for arm and arm64, the KDE frameworks and most other dependencies are built as part of the application builds.

The container can be started with

```bash
docker run -ti --rm kdeorg/android-sdk bash
```

An application can be built by executing

```bash
docker run -ti --rm -v /tmp:/output kdeorg/android-sdk /opt/helpers/build-generic okular
```

The generated apks will be available in the ```/tmp``` directory of the host system.

When building a project with local patches, the ```src``` directory needs to be added as a volumn to the ```docker run``` command, e.g.:

```bash
docker run -ti --rm -v $HOME/kde/src:/home/user/src -v /tmp:/output kdeorg/android-sdk /opt/helpers/build-generic okular
```
