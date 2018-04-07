---
title: "스마트홈 업체들 - 2. Smart Lock"
date: "2017-07-28 09:00"
---

이 분야의 회사들은 도어락에 [Bluetooth Smart(BLE)](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy)나 NFC 등 다양한 솔루션을 이용하여 스마트폰, IC 카드 등으로 쉽게 열수 있고, 출입 기록, 시간대별 사용제한, 원격 인터폰 등의 다양한 편리성을 제공해 주는 스마트락을 만들거나 솔루션을 제공하는 곳이다.

# [August Home](http://august.com/){:name='August'}

Welcome to a life that is more simple and secure.
![August Home](/images/2017/7/august.png)

- 특징
  - 다음과 같이 4개의 하드웨어와 앱으로 구성
  - SMART LOCK
    - 스마트폰 등록으로 가까워지면 자동으로 열리고 잠기는 기능
    - Bluetooth, 4개의 AA Battery로 동작
  - CONENCT
    - 전원 콘센트에 꽂는 Bluetooth - to - Wi-Fi bridge
    - SMART LOCK의 인터넷 연결을 중계해주어 원격에서 스마트폰으로 모니터링, 제어 가능
  - DOORBELL CAM
    - 14~24VAC 인터폰 라인에 연결
    - HD급 카메라, 버튼, 근접 센서, 2.4GHz Wi-Fi, Bluetooth 4.0
    - 사람이 접근하면 자동 녹화 등의 기능
    - 영상은 30일간 서버에 저장 (월 $4.99의 별도 사용료 필요)
  - SMART KEYPAD
    - Bluetooth, 4개의 AA Battery 로 동작
    - 비밀번호 입력
    - Apple Siri, Amazon Alexa, Google Assistant 지원
    - 임시 번호, 시간대별 제어 가능
    - 입출입 기록 관리

# [Nuki](http://nuki.io/){:name='Nuki'}

A small upgrade to your door lock
![Nuki](/images/2017/7/nuki.jpg)

