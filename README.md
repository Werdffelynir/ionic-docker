# Ionic-docker
A ionic 1/2 image to be used with Gitlab CI

### Features
- Node
- Npm or Yarn
- Ionic
- Cordova
- Android sdk and build tools
- Ready to run Google Chrome Headless for e2e tests
- Ruby
- Scss-lint support

##Usage

```
git clone https://github.com/Werdffelynir/ionic-docker ./ionic-docker

docker build --rm -t ionic-image ./ionic-docker

docker run -ti --rm -p 8100:8100 -p 35729:35729 -v /path/to/ionic_project:/sources:rw ionic-image

```

### Automation
With this alias:

```
alias ionic="docker run -ti --rm -p 8100:8100 -p 35729:35729 --privileged -v /dev/bus/usb:/dev/bus/usb -v ~/.gradle:/root/.gradle -v \$PWD:/myApp:rw ionic-image"
```

> Due to a bug in ionic, if you want to use ionic serve, you have to use --net host option :

```
alias ionic="docker run -ti --rm --net host --privileged -v /dev/bus/usb:/dev/bus/usb -v ~/.gradle:/root/.gradle -v \$PWD:/myApp:rw marcoturi/ionic ionic"
```

> Know you need gradle for android, I suggest to mount ~/.gradle into /root/.gradle to avoid downloading the whole planet again and again

you can follow the [ionic tutorial](http://ionicframework.com/getting-started/) (except for the ios part...) without having to install ionic nor cordova nor nodejs on your computer.

```bash
ionic start myApp tabs
cd myApp
ionic serve
# If you didn't used --net host, be sure to chose the ip address, not localhost, or you would not be able to use it
```
open http://localhost:8100 and everything works.

### Android tests
You can test on your android device, just make sure that debugging is enabled.

```bash
cd myApp
ionic cordova platform add android
ionic cordova build android
ionic cordova run android
```

### FAQ
* The application is not installed on my android device
    * Try `docker run -ti --rm -p 8100:8100 -p 35729:35729 --privileged -v /dev/bus/usb:/dev/bus/usb -v \$PWD:/myApp:rw ionic-image adb devices` your device should appear
* The adb devices show nothing whereas I can see it when I do `adb devices` on my computer
    * You can't have adb inside and outside docker at the same time, be sure to `adb kill-server` on your computer before using this image


### Inspired by:
- https://github.com/marcoturi/ionic-docker
- https://github.com/procoders/ionic-docker
- https://hub.docker.com/r/webnicer/protractor-headless/~/dockerfile/
- https://github.com/agileek/docker
- http://stackoverflow.com/questions/29558444/angularjs-grunt-bower-gitlab-ci-setup-for-testing
- https://github.com/tippiq/docker-protractor
