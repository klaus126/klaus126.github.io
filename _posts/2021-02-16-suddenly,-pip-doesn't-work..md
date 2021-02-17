---
date: 2021-02-16 12:50:30
layout: post
title: "Suddenly, pip doesn't work."
subtitle: 당황하지말고, 차근차근
description:
image: https://images.unsplash.com/photo-1429552077091-836152271555?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1832&q=80
optimized_image: https://images.unsplash.com/photo-1429552077091-836152271555?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1832&q=80
category: Environment
tags: 
    - pip
    - environment
    - python-env
author: Klaus
paginate: false
---


# 어느날 갑자기, pip가 안된다.



분명 어제까지 잘 사용했는데, 갑자기 에러가 발생하며 실행이 되지 않는다. 더불어 주피터 노트북도.. 빠르게 고쳐보자.



##### OS spec

```bash
OS: macOS Big Sur version 11.1
```





#### 안될 땐, 구글링

살펴본 해결책은 다음과 같다.



### Round 1. pip 재설치 및 업그레이드

1. pip 재설치

   1. ```bash
      curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
      python3 get-pip.py --force-reinstall
      ```

2. pip 업그레이드

   1. ```bash
      python3 -m pip install --user --upgrade pip
      ```

> 둘다 실패..



### Round 2. pip 삭제 후 재설치

pip 삭제 후, 재설치를 해보았다. 해당 설치는 각 가상환경마다 별도로 실행을 해줘야한다.

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```

> 실패..



### 상황파악

- 침착하게 상황을 생각해보자.

  - 현재 pip 가 존재하는데, path설정이 안되어있는지

  - pip가 아예 날아가버린 것인지

    - ```bash
      which pip
      # /Users/my-home/Library/Python/3.7/bin/pip
      ```

    - 아주 잘 존재한다. 추가적으로 python과 pip의 위치를 확인해보자

      - ```bash
        which python
        # /usr/bin/python
        
        which python3
        # /Library/Frameworks/Python.framework/Version/3.9/bin/python3
        
        which pip
        # /Users/my-home/Library/Python/3.7/bin/pip
        
        which pip3
        # /Users/my-home/Library/Python/3.7/bin/pip3
        
        which jupyter
        # /Library/Frameworks/Python.framework/Version/3.9/bin/jupyter
        ```

    - 해당 위치에 존재는 하지만, path 설정이 변한듯 하다. zshrc / zprofile의 path 설정으로 현재 상태를 살펴본 결과 python, jupyter path 둘다 변해 있음을 확인했다.





- 어디서부터 잘못된 것인지 고찰해보자

  - ```
    history
    ```

  - 어제의 나는 oracle instant client를 설치했고, path를 잡아주었다.

    - ```bash
      sudo mkdir /Applications/Oracle
      cd /Applications
      mkdir ~/lib
      sudo ln -s /Applications/Oracle/instantclient_19_3/libclntsh.dylib ~/lib/
      export LD_LIBRARY_PATH=/Applications/Oracle/instantclient_19_3:$LD_LIBRARY_PATH\nexport PATH=$LD_LIBRARY_PATH:$PATH
      ```

  - oracle instant client 설치는 에러가 발생했고, brew install로 설치하였다.

    - ```bash
      brew tap InstantClientTap/instantclient
      brew install instantclient-basic
      brew install instantclient-sqlplus
      ```

  - 그리고 위 설치가 안되어, -all, --force라는 플래그를 사용했다..

    - ```bash
      softwareupdate --all --install --force
      ```

  - 그럼에도 설치가 안되자, sudo rm -rf를 했다..

    - ```bash
      sudo rm -rf /Library/Developer/CommandLineTools
      sudo xcode-select --install
      ```

  - 그리고 설치가 잘되어서 아주 행복하게 코딩을 했다.

- 범인은 이 안에 있다.






### Round 3. Xcode Commandline Tool에 있는 python/pip 제거하기

  ```
  pip 19.2.3 from /Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.8/lib/python3.8/site-packages/pip (python 3.8)
  ```

  상기 코드를 확인하고, 어제 설치했던 CommandLineTools가 환경 변수에 영향을 끼친게 아닐까라는 가설을 세워본다. 구글링을 해보니 재밌는 포럼글이 있었다.

![screenshot](./site/assets/../../../assets/postimg/Screen%20Shot%202021-02-16%20at%2010.01.19%20PM.png)

  동일한 증상이다. 질문자는 pip를 수동으로 업그레이드(`pip install --upgrade pip`)하다가 문제를 만났고, Xcode 11.2가 설치되기전까지는 잘 사용했다고 한다. Xcode 설치 후 오류를 만났고, 이후 pip를 제거하고, Xcode설치시 따라오는 /usr/bin 하위 디렉토리에 설치된 pip를 사용해서 문제 해결했다고 한다. 

  동일하게 시도해볼까하다, Xcode command line 설치시 path의 우선순위와 path location을 알 수 없고, 환경변수 설정 및 `~/.zprofile`,  `~/.zshrc` 의 설정이 어떻게 반영되는지 확실치않아, 역으로 Xcode command line 설치시 따라오는 python/pip등을 없애고 기존에 있던대로 돌리기로 했다.



우선 `sudo xcode-select --install` 을 통해 설치된 새로운 파이썬을 삭제하자.

```bash
sudo rm -rf /Library/Frameworks/Python.framework
```



그리고 `/usr/local/bin` 에 위치한 다른 여러 파이썬을 삭제하자. (python2가 default라 이를 없애려하는 시도를 종종 보는데, macOS에 사용되는 것이니 지우면 안된다.)

```bash
rm /usr/local/bin/python3*
```



python이 어디에 존재하는지 확인해보자

```bash
which python3
```

>  /usr/bin/python3

해당 파이썬도 삭제하자

```bash
brew uninstall python3
```



이제 기본 내장 파이썬(2.xx)를 제외한 모든 파이썬이 삭제되었다. 새로운 파이썬을 설치하자

```bash
brew install python3
```

```bash
which python3
```

> /usr/local/bin/python3

`/usr/local/bin/python3` 일반적으로 직접 설치하는 파이썬은 이 디렉토리에 설치된다.

이제 pip 업그레이드 및 jupyter를 설치하자

```
python3 -m pip install --upgrade pip
python3 -m pip install jupyter
```



jupyter가 어디에 설치되었는지 확인해보자

```
which jupyter
>> /usr/local/bin/jupyter
```



이제 사용하면 된다.

```
jupyter notebook
```






