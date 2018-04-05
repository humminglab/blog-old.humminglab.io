---
title: "Embedded IoT SDK 에서 Wi-Fi 지원"
date: "2018-04-03 18:00"
---

Embedded 형태의 IoT의 센서 노드를 개발하기 위한 소프트웨어 플랫폼(SDK)은 다양해져서 잠깐만 인터넷 검색을 해보아도 여러 솔루션을 찾아 볼 수 있다. 특히 센서 노드의 특성 상 이들 SDK는 IEEE 802.15.4(Zigbee, Thread), IEEE 802.15.1(Bluetooth, BLE), IEEE 802.11(Wi-Fi)와 같은 wireless internet connectivity가 주요 기능으로 들어간다.

이 문서에서는 무선 솔루션 중 Wi-Fi를 지원하기 위하여 SDK 선정 시 고려할 사항들을 정리한다.

## IoT 기기의 Wi-Fi 프로토콜 구현 특징

모든 IEEE 802 프로토콜은 [OSI Model](https://en.wikipedia.org/wiki/OSI_model)로 보면 Layer 1 Physical Layer와 Layer2 Data Link Layer를 정의한 표준이다. 이 의미는 상위의 network layer 들은 필요한 요구사항에 맞도록 다양한 프로토콜을 사용할 수 있다는 것이다. 802.15.4를 보면 chip 솔루션 업체도 상위 레이어를 직접 개발이 가능하도록 제공을 하고, 실제로도 [Zigbee](http://www.zigbee.org), [Thread](https://www.threadgroup.org), [WirelessHart](https://en.wikipedia.org/wiki/WirelessHART) 등의 여러 프로토콜이 있다.

802.15.4, 802.15.1 프로토콜은 PAN(Personal Area Network) 또는 센서 기기를 위한 프로토콜로 저속으로 작은 크기의 데이타를 전송하기 위한 용도가 주이다. 이와 대비하여 802.11 Wi-Fi는 근거리(LAN, Local Area Network)에서 802.3 ethernet과 유사한 수준의 무선 네트워크를 위한 프로토콜이다. LAN을 위한 표준 프로토콜인 TCP/IP가 있는 상태에서 다른 네트워크 프로토콜을 사용한다는 것은 넌센스 일 것이다. 이러므로 WiFi device의 driver 제공도 MS Windows, Linux 등의 OS에 TCP/IP 인터페이스에 연결되는 device driver 를 제공한다.

IoT 기기가 Wi-Fi를 지원하였을 때의 가장 큰 장점은 별도로 gateway(예를 들어서 zigbee to ip gatway) 구성 없이도 센서 노드 만으로 기존의 무선 AP에 연결하여 네트워크 구성이 가능하다는 것이다. 이같은 장점으로 인하여 Wi-Fi에 대한 요구는 당분간 꾸준히 있을 것으로 생각된다.

IoT 기기에서 Wi-Fi를 지원하는 방법은 다음과 같이 두 가지를 고려할 수 있다.
- IoT 기기의 RTOS에서 TCP/IP와 Wi-Fi device driver 구현
-

하드웨어, 성능 등이 제한된 임베디드 기기(Constrained Device)는 OS 없이 구동하거나 [FreeRTOS](https://www.freertos.org)와 같은 작은 용량의 OS를 사용하기 때문에 Wi-Fi를 지원하는 것은 솔루션 업체에서 제공해주지 않는 이상 쉽지 않다.

참고로 RTOS에 Wi-Fi를 포팅하는데 가장 큰 걸림돌은 802.11i 의 security 부분이다. 그나마 과거에는 오픈소스로 덩치 큰 [OpenSSL](https://www.openssl.org)을 이용하는 것 밖에는 없었으나 최근에는 Apache 2.0 license인 [Mbed TLS(PolarSSL)](https://tls.mbed.org)가 있어 다른 RTOS로 포팅이 그나마 수월해졌다.




## SDK

* [Cypress Wiced SDK](http://www.cypress.com/products/wiced-software)
* [ARM Mbed OS](https://os.mbed.com)
* [Wind River Zephyr](https://www.zephyrproject.org)