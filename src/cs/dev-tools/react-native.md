# React Native Tutorial

### 1 Introduction

React Native is the proper choice for cross-platform developing. As the slogan, "Learn once write anywhere", shows, React Native project can run in iOS, android, web. Apart from developing mobile applcations, we can also create software for Windows, MacOS, and Linux in the almost same code framework.

In this tutorial, I use **Ubuntu 20.04** to write code and run the application

### 2 Basic Part: React Native CLI

In this part, we will set up React Native CLI project. Users in Windows and MacOS can access [this link](https://reactnative.dev/docs/environment-setup) for guide.

**Attention**: in Windows and Linux, React Native CLI cannot build projects with native code for iOS, but we can build our iOS app with Expo CLI (introduced in 3. Advanced Part)

#### 2.1 Environment setup

In this totural, I will build android applcation in Ubuntu 20.04.

#### 2.1.1 Installing dependencies

**Node**

Follow the [installation instructions for your Linux distribution](https://nodejs.org/en/download/package-manager/) to install Node if you are not using Ubuntu or Debian Distro.

Installing node:

```shell
sudo apt install nodejs
```

verify it by checking the installed version using the following command:

```shell
node -v
```

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405121144.png)

**JDK**

In Ubuntu:

```shell
sudo apt install default-jre
sudo apt install default-jdk
```

React Native requires at least the version 8 of the Java SE Development Kit (JDK). If you are using other operating system, download and install [OpenJDK](http://openjdk.java.net/) from [AdoptOpenJDK](https://adoptopenjdk.net/) or your system packager. You may also [Download and install Oracle JDK 14](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html) if desired.

**Android development environment**

**(1) Install Android Studio**

install the android studio IDE from [https://developer.android.com/studio](https://developer.android.com/studio)

**(2) Install Android SDK**

After Installing Android Studio, we need install Android SDK from Android Studio

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405122027.png)

Open SDK Manager.

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405122140.png)

Click `Android 11.0(R)` and click Apply to install it.

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405122353.png)

And then, select the "SDK Tools" tab and check the box next to "Show Package Details". Make sure that `30.0.2` is selected. Click apply to install.

**(3) Configure the ANDROID\_SDK\_ROOT environment variable**

Add the following lines to your `$HOME/.bashrc`

I am using `zsh`, so I edit `~/.zshrc` and add the following lines:

(**Attention**: the folloing lines are differet from official tutorals. Also, difference Operating Systems have different android sdk location. Please be careful in this step)

```shell
export ANDROID_HOME=$HOME/Android/Sdk
export ANDROID_SDK_ROOT=$HOME/Android/Sdk
export ANDROID_SDK=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

**(4) Install Android Virtual Devices (AVDs)**

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405124712.png)

Install Virtual Device

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405124854.png)

**(5) Watchman**

Follow the [Watchman installation guide](https://facebook.github.io/watchman/docs/install/#buildinstall) to compile and install Watchman from source.

I use `brew` to install `watchman`

```shell
brew install watchman
```

If you are not familiar with brew, access the official website for more information: [https://brew.sh/](https://brew.sh/)

#### 2.2 Build Android App with React Native

Create a new application:

```shell
npx react-native init tutorial
```

After initialize the project, open two terminals.

(1) In one terminal:

```shell
npx react-native start
```

(2) In the other terminal

```shell
npx react-native run-android
```

Executing result:

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405134116.png)

#### 2.3 Build iOS App with React Native

NOT SUPPORT IN UBUNTU WITH REACT NATIVA CLI

#### 2.4 Simple Example of Calculator

Enter 3 numbers and return the sum of them.

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405163815.png)

source code:

```
//omit
```

### 3. Advanced Part: Expo CLI

#### 3.1 Introduction to Expo CLI

We introduce the knowledge of React Native CLI in part 2 (Basic Part). In this part, we will step further and do something awesome with expo.

Similar with React Native, Expo is also a framework to build cross-platform apps. It has many tools and services built for React Native. We can even develop iOS application with Expo in Linux or Windows.

#### 3.2 Expo-cli Example in Android Virtual Device

Install expo-cli in ubuntu

```shell
yarn global add expo-cli
```

Initialize expo project

```shell
expo init expo_tutorial
```

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220410233434.png)

Run the expo application

```shell
cd expo_tutorial
yarn start
```

* Open web

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220423002345.png)

* Open Android virural device

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220423002320.png)

* Open in iOS physical device (scan the QR code)

Install expo app from Appstore first

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405170735.png)

#### 3.3 Simple example on expo cli

* Calculator on Android Virtual Device

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220504152550.png)

* Calculator in iOS:

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/photo\_2022-05-04\_15-28-58.jpg)

### Troubleshooting

* Android SDK is not found

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220405133340.png)

This problem is wired. I have installed everything, including `31.0.0` Android SDK, react-native doctor still gives this report. **Actually, I can run android virtual device successfully, so this report does not matter**.

However, if you cannot run android virtual device due to the missing Android SDK, try the following solution:

(1) In `android/` directory

(2) Create file named `local.properties`

(3) Add this line to `local.properties`:

```shell
sdk.dir = /Users/USERNAME/Library/Android/sdk # for MacOS
```

or

```shell
sdk.dir = $HOME/Android/Sdk # for Linux
```

* Permission denied error

Changing owner of the while project files can solve this problem.

```shell
sudo chown -R $USER .
```

* Node version problem

If you have some problem with the nodejs version, try to manage node verions with `nvm`: [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

In this tutorial, I choose `node v16.14.2`

```shell
nvm use 16
```

### References

[React Native](https://reactnative.dev/)

[https://reactnative.dev/docs/environment-setup](https://reactnative.dev/docs/environment-setup)

[https://stackoverflow.com/questions/51622425/react-native-running-in-emulator-gives-bundling-failed-permission-denied-error](https://stackoverflow.com/questions/51622425/react-native-running-in-emulator-gives-bundling-failed-permission-denied-error)

[https://stackoverflow.com/questions/32634352/react-native-android-build-failed-sdk-location-not-found](https://stackoverflow.com/questions/32634352/react-native-android-build-failed-sdk-location-not-found)
