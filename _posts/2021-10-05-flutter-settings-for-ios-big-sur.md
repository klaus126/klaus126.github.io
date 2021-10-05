---
date: 2021-10-05 07:35:30
layout: post
title: "Flutter settings for iOS Big Sur"
subtitle: "Flutter initial settings"
description: "How to set Flutter"
image: https://images.unsplash.com/photo-1484417894907-623942c8ee29?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1632&q=80
optimized_image: https://images.unsplash.com/photo-1484417894907-623942c8ee29?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1632&q=80
category: Flutter
tags: 
    - Flutter
    - Environment
author: Klaus
paginate: false
---



# Flutter Setting

cf. [Flutter Official Install Guide](https://flutter.dev/docs/get-started/install/macos)



### System status

```
- MacOS Big Sur version 11.6
- intel processor
```





### 1. Get the Flutter SDK

1. Download stable release of the Flutter SDK

   - [Flutter SDK Official Download Link](https://storage.googleapis.com/flutter_infra_release/releases/stable/macos/flutter_macos_2.5.2-stable.zip)

2. Extract the file in the directory you want 

   - Flutter's official guide makes a *development* directory just below the home dir(`~/`).

     ```
     cd ~
     mkdir development
     ---------(above: if you don`t have development directory below the home)----
     cd ~/development
     unzip ~/Downloads/flutter_macos_2.5.2-stable.zip
     ```

3. Path setting

   - `pwd` means current locations.

     - If your current directory is '~/development', then it means '~/development' directory.
     - If you installed Flutter SDK in another directory, then you could replace `pwd` with your installed directory.

     ```
     export PATH="$PATH:`pwd`/flutter/bin"
     ```

     



### 2. Run Flutter Doctor

> flutter doctor will diagnosis your setting

![flutter docter diagnosis](https://i.ibb.co/F7Qh1wg/2-android-SDK-not-found.png)





### 3. Handling Dependency: Android Studio

1. Download & Install Android Studio (It takes more than 1 hour for me.)
   - [Download Android Studio](https://developer.android.com/studio)
   - run 'Android Studioo Setup Wizard', this installs below (the leatest version) 
     - `Android SDK`
     - `Android SDK Command-line Tools`
     - `Android SDK Build-Tools`
   - Before you start installing, you can set SDK Folder & JDK location. 
2. Set up your Android device
   - @your android device,
     - `settings > software information`, tab the `build number` button several times until the `PIN CODE` pop-up appears.
     - enter your device password or pattern.
       - You could see the 'Developer mode enable' message
     - Now, go back to `Settings` menu
       - You coud see the `Developer options` menu.
     - Tap the switch to enable/disable the Developer options menu.
   - connect your android device to your computer.





### 4. Handling Dependency: Android Toolchain

1. run `flutter doctor --android-licenses` at your terminal

   ![2. android SDK not found](https://i.ibb.co/F7Qh1wg/2-android-SDK-not-found.png)

   - if you got this message(Android sdkmanager not found), handling it this way.

     ### A. Setting 'Android SDK Tools'

     - first, run 'Android Studio' and click 'More Actions'.

       ![3. Android studio - sdk setting](https://i.ibb.co/dWppgJy/3-Android-studio-sdk-setting.png)

     - uncheck 'Hide Obsolete Packages'

       ![4. Android SDK setting - uncheck box](https://i.ibb.co/ChnCqVC/4-Android-SDK-setting-uncheck-box.png)

     - check 'Android SDK Tools (Obsolete)'

       - ![5. Android SDK setting - check Android sdk tool obsolete](https://i.ibb.co/8NwWjj5/5-Android-SDK-setting-check-Android-sdk-tool-obsolete.png)

     - Component Installer will appear. 

       ![6. SDK installation](https://i.ibb.co/pRQpB0Z/6-SDK-installation.png)

   

   	### B. Android SDK Command-line Tools (latest)

   - check 'Android SDK Command-line Tools (latest)'

     ![7. Android SDK Command-line Tools latest install](https://i.ibb.co/NtcqTzr/7-Android-SDK-Command-line-Tools-latest-install.png)

   - run `flutter doctor --android-licenses` at your terminal again.

