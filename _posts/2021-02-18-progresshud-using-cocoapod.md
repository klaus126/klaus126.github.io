---
date: 2021-02-18 06:32:21
layout: post
title: "ProgressHUD using cocoapod"
subtitle: make a loading bar at my app using ProgressHUD
description:
image: /assets/postimg/progressHUD.webp
optimized_image: /assets/postimg/progressHUD.webp
category: ios
tags:
    - iOS
    - progressHUD
    - cocoapods
author: Klaus
paginate: false
---



# ProgressHUD 사용하기



[ProgressHUD github](https://github.com/relatedcode/ProgressHUD)

``` zsh
pod 'ProgressHUD'
```



## 설치하기



### `Podfile` 설정

- `Podfile` 에 사용할 라이브러리인 `ProgressHUD` 를 명시하자.

``` zsh
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'MyProjectTitle' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!

#########################
# 여기에 원하는 라이브러리(+버전)을 기록하면 된다.
pod 'ProgressHUD'
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



### `pod install`  을 통해, 라이브러리를 설치하자

``` zsh
pod install
```











## 사용하기

 사용하는 방법은 앱을 어떻게 만들것인지에 따라 매우 다양할 수 있다. 필자는 앱을 실행하고, 데이터를 로딩하는 동안 progressbar가 동작하도록 구성할 예정이다. 데이터를 로딩하는 순서를 생각해보면 다음과 같다.

- 앱 실행
- 뷰 로드
- 원하는 앱 기능 사용

  iOS app lifecycle에 따르면, 상기 사항 중 뷰가 로딩되고 나서 데이터가 로딩이 될 것이다. 즉, 사용할 ViewController 에서 view가 로딩되고나서 바로 실행할 것이므로, viewDidLoad() 함수에 명시해둬야할 것이다. 현재 뷰를 로딩할 떄, 데이터를 가져올 것이므로, 이를 구조적으로 본다면 다음과 같다.

``` swift
// @ MyViewController.swift
class MyViewController: UIViewController {
  // IBOutlet setting (생략)
  
  // viewDidLoad()
  override func viewDidLoad(){
    super.viewDidLoad()
    
    // view가 로딩되고 데이터를 가져올 것이므로,
    getData()
  }
}
```

>  상기 사항과 같이 별도의 `getData()` 함수를 만들어서 관리하려고 한다. 이런 경우,`viewDidLoad()` 함수가 실행되면서, `getData()` 함수를 실행시키기에  `getData()` 함수 안에 `progressbar` 를 설정해둔다면, 원하는대로 구현될 것이다. 한번 `getData()` 함수를 구현해보자. 





``` swift
// @ MyViewController.swift
class MyViewController: UIViewController {
  // IBOutlet setting (생략)
  
  // viewDidLoad()
  override func viewDidLoad(){
    super.viewDidLoad()
    
    // view가 로딩되고 데이터를 가져올 것이므로,
    getData()
  }
  
  func getData() {
    // network
    // n-i. URL 형변환
    let myURL = URL(string: originalURL)
    
    // n-ii. URL -> request 형태로 형변환
    let requestURL = URLRequest(url: myURL!)
    
    
    // n-iii. 접속
    URLSession.shared.dataTask(with: requestURL) { [self] (data, response, error) in
                                                 if error != nil {
                                                   print(error?.localizedDescription)
                                                   return
                                                 }
                                                  
                                                  //data parsing
                                                  self.my = self.parsingJsonData(data: data!)
                                                  
                                                  //finally, tableview renew
                                                  OperationQueue.main.addOperation {
                                                    self.tableView.reloadData()
                                                    
                                                  }
                                                 }.resume()
  }
}
```

> 구현된 코드를 확인해보자면, 
>
> - view가 로딩되고, `getData()` 함수를 실행한다.
> - `getData()` 함수 실행시, 
>   - url을 string으로 변경하고 이를 URLRequest 형태로 변환한다.
>   - 이를 URLSession에서 처리해서 data parsing을 진행한다.
>   - 최종적으로 parsing한 데이터를 tableview를 reload함으로써 tableview에 반영한다.



현재 `ProgressHUD` 를 사용하여 **데이터를 가져올 때** 로딩 바를 구현할 것이므로, 이 프로세스를 한번 나누어보자.

- view 로딩되고
- getData() 함수 실행(data parsing)
- 데이터가 불러와지면 tableview를 갱신

여기에서 로딩바는 **데이터를 불러오는 시점 ~ tableview 갱신** 까지 동작하면 된다.



이를 한번 구현해보자.

``` swift
// @ MyViewController.swift
class MyViewController: UIViewController {
  // IBOutlet setting (생략)
  
  // viewDidLoad()
  override func viewDidLoad(){
    super.viewDidLoad()
    
    // view가 로딩되고 데이터를 가져올 것이므로,
    getData()
  }
  
  func getData() {
    // network as n
    // n-i. URL 형변환
    let myURL = URL(string: originalURL)
    
    // n-ii. URL -> request 형태로 형변환
    let requestURL = URLRequest(url: myURL!)
    
    // ProgressHUD as p
    // p-i. loading icon show
    ProgressHUD.animationType = .singleCircleScaleRipple
    ProgressHUD.show()
    
    // n-iii. 접속
    URLSession.shared.dataTask(with: requestURL) { [self] (data, response, error) in
            if error != nil {
            print(error?.localizedDescription)
            // p-iii. 로딩이 실패해도 로딩바는 꺼져야하므로
            ProgressHUD.dismiss()
            
            return
            }
            
            //n-iii-i. data parsing
            self.my = self.parsingJsonData(data: data!)
            
            //n-iii-ii, tableview renew
            OperationQueue.main.addOperation {
            self.tableView.reloadData()
            
            // p-ii. dismiss()
            ProgressHUD.dismiss()
            }
            }.resume()
    
    
    
  }
}
```



