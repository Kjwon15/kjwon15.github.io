---
layout: post
category: mastodon
tags:
- mastodon
- sysop
- spammer
- abusing
title: 마스토돈 스패머와의 싸움
date: 2018-05-25 23:16:55 +0900
---

## 사건의 발단

오늘(2018-05-25) 일어나 보니 핸드폰에 알림이 뭔가 많이 와 있었다. 내가 운영하고 있는 [큐돈](https://qdon.space)이라는 마스토돈 인스턴스 팔로우 알림이었는데 특이한 점이 있어서 곧바로 확인을 하게 됐다.

팔로우를 받은 계정은 [@qdon_support@qdon.space](https://qdon.space/@qdon_support)라는 이 인스턴스의 관리자 계정이었고 마스토돈은 기본적으로 인스턴스에 가입을 할 때 관리자를 모두 팔로우 하도록 설계되어 있다. 그 말은 내 인스턴스에 하룻밤 사이에 10명이 가입을 했다는 얘기인데 아직 소규모 인스턴스기에 어딘가에서 홍보가 잘 됐거나 스팸이거나 둘 중 하나였다. 결과적으로 스팸이었다.


## 급하게 조치

계정명들은 다 일반적으로 아이디로 볼 법한 단어조합이라 사람인가보다 싶었는데 등록한 이메일 계정의 도메인이 수상했다.
topkek.top
shakeme.club
vesigarna.site
kiskus.gdn
kakawa.pw
irewal.asia
grenka.top
boxwerko.pw
barklop.store
bashmac.gdn
juji.gdn

딱 봐도 정상적인 이메일 서비스로는 안 보이고 이 와중에도 새로 가입이 지속적으로 되고 있었다.
일단 취한 조치는 스패머 때문에 잠시 가입을 닫는다는 안내메시지와 함께 신규 가입을 받지 않도록 설정하고 해당 이메일 도메인을 차단했다. 신규 가입을 차단해도 초대를 통한 가입은 되기 때문에 공식 계정에서 DM을 주면 초대장을 주겠다는 안내를 했다.


## 으어어어어

내가 스패머를 과소평가 했었다. 이 놈들은 또 다른 도메인을 이용해서 가입을 했고 이 계정들이 어뷰징을 할 미래가 뻔히 보였지만 아직 아무 것도 한 게 없기에 계정을 차단시키기는 좀 그렇고 어떻게 해야 하나 싶어 일단 "사람이 가입한 계정이 맞나요?"라고 멘션을 보냈는데 마침 그 순간 퍼블릭 타임라인에 스팸이 올라왔다.

스팸을 올린 그 계정을 차단하니 다른 계정에서 올리기에 새로 가입 된 수상한 계정들을 모두 차단했다. 그리고 관리자로서 마스토돈 안에서 할 수 있는 것은 다 했다고 생각 했기에 그 밖으로 나가 행동을 했다.

혹시나 자동화 된 툴이 벌써 있나 싶어서 웹 로그를 뒤져 보았는데 평범한 Firefox UA를 가지고 있었다. 예전의 경험에 의하면 `MFS/1.0`(Morfeus Fucking Scanner) 같은 UA로 툴의 이름이 나오는 경우가 많았는데 아마 무식하게 수동으로 했다거나 selenium 같은 걸 쓴 모양이다. 실제로 브라우저에서 일어날만한 리소스 요청들이 있었다. 그래서 안타깝게도 UA 문자열을 이용한 필터링은 물 건너 갔다.


## 신고

일단 스패머임이 명백해졌기에 약관 위반인 거고 IP를 whois에 조회 해서 어뷰징 신고용 이메일을 찾았고 메일로 "당신의 네트워크에서 스패머가 발견되었습니다. IP는 이러이러하고 조치를 취해주었으면 합니다" 하는 내용을 보냈다.

잠시 후에 메일이 왔고 대략 다음과 같은 내용이었다.

> 귀하의 신고를 잘 받았습니다. 이는 우리의 클라이언트에게 전달 되었고 황색 위험 신호로 마크 되었습니다. 규정에 따라 클라이언트는 문제의 원인을 72시간 내에 제거해야 합니다.
> 이런 일이 생긴 것에 대해 대신 사과 드립니다.
> 더 궁금한 사항이 있으면 언제든 ~~~로 연락 주세요

처음 메일을 열자마자 러시아어가 튀어나와서 당황했지만 다행히도 밑에 영문으로 쓰여 있어서 다행이었다.


## 스패밍이 끝나지 않아

뭐 신고도 들어갔고 이젠 멈추겠지 싶어 가입 차단을 해제했는데 조금 있다가 또 무차별적인 가입이 들어왔다. 스패밍이 돈이 좀 되나보다. 이렇게 도메인을 막 살 수 있다니.. 아무튼 아까 보냈던 어뷰징 리포트 메일에 추가적인 IP 목록을 전송하고 이번엔 다른 조치를 취했다. ISP 말고 도메인쪽으로 신고를 넣었다.
역시나 도메인을 whois에 조회했고 모두 다 같은 사람이 발급 받은 도메인이었다. 어뷰징 신고용 이메일에 마찬가지로 메일을 보냈다.

> 저는 qdon.space의 관리자입니다.
> 당신의 고객 중 한 명이 우리 사이트에 무차별적으로 가입하고 스패밍을 하기 위해 수많은 도메인을 생성한 것을 발견했습니다. 조치를 취해 주세요


## 결국 이렇게까지 해야 하나..

하지만.. 러시아와 다르게 미국은 아직 새벽인 시간이었다. 별로 하고 싶지 않은 일이었지만 IP 차단을 하기로 했다. 스패머가 VPN도 안 쓰고 작업을 하는 듯 싶었다. 뭔 짓을 해서 IP를 바꿨는 지는 모르겠지만 아무튼 전부 다 러시아쪽 IP 주소였고 앞 24비트가 일치하는 주소였다. 애초에 우리 인스턴스는 한국인들을 향한 인스턴스이고 러시아에 거주하는 사람 자체가 없기 때문에 별 문제가 되지 않을 것이라 생각하고 해당 IP 대역을 UFW로 차단했다.


---

P.S
## KISA가 마음에 들지 않아

whois 조회를 하는 과정에 whois라는 커맨드라인 툴도 이용했지만 난 주로 whois.kisa.or.kr을 이용 하는데 이 사이트가 얼마 전에 한글 도메인에 맛을 들였는지 후이즈검색.한국 도메인으로 리다이렉트를 시작했다. 뭐 이거야 별 상관 없는 얘기지만 어뷰징 리포트를 하기 위해서는 이메일 주소를 복사해야 하는데 2018년에 맞지 않게 드래그 방지가 되어 있었다. 도대체 왜?? 그래서 덕분에 이메일 주소를 하나하나 쳐 넣고선 혹시나 틀린 글자가 없나 일일히 확인하는 작업을 거쳤다. 역시 국내 사이트는 믿고 걸렀어야 하는 것이다.