---
layout: post
title: 우분투 17.10과 인텔의 문제 삽질
tags: fix, Ubuntu
category: Ubuntu
author: Kjwon15
date: 2017-11-14 08:50:17 +0900
---


우분투는 12.10인가 이후로 절대로 새 버전이 나오자마자 설치하면 안 되고 한 달을 기다린 후 새로 설치를 해야 하는데 나는 항상 같은 실수를 반복하고 업그레이드 설치를 했다가 망해서 홧김에 백업도 안 하고 그냥 다 날려버렸다. 어차피 최근의 랩탑은 중요한 데이터라고는 하나도 없고 다 백업이 있는 데다가 그나마 중요한 데이터라고 할 수 있는 게 `~/.ssh/id_rsa` 뿐이니 별 문제는 없었다.

문제는 인텔이 그래픽 드라이버도 제대로 안 내 주고 시스템이 잘 안 맞으면 성능도 말도 안 될 정도로 떨어진다는 것이다. 이걸 하나하나 해결하다가 오늘에서야 좀 쓸만해 졌다.
이 글은 모든 사람들에게 통용 되는 게 아니고 그냥 말 그대로 일기장처럼 줄줄이 늘어놓는 글이다. 따라해서 되면 운이 좋은 거고 아니면 참고용으로만 보길.

----

## 1. 인텔 그래픽 드라이버

인텔은 자기들 말로는 오픈소스 드라이버를 제공한다고 하는데 이게 최신 버전의 우분투만 지원하고 버전이 안 맞으면 "지원 안 해요" 하고 내팽게 치는 그런 까다로운 아이다. 문제는 이 최신 버전이라는 게 진짜 최신 버전이 아니라 자기들이 지원 하는 최신 버전이다.
바로 전 버전인 우분투 17.04(당연하겠지만 2017년 4월에 나옴)를 지원하는 드라이버가 [2017년 9월에 나왔다][intel driver]. 6개월마다 새 버전이 나오는 우분투 기준으로 너무나 느린 업데이트다.

방법이 두 가지 있다. 하나는 17.04에서 그래픽 드라이버를 설치 한 채 17.10으로 업그레이드를 하는 것이고 하나는 17.04라고 속이는 것이다. 첫번째 방법은 못 쓰게 되었으니 두번째 방법이다.

`/etc/lsb-release` 파일을 수정하면 된다. `DISTRIB_RELEASE`는 17.04로, `DISTRIB_CODENAME`은 zesty로 바꾸면 된다. ppa를 등록 할 때도 드는 생각이지만 왜 하필 코드네임을 써야 하는 지 모르겠다. 이게 코드네임만 보고 xenial보다 zesty가 최신 버전이겠구나 하는 생각은 할 수 있지만 xenial의 다음 버전이 뭔지는 인터넷에 검색 하기 전에는 절대 모른다. 그냥 17.04라고 적었으면 다음 버전이 17.10인 걸 바로 알텐데 왜 이러는 지는 모르겠지만 아무튼 불편하다.
아무튼 저 파일을 수정하고 나면 `intel-graphics-update-tool`로 그래픽 드라이버가 설치는 된다.

근데 이것도 영 찝찝하다. 인텔 웹사이트에 있는 [링크][intel driver]에 있는 "Packages Updated by the Tool" 항목에 있는 것들을 쭉 복사해다가 업데이트 해 주자.

[intel driver]: https://01.org/linuxgraphics/downloads/intel-graphics-update-tool-linux-os-v2.0.6


## 2. CPU 클럭

예~전엔 CPU governor로 ondemand, conservative 등을 설정 할 수 있었지만 언젠가부터 intel_pstate라는 게 도입이 되면서 커널이 CPU 클럭을 조절하는 게 아니라 인텔 칩에 맞기고 *powersave*, *performance* 중에 고르도록 바뀌었다. 하지만 뭐가 문제인지 CPU 로드가 올라갈수록 최소 클럭으로 바꿔버리는 버그가 있었고 그렇게 시스템 로드가 12가 넘는 경우도 생겼다. 이 상태로는 글자 입력도 안 된다.

이런 건 항상 커널 문제였다. [우분투 커널 PPA 사이트][kernel ppa]에 들어가서 v4.14를 다운 받아 설치 했다. 파일은 5개지만 3개만 설치하면 되는데 lowlatency, generic 중에 하나만 골라 설치하면 된다. 보통은 generic이다.

하지만 결국 pstate를 꺼야 한다. `/etc/default/grub` 파일을 열고 `GRUB_CMDLINE_LINUX`에 `intel_pstate=diable`을 적어 주고 `sudo grub-update` 해서 GRUB를 업데이트 해 주자. 이러면 커널이 해당 기능을 아예 안 쓴다. 웬만하면 쓰는 게 좋겠지만 버그 때문에 컴퓨터를 못 쓰겠다는데 어쩌겠어, 꺼야지.


[kernel ppa]: http://kernel.ubuntu.com/~kernel-ppa/mainline/