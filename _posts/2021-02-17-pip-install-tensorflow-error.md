---
date: 2021-02-17 02:41:29
layout: post
title: "pip install tensorflow error on macOS Big Sur"
subtitle: macOS Big Sur에서 텐서플로우 설치가 안될 경우
description:
image: /assets/postimg/michael-dziedzic-0W4XLGITrHg-unsplash.jpg
optimized_image: /assets/postimg/michael-dziedzic-0W4XLGITrHg-unsplash.jpg
category: environment
tags: 
    - environment-error
    - tensorflow-error
    - install-error
    - error
author: Klaus
paginate: false
---


# 텐서플로우 설치 오류 (pip install tensorflow)



pip error를 잡고 나서, 텐서플로우를 설치하는데 오류가 발생했다. 오류 메세지는 다음과 같다.

### Error Description

```
ERROR: Could not find a version that satisfies the requirement tensorflow (from versions: none)
ERROR: No matching distribution found for tensorflow
```



현재 필자의 OS 스펙은 다음과 같다. 

### OS spec

```bash
OS Platform & Distribution: macOS, Big Sur, v11.1
python version: 3.9.0
install pakage manager: pip
virtual environment: virtualenv
```



에러를 핸들링 해보기 위해서 다음을 시도해 보았다. 

- 설치 방식 변경(`pip` / ` python -m`)
- 설치 위치 변경(`global` / `local`)



### Trials for handling error

```
# pip로 설치 시도
pip install tensorflow --upgrade

# 버전 설정 후 시도
pip install tensorflow==2.2.0

# python -m 설치 시도
python -m pip install tensorflow
```

> 여전히 에러메시지가 보인다.
>
> - ERROR: Could not find a version that satisfies the requirement tensorflow (from versions: none)
> - ERROR: No matching distribution found for tensorflow



### TensorFlow 공식 사이트에서 설치 메뉴얼 확인

여태껏 `pip install` 으로 충분히 잘 설치가 되었었기에 설치 메뉴얼을 정확하게 본 적이 없었다. 이번 기회에 제대로 숙지해보자.

[tensorflow install guide](https://www.tensorflow.org/install/pip)

##### TensorFlow System requirements

```
- Python 3.5 ~ 3.8
	- Python 3.8 support requires TensorFlow 2.2 or later.
- pip 19.0 or later (requires manylinux2010 support)
- Ubuntu 16.04 or later (64-bit)
- macOS 10.12.6 (Sierra) or later (64-bit) (no GPU support)
- Windows 7 or later (64-bit)
	- Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017 and 2019
- Raspbian 9.0 or later
- GPU support requires a CUDA®-enabled card (Ubuntu and Windows)
```

> macOS를 사용하는 사람이 중점적으로 확인할 것은 하기 요약해두었다.
>
> - python 3.5 ~ 3.8
> - pip 19.0 or later
> - macOS 10.12.6 (Sierra) or later (64-bit) (no GPU support)



### Check point & Solution

1. python 3.5 ~ 3.8 까지 지원하는데, 현재 macOS Big Sur에서 `brew install python` 을 할 경우, 기본적으로 `3.9` 버전이 설치된다.

   > 이 경우, python version을 지원 범위 내로 설정하여 가상환경으로 새롭게 만들어보자.

2. pip version이 낮을 수 있으니 추가로 확인해보자.

   > 이 경우, pip version을 업데이트하고 다시 시도해보자.

3. 혹시 설치시 32 bit의 파이썬을 설치했는지 확인해보자. (홈페이지에서 다운로드 받아 설치하는 경우 간혹 발생한다고 한다.)

   > 이 경우, 파이썬을 삭제하고 다시 설치해보자.



##### solution for #1.

```
# 가상환경 설정
virtualenv --python=python3.8.2 venv-name-you-want
```

> 이후 `pip install tensorflow --upgrade` 설치가 잘 진행된다.

cf. tensorflow 설치 후 numpy 설치시, `numpy==1.19.2` 와 의존성이 있다고 하니 참고하자.



##### solution for #2.

```
# 1. using pip install
pip install --upgrade pip

# 2. using pypa
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```



##### solution for #3

현재 설치된 파이썬을 삭제하고, 새로운 파이썬을 설치하자

```bash
brew install python3
```

```bash
which python3
```

> /usr/local/bin/python3

`/usr/local/bin/python3` 일반적으로 직접 설치하는 파이썬은 이 디렉토리에 설치된다.













