# A Basic Flutter App

This is a very basic basic flutter app that displays a persons name. This app was created as part of AGL GSoC 2023 quiz.

## AGL Integration Details

Following steps were taken to integrate this app into AGL image.

### 1. Build AGL locally
The 'agl-ivi-demo-platform-flutter' of AGL octupus branch was built locally and tested using QEMU.

### 2. Add Yocto recipe for our flutter app
Under the `recipes-demo` folder i created a new directory named `flutter-quizapp`. The flutter-quizapp has `.bb` file containing following recipe:
```
SUMMARY = "Flutter Quiz App"
DESCRIPTION = "A Flutter based IVI Dashboard Application for automotive grade Linux."

HOMEPAGE = "https://github.com/malik727/agl-flutter-quiz-app"

BUGTRACKER = "https://github.com/malik727/agl-flutter-quiz-app/issues"

SECTION = "graphics"

LICENSE = "CLOSED"


SRC_URI = "git://github.com/malik727/agl-flutter-quiz-app.git;protocol=https;branch=main \
    "
SRCREV = "${AUTOREV}"
S = "${WORKDIR}/git"

inherit agl-app flutter-app

# flutter-app
#############
PUBSPEC_APPNAME = "agl_flutter_quiz_app"
FLUTTER_APPLICATION_INSTALL_PREFIX = "/flutter"
FLUTTER_BUILD_ARGS = "bundle -v"

# agl-app
#########
AGL_APP_TEMPLATE = "agl-app-flutter"
AGL_APP_ID = "agl_flutter_quiz_app"
AGL_APP_NAME = "Flutter Quiz App"
```

### 3. Update packagegroup to include our app
In order to display our app inside the IVI we need to include it inside `recipes-platform` packagegroups. The `packagegroup-agl-demo-platform-flutter.bb` file was updated as follows:
```
.
.
.
AGL_APPS = " \
    flutter-dashboard-${AGL_FLUTTER_RUNTIME} \
    flutter-hvac-${AGL_FLUTTER_RUNTIME} \
    flutter-quizapp-${AGL_FLUTTER_RUNTIME} \
    ondemandnavi \
    settings \
    mediaplayer \
    messaging \
    phone \
    radio \
    "
.
.
.
```

### 4. Build the image to again
In order for our changes to be reflected we need to re-build the updated recipes and then build the entire image again. Following commands will acheive this:
```
$ source agl-init-build-env
$ bitbake flutter-quizapp
$ bitbake packagegroup-agl-demo-platform-flutter
$ bitbake agl-ivi-demo-platform-flutter
```
This will take a couple of minutes depending upon system speed.

### 5. Running our image using qemu
Now its time to see if our newly added app works as intended or not. We'll run the following command to boot the image:
```
$ runqemu kvm serialstdio slirp publicvnc
```
The AGL image is running in background, we can use Vinagre to open it. Below is the screenshot showing our "Flutter Quiz App" inside AGL.

![Quiz App Pic 01](https://i.ibb.co/Bf2RdjR/image.png "")

After opening the app we can see my name.

![Quiz App Pic 01](https://i.ibb.co/MCcwbFf/image-1.png "")
