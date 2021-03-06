---
layout: post
title: Wi-Fi 데드드랍 만들기
category: DIY
tags:
- raspberry pi
- DIY
- Wi-Fi
- Deaddrop
date: 2018-08-06 00:03:57 +0900
---


방글라데시에서 버스와 관련해 시위가 일어났고 국가는 ISP단에서 인터넷 속도를 제한하고 영상 업로드를 24시간 차단하는 등의 행동을 했다[^Reference].
마침 오늘 난 이럴 때에 유용할 수 있는 도구를 만들었고 이를 공유하고자 한다.

<!--more-->

## 기획

이 프로젝트를 계획한 건 몇 달 전 쯤이다. 하지만 부품 수급의 문제로 인해 오늘 완성하게 되었는데 Wi-Fi 기반의 솔라패널과 배터리로 전력을 공급 받는 Wi-Fi 데드드랍이다.

한국에선 보기 힘들지만 아래와 같은 사진을 어딘가에서 본 적은 있을 것이다.
![USB male port mounted on wall](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8a/Dead_Drops_by_Aram_Bartholl_sthemessage_in_Kunstenlab_%286636685395%29.jpg/1280px-Dead_Drops_by_Aram_Bartholl_sthemessage_in_Kunstenlab_%286636685395%29.jpg)[출처 CC-BY-SA](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8a/Dead_Drops_by_Aram_Bartholl_sthemessage_in_Kunstenlab_%286636685395%29.jpg/1280px-Dead_Drops_by_Aram_Bartholl_sthemessage_in_Kunstenlab_%286636685395%29.jpg)
이게 뭐 하는 물건이냐 하면 저렇게 벽 같은 곳에 USB 메모리를 숨겨 두고 익명으로 몰래 파일을 공유하는 것이다. 스파이들이 쓸 수도 있고 내부고발자들이 쓸 수도 있다.

내가 만든 건 이것의 Wi-Fi 버전이다. 본체도 눈에 띄지 않고, 이동도 가능하면서 누가 사용하고 있는지도 잘 모르게 할 수 있는 장점이 있다. 해외에선 스케이트보드 밑판에 장착한 사진도 있었다.

### PirateBox

이미 해외에서 더 활성화 된 분야이기 때문에 아예 이걸 목적으로 하는 소프트웨어가 있다. [piratebox.cc](piratebox.cc)에서 정보를 얻을 수 있고 소프트웨어나 아예 라즈베리용 이미지를 구워 둔 것을 다운로드 받을 수 있다.


## 전력 공급

USB만 달랑 꽂아 놓으면 되는 USB 데드드랍과 다르게 이건 전력 공급을 항시 해 줘야 하는 문제가 있다. 다른 가이드들을 찾아보면 대부분 AC 어댑터를 꽂아놓는 식인데 이런 식으로 하려면 본인 집에다가 두거나 남의 전기를 몰래 훔쳐 쓰도록 해야 한다. 그래서 난 배터리와 솔라패널을 이용하기로 했다.


### 배터리, 충전/승압 회로

