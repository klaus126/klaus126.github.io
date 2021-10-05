---
date: 2021-10-05 14:13:28
layout: post
title: "Sound null error - flutter swiper dependency issue, MacOS 13 over"
subtitle: "vscode shell changed, flutter path, sound-null probelm"
description: "issue handling after installing flutter swiper"
image: https://images.unsplash.com/photo-1562450840-34e013fadeee?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=735&q=80
optimized_image: https://images.unsplash.com/photo-1562450840-34e013fadeee?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=735&q=80
category: Flutter
tags:
    - Flutter
    - Environment
    - troubleshooting
author: Klaus
paginate: false
---



# Sound null error - flutter swiper dependency issue.







## Sound null error

After install `flutter swiper` default shell of vscode has changed `zsh` (MacOS default shell) to bash shell.

Also, After changing to bash shell, due to flutter swiper issue, flutter path probelm is occured.

- VS code default shell has chaged
- flutter path broken
- sound-null-problem when running a simulator



### 1. VS code shell issue

> MacOS default shell : zsh

- cause: install `flutter swiper` at `pubspec.yaml`
- effect: VS code default shell has changed to `bash` shell
- how to handle it.
  - change the default shell
  - `code` > `Preferences` > `Settings`
    - `User` > `Feature` > `Terminal`
      - `Edit in settings.json`

```
"terminal.integrated.shell.(your-os)": "bin/zsh"
```



### 2. flutter path issue

- cause: shell changed (I guess)
- effect: Flutter path broken, cannot use `flutter` command not found.
- how to handle it.
  - path setting

```
export PATH="$PATH:/YOUR/LOCATION/FLUTTER/development/flutter/bin"
```



### 3. sound-null-problem

- cause: dependency problem
- effect: `flutter swiper dependency error sound null safety` error occurs.
- how to handle it.

```
flutter run --no-sound-null-safety 
```



