# AeroGear Digger Build Containers

**_Deprecated:_** This repository is deprecated and no longer maintained!

[![](https://img.shields.io/docker/automated/jrottenberg/ffmpeg.svg)](https://hub.docker.com/r/aerogear/mobile-sdk/)
[![Docker Stars](https://img.shields.io/docker/stars/aerogear/mobile-sdk.svg?style=plastic)](https://registry.hub.docker.com/v2/repositories/aerogear/mobile-sdk/stars/count/)
[![Docker pulls](https://img.shields.io/docker/pulls/aerogear/mobile-sdk.svg?style=plastic)](https://registry.hub.docker.com/v2/repositories/aerogear/mobile-sdk/)

[![Build Status](https://travis-ci.org/aerogear/digger-build-containers.png)](https://travis-ci.org/aerogear/digger-build-containers)
[![License](https://img.shields.io/:license-Apache2-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)

Building mobile applications inside a Linux container has benefits of a true isolation, for each build. This repository contains Docker-formatted container images for building supported mobile platforms:
* Android SDK
* Apache Cordova CLI (Version 6)

## Project Info

|                 | Project Info  |
| --------------- | ------------- |
| License:        | Apache License, Version 2.0  |
| Build:          | Docker  |
| Documentation:  | tbd  |
| Issue tracker:  | https://issues.jboss.org/browse/AGDIGGER  |
| Mailing lists:  | [aerogear-users](http://aerogear-users.1116366.n5.nabble.com/) ([subscribe](https://lists.jboss.org/mailman/listinfo/aerogear-users))  |
|                 | [aerogear-dev](http://aerogear-dev.1069024.n5.nabble.com/) ([subscribe](https://lists.jboss.org/mailman/listinfo/aerogear-dev))  |
| IRC:            | [#aerogear](https://webchat.freenode.net/?channels=aerogear) channel in the [freenode](http://freenode.net/) network.  |


## Prerequisites

The following gives a quick introduction, how to get started using our _AeroGear Digger Build Containers_!

### Platform container

First you need a _Platform container_, which provides the desired mobile platform, wrapped in a Linux container.

#### Android Platform container

For our Android (and Cordova-Android) testing we have used containers provided by [@appunite](https://github.com/appunite/docker). The project has images on [Docker Hub](https://hub.docker.com/r/jacekmarchwicki/android/tags/), but you can build them locally as well.

### Platform volume

Once a Platform container is availble it needs to be used for the creation of the _Platform volume_:

```
docker create -v /opt/android-sdk-linux --name my-android-volume jacekmarchwicki/android:java8-r24-4-1 /bin/true
```

_NOTE:_ After some time, you will have your _Platform volume_, called `my-android-volume`. 


## Build container

_NOTE:_ Currently we have no images on Docker Hub....

With the prerequisites being fulfilled, it's time to build the _Build container_. Go to the `0.1` folder and execute:

```
docker build -t aerogear/mobile-sdk . 
```

## Building an App inside of a Linux container

Last but not least you need a folder containing the source code of your (Android) mobile application, the following command runs a build inside of a Linux container, using the above containers and volumes:

```
docker run -it -v /path/to/my/android_or_cordova-app/:/app:z --volumes-from my-android-volume \
   -e AG_MOBILE_SDK=/opt/android-sdk-linux \
   -e ANDROID_HOME=/opt/android-sdk-linux \
   -e PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/mobile-sdk/tools:/opt/android-sdk-linux/tools \
   aerogear/mobile-sdk build

```

### Tested applications

We have tested a few (Android) applications that work with our _Build container_:

* [The FeedHenry Android template apps](https://github.com/feedhenry-templates?utf8=%E2%9C%93&query=android)
* [The FeedHenry Cordova template apps](https://github.com/feedhenry-templates?utf8=%E2%9C%93&query=cordova-app)
* [Google IO Schedule App](https://github.com/matzew/iosched/tree/changes)
* [DevNexus App](https://github.com/matzew/devnexus-android/tree/dummy-google-services)
* [Memory Game](https://github.com/sromku/memory-game) _random Gradle based OSS project/game_

Have fun!