18650 배터리를 사용해서 라즈베리 파이를 7시간 돌렸다는 [블로그 글](http://www.samplerbox.org/article/howtopowerrpiwith18650)이 있었는데 이걸 기초로 만들어 봤다. 결론적으로 말하자면 저 글에서 사용한 모듈은 승압회로가 없어서 리튬이온 배터리의 출력을 그대로 제공하게 되고 3.6~4.2V로는 5V를 요구하는 라즈베리파이를 돌리기 버겁다. 그래서 모듈을 여러가지 사 봤다.

한가지 더 중요한 게 있는데 샤오미 보조배터리로 라즈베리를 돌려 본 사람이라면 알 수도 있겠지만 배터리에 충전기를 꽂는 순간 출력이 잠시 끊기는 Sag이 일어난다. 그렇게 되면 라즈베리는 재부팅을 하게 되는데 별로 좋은 현상은 아니다. 이것까지 고려해서 모듈을 골라야 한다.
이게 왜 중요하냐면 솔라패널은 햇빛을 받을 때만 전력 공급이 되는데 구름이 지나가거나 하는 등으로 잠시 공급이 중단 된다고 배터리에서 라즈베리로 가는 전력까지 끊기는 순간이 존재하면 재부팅을 하게 되고 다운타임과 부팅으로 인한 전력소모로 이어진다.

- 3.7V 배터리를 연결했을 때 5V로 올려주는 승압회로가 존재하나
- 충전기를 연결하거나 해제할 때 출력이 잠시라도 끊기지 않는가

처음으로 샀던 모듈은 [이거][부품 1]다. 앞서 말 했듯이 승압회로가 없어 쓰질 못 한다.

두번째로는 [부품 2][], [부품 3][]를 샀는데 1번 부품은 USB 아웃풋이 있어서 쓰기 좋지만 충전기를 꽂는 순간에 정전이 일어나서 마음껏 충전기를 꽂지 못 한다. 그래서 결국 2번 부품을 쓰게 되었는데 아웃풋 단자엔 USB 케이블을 잘라 납땝을 해 줘야 한다. 나중에 알았지만 2번 부품은 충전기만 꽂고 배터리를 분리했을 때는 아웃풋이 나오질 않는다. 24시간 작동을 원하면서 배터리, 충전기를 교체해도 문제가 없게 하려면 배터리를 두 개 병렬로 연결해서 하나씩 교체하면 된다.

![Charging circuit](https://ae01.alicdn.com/kf/HTB1Swx.XVGWBuNjy0Fbq6z4sXXas.jpg)


### 솔라 패널

예전이랑은 다르게 솔라패널이 저렴해지고 성능도 괜찮아져서 야외나 창가에 있다면 보조배터리 대신 그냥 사용해도 무난할 정도다. 미국의 캠퍼들은 캠핑카 위에 큰 패널을 달아놓기도 한다.
USB 아웃풋이 있는 모델 중에 작으면서도 쓸만한 출력이 나오는 걸 아무거나 골라 쓰면 되는데 나는 [이 모델][solar panel]을 사용했다. 


### 라즈베리 파이

라즈베리 파이는 저렴하면서도 작고 예쁜 싱글보드 컴퓨터다. 특히 제로 버전은 카드의 절반조차 안 되는 작은 크기를 가지고 있어서 zero-W 모델을 구입했다.W가 안 붙은 모델은 와이파이가 없는 버전이니 USB 동글을 사용하거나 해야 한다.

우리는 AC 어댑터로 전력을 공급 받는 게 아니기 때문에 전력 소비를 최소로 낮추는 게 중요한데 0W 모델 기준으로 CPU의 클럭이 700, 1000MHz밖에 존재하질 않는다. 강제로 언더클락(오버클락의 반대)을 해 줘야 하는데 `/boot/config.txt`를 열어 `arm_freq_min=300`과 같이 MHz 단위로 적어 주면 클럭을 더 낮출 수 있다. 그리고 `cpufrequtils` 패키지를 받아 `cpufreq-set -g powersave` 명령을 실행해서 저전력 정책으로 바꿀 수 있다(재부팅 할 때마다 다시 실행해 줘야 한다).


## 완성

![Photo of completed setup]({{ "/imgs/2018-08-06/IMG_20180805_153248.jpg" | absolute_url }})

조잡해 보이긴 하지만 라즈베리 위에 양면테이프를 이용해서 충전 모듈을 달았고 옆에 배터리 소켓도 장착했다.

![Photo of completed setup side with Simple box]({{ "/imgs/2018-08-06/IMG_20180806_150433.jpg" | absolute_url }})

집에 굴러다니던 AC 어댑터 상자에 딱 맞게 들어간다. 상황에 맞게 위장하고 잘 숨기면 된다.


## 활용

난 그냥 순전히 라즈베리 0W 모델을 구입한 후에 "AC 어댑터에 꽂아서 쓰면 진정한 무선이 아니잖아"라는 생각으로 배터리도 달고 충전도 자체수급하도록 만들어 보고 싶어서 만든 프로젝트였다. 하지만 한국의 광주에서 일어났던 것과 비슷한 일이 방글라데시에서 벌어지는 지금, 시대가 달라진만큼 국가적으로 ISP 레벨에서 인터넷 속도를 제한하고 검열을 하고 있다. 검열이야 TOR를 사용해서 우회할 수 있다지만 그것도 인터넷이 되어야 가능한 얘기다. 속도 제한이 걸리거나 최악의 경우 인터넷 자체를 차단한다면 방법은 외부 저장매체에 담아서 물리적으로 이동하는 방법 뿐이다. 와이파이 기반의 데드드랍을 만들어서 해당 지역에 갔다 오는 도중에 사람들이 찍은 사진, 영상을 중거리에서 원격으로 받아 해외 언론에 대신 전달해 주는 식으로 이용 할 수 있을 것이다. 밀고자와 대면으로 접촉하는 경우 정부 관계자들에게 보이고 의심을 사 수색을 당할 수 있기 때문에 그런 방법에 비해 큰 장점이 있을 것이다.


[^Reference]: https://www.dhakatribune.com/bangladesh/2018/08/05/btrc-no-directive-issued-to-suspend-broadband-internet-service
[부품 1]: https://ko.aliexpress.com/item/2PCS-Blue-5V-Micro-USB-1A-18650-Lithium-Battery-Charging-Board/32825966532.html?spm=a2g0s.11045068.rcmd404.1.41b356a4uqQqYY&gps-id=detail404&scm=1007.16891.96945.0&scm_id=1007.16891.96945.0&scm-url=1007.16891.96945.0&pvid=766bd380-f606-4e71-9c33-56b8b76df332
[부품 2]: https://ko.aliexpress.com/item/5-USB-18650-DIY/32843308095.html?spm=a2g0s.9042311.0.0.3da24c4di1xhLE
[부품 3]: https://ko.aliexpress.com/item/18650-3-7-4-2-DC-DC/32846456729.html?spm=a2g0s.9042311.0.0.3da24c4di1xhLE
[solar panel]: https://ko.aliexpress.com/item/LEORY-5W-5-5V-Portable-Folding-Solar-Panel-Charger-Camping-Solar-Power-Bank-For-Cellphone-MP4/32793484103.html?spm=a2g0s.9042311.0.0.3da24c4di1xhLE