- 특징
  - 오스트리아 회사
  - 다음과 같이 3개의 하드웨어와 앱으로 구성
    - SMART LOCK: August SMART LOCK과 유사
    - BRIDGE: [August](#August) CONNECT와 유사한 bridge 기능
    - FOB: 문을 열 수 있는 Bluetooth 리모컨
  - Amazon, IFTTT 연동
  - 기능은 [August](#August) 솔루션과 유사

# [Latch](http://www.latch.com/){:name='Latch'}

Latch is the first smart access system
![Latch](/images/2017/7/latch.png){:height="50%" width="50%"}

- 특징
  - 위 그림에서 왼쪽이 R series, 오른쪽이 L series
  - R series
    - 일반 사무실에 출입문의 통제 시스템 같은 제품. 세콤 출입문 처럼 카드를 대면 자석으로 잠김 문이 열리거나 자동문이 열림
    - Bluetooth, NFC, Touchpad, 카메라 지원 (검은 동그라미에 터치 키패드와 가운데 카메라가 달려 있음)
    - [Wiegand](https://en.wikipedia.org/wiki/Wiegand_interface) interface, Wi-Fi, Ethernet을 이용한 access control 장치와의 연결 가능
  - L series
    - 일반 도어락
    - Bluetooth, NFC, Touchpad, 카메라 지원 (검은 동그라미에 터치 키패드와 가운데 카메라가 달려 있음)
    - 6개의 AA 건전지로 구동
  - 가정 보다는 아파트 등의 enterprise target을 고려

August, Nuki는 가정의 현관을 위한 것이라면 이 제품은 enterprise를 고려하여 사무실, 아파트 등에 적합해 보인다. 또한 느낌상 제품이 기존 도어락 시장 경험이 많은 사람이 만든것 같다.

도어 입출력 기록 등 동기화는 스마튼폰을 이용하여 입출입 하는 동안 자동으로 스마튼폰을 이용하여 동기화 수행. 즉, 도어락은 인터넷 연결이 필요없이 설정 등이 적용됨. R series의 경우 무선랜이나 ethernet을 지원하기 때문에 인터넷 연결 동기화 가능.

# [Unikey Technologies](http://www.unikey.com/){:name='Unikey'}
We're Revolutionizing the Key
![Unikey](/images/2017/7/unikey.jpg)

- 특징
  - 제품을 만드는 회사가 아니라 스마트 도어의 하드웨어 설계 부터, 클라우드 솔루션 까지 턴키 솔루션을 제공하는 플랫폼 회사
    - 이를 이용하여 기존 도어락 회사에서 빠르게 스마트 도어를 만들 수 있도록 지원(예: Kwikset kevo)
  - 다음과 같은 블럭으로 구성
    - UniKey PassiveX (At Door Experience (IP Licensing))
      - Touch-To-Open (Passive Intent): Bluetooth로 스마튼폰과 근접한 상태에서 터치하면 열리는 방식접한 상태에서 터치하면 열리는 방식
      - Inside / Outside and Auto Calibration: 머신러닝을 이용하여 사람이 집 안에 있는지 밖에 있는지 검출
    - UniKey CloudLogic (Cloud-Based Access Control Features)
      - API/SDK를 제공하여 manafacturer에서 쉽게 구현
    - UniKey App Foundations (App Templates, Reference Designs & Development)
      - UniKey SmartWare (Embedded & Hardware)
      - Machine learning도 지원되는 임베디드 디자인, 시험, 개발 지원
    - UniKey Integrations (Ecosystem Integrations)
      - 다른 시스템과 연동
    - UniKey Security Essentials (Access Control and eKey Security)

# [Nexkey](http://www.nexkey.com/){:name='Nexkey'}

Nexkey is rethinking smart locks from the inside out.

아직 제품이 나오지 않아 정확히 알 수 없으나, 스마트폰을 이용한 도어락과 빌딩 관리등에 사용할 수 있는 클라우드 서버 API와 안드로이드, iOS 앱으로 구성 된 것 같다.

# [Glue](http://www.gluehome.com/){:name='Glue'}

Opening doors, unlocking potential
![Glue](/images/2017/7/glue.jpg){:height="50%" width="50%"}

- 특징
  - 스웨덴 회사
  - [August](#August)나 [Nuki](#Nuki)와 유사하게 Smart lock과 Wi-Fi Hub로 구성된 도어락
  - 스마트폰이 근접하면 자동으로 열리는 기능은 없는 것 같음

# [Akerun](http://akerun.com/){:name='Akerun'}

최신의 간편한 입퇴실 관리 시스템
![Akerun](/images/2017/7/akerun.png)

- 특징
  - 일본 회사
  - Bluetooth 기반의 도어락 센서로 다른 회사와 유사
  - NFC Reader도 별도 설치가 가능하고, Akerun Remote를 이용하여 Wi-Fi 로 연계하여 외부에서도 사용가능

# 정리

정리해보면 [August](#August), [Nuki](#Nuki), [Nexkey](#Nexkey), [Glue](#Glue), [Akerun](#Akerun)의 제품은 디테일 한 부분에서는 차이가 있을지 모르나 제품은 거의 비슷해 보인다. 이중 [CB INISIGHTS](https://www.cbinsights.com/company/august)에 나온 것처럼 투자 금액도 가장 높고 투자자도 많은 [August](#August)가 라이업이나 기능 지원 목록이나 가장 좋아 보인다.

[Latch](#Latch)의 경우는 아파트, 빌딩 등 기존 시설에 적용할 수 있는 현실적인 방법(Bluetooth smart를 이용한 동기화)을 만들어 enterprise 시장으로 차별화를 보이는 것 같다. [Unikey](#Unikey)의 경우는 smart lock 플랫폼을 만들어 기존 도어락 업체들이 스마트락 시장에 들어올 수 있는 솔루션을 제공해주는 회사로 타 업체와는 성격이 다르다. 도어락도 화재, 안전성 등 국가별로 다를 수 있는 법적인 사항을 만족하기 위해서는 직접 만드는 것 보다는 이렇게 기존 업체들이 만들 수 있도록 솔루션을 제공하는 것이 합리적으로 보인다. 다만 홈페이지에 나와있는 machine learning 기술이라는 것은 크게 돋보이지 않고, 특허라는 touch-to-go 도 [August](#August) 등 다른 smart lock 업체에서 하는 것과 차이는 없어 보인다. 찾아보아야 겠지만 특허라는 것이 근접해서 터치 한다는 것이 아닐까 생각된다(다른 제품은 근접하면 자동으로 열림).
