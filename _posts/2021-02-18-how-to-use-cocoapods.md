---
date: 2021-02-18 05:20:21
layout: post
title: "how to use cocoapods"
subtitle: using cocoapods for mananging dependencies when iOS app development
description:
image: /assets/postimg/cocoapod.webp
optimized_image: /assets/postimg/cocoapod.webp
category: ios
tags:
    - iOS
    - cocoapods
    - podfile
    - library-dependencies
author: Klaus
paginate: false
---

# cocoapods 사용하기 



## 1. cocoapod 이란?

### cocoapods?

> what is cocoapods
>
> - CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects. It has over 80 thousand libraries and is used in over 3 million apps. CocoaPods can help you scale your projects elegantly.

iOS 앱 개발시, 다양한 **3rd party library/open source**를 사용할 때가 있다. 이 때 가장 널리 사용되고 있는 라이브러리 매니져인 `cocoapods` 을 설치하고 사용해보자.



## 2. 설치

### Cocoapods 설치

- `cocoapods` 을 설치하자.

``` zsh
sudo gem install cocoapods
```



- 만일 설치가 안된다면, 다음을 실행해보자.

``` zsh
gem list | grep cocoapods
gem uninstall cocoapods

brew install cocoapods
```



## 3. 사용

### `Podfile` 생성

- 설치가 완료되고 나서, 프로젝트 디렉토리로 이동한 후,  `pod init` 을 통해 `Podfile` 을 만들자.

``` zsh
pod init
```

> 위 명령어를 실행하고 나면, 프로젝트 디렉토리에 `Podfile` 이 생성되어 있다. 해당 파일을 원하는 텍스트 에디터로 열고, 설치하고자 하는 `라이브러리, 버전`  등을 기록하고, 추후 `pod install` 로 라이브러리 설치를 하자.



### `Podfile` 설정

- 생성된 Podfile의 예시는 다음과 같다. 하기 `#####` 사이에 관리하고자 하는 라이브러리를 기록하고 저장하자.

``` zsh
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'MyProjectTitle' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!

#########################
# 여기에 원하는 라이브러리(+버전)을 기록하면 된다.
pod 'ProgressHUD'
pod 'Kingfisher'
#########################


  # Pods for MyProjectTitle

  target 'MyProjectTitle' do
    inherit! :search_paths
    # Pods for testing
  end

  target 'MyProjectTitle' do
    # Pods for testing
  end

end
```



### `pod install`

- `pod install` 을 통해, `Podfile` 에 기록해두었던 라이브러리를 진행할 프로젝트에 설치하자.

``` zsh
pod install
```

> `내프로젝트명.xcworkspace` 이라는 흰색 아이콘의 워크스페이스가 생성된 것을 확인할 수 있다. 이 워크스페이스를 더블클릭해서 실행해보자. 앞으로 이 워크스페이스를 통해서 작업을하면 된다. 





### `import 라이브러리`

- 이제 사용하고자 하는 라이브러리를 원하는 파일에서 import 해서 사용하면 된다.

``` swift
// @ your-swiftfile in working directory
import ProgressHud
import Kingfisher
```







### how to use

- Xcode project에 `Pods` 을 추가하자
  - `Podfile` 을 만들어서 dependencies를 추가하자.
- 프로젝트 디렉토리로 이동하여 `$ pod install` 실행
- `App.xcworkspace`  를 실행해서 개발에 사용하자.









