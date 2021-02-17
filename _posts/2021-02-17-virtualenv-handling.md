---
date: 2021-02-17 03:29:42
layout: post
title: "Virtualenv handling"
subtitle: a light virtualenv tutorial
description:
image: /assets/postimg/virtual-env.jpg
optimized_image: /assets/postimg/virtual-env.jpg
category: environment
tags:
    - virtualenv
    - python-env
author:
paginate: false
---



# Virtualenv handling



### 1. 가상 환경 설치

- `pip install`

```zsh
pip install virtualenv
# pip install virtualenv --user // for local user
```





### 2. 가상 환경 구성하기

- 프로젝트 디렉토리를 생성하고 해당 프로젝트 디렉토리로 들어가서 `virtualenv` 를 구성할 수 있다. 
- 일반적으로 `venv_` 를 prefix로 설정하여 구분한다.
- 필요에 따라 `python version`을 설정하고 가상환경을 만들 수 있다.

```zsh
# 디렉토리 생성
mkdir my-project-name

# 디렉토리로 이동
cd my-project-name

# 가상환경 생성
virtualenv venv-name-you-want
```

``` zsh
## python version을 설정하고 가상환경 세팅
virtualenv venv-name-you-want --python=python3.8.2

## global에 설치된 패키지를 상속받아 설치하는 경우
virtualenv venv-name-you-want --system-site-packages
```







### 3. 가상환경 활성화

```zsh
source venv-name-you-want/bin/activate
```

> 특정 패키지를 설치하고 싶은 경우, 가상환경 활성화된 상태에서 설치하면 해당 가상환경에서만 사용이 가능하다.



### 4. 가상환경 비활성화

```zsh
# 가상환경이 활성화되어있는 상태에서
deactivate
```





### cf. 설치한 패키지 및 버전 정보를 저장해두고 설치하기

```zsh
# 설치한 패키지 저장하기
pip freeze > requirements.txt

# 저장한 패키지를 설치하기
pip install -r requirements.txt
```





### cf. 주피터 노트북 커널 등록

`jupyter notebook` 을 사용하는 경우, 내가 설정한 가상환경을 `jupyter notebook kernel` 에 등록해줘야 사용이 가능하다.

``` zsh
# 가상환경 활성화 이후
## 가상환경에 ipykernel 설치
pip install ipykernel

## ipykernel에 가상환경 등록
python -m ipykernel install --user --name YOUR_VENV_NAME
```















