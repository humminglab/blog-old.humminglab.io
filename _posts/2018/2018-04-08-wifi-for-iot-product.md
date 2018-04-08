---
title: "Embedded IoT 플랫폼에서 Wi-Fi 지원"
date: "2018-04-08 14:00"
---

Embedded 형태의 IoT의 센서 노드를 개발하기 위한 플랫폼(HW + SW SDK)은 다양해져서 잠깐만 인터넷 검색을 해보아도 여러 솔루션을 찾아 볼 수 있다. 특히 센서 노드의 특성 상 이들 SDK는 IEEE 802.15.4(Zigbee, Thread), IEEE 802.15.1(Bluetooth, BLE), IEEE 802.11(Wi-Fi)와 같은 wireless internet connectivity가 주요 기능으로 들어간다.

이 문서에서는 제품에 Wi-Fi 기능을 넣기 위하여 솔루션 선정 시 고려할 사항들을 정리한다.

## Constrained Devices

IoT 센서 기기와 같은 경우는 배터리로 구동하거나, 낮은 사양 등 일반 PC 나 android 기기에 비하여 저사양 기기라고 할 수 있다. 보통 이를 constrained device 라고 한다(자세한 정의는 [RFC 7228 Terminology for Constrained-Node Networks](https://tools.ietf.org/html/rfc7228) 참고). 여기에서는 이를 '저사양 기기'라고 표기한다. 이들 저사양 기기는 일반 PC, 네트워크 환경에 비교하여 일반적으로 다음과 같은 사항 중 하나 이상에 해당될 수 있다.

- 배터리 구동 등으로 전력 사용에 제한
- 메모리, 시스템 자원의 제한으로 [FreeRTOS](http://freertos.org)와 같은 작은 OS를 사용하거나, 아니면 OS 없이 timer와 interrupt 로 구동
- 제한된 저속 네트워크 사용

PC는 Windows, Linux 등의 범용 OS가 있고, 하드웨어도 PCI, USB, SDIO 등 표준화된 인터페이스와 OS에서 제공하는 해당 인터페이스의 프레임워크가 있어 새로운 기기를 붙이는 것이 어렵지 않으나, 이런 것이 없는 저사양 기기에서는 새로운 디바이스를 추가하는 것이 쉬운 일은 아니다.


## 저사양 기기를 위한 Wi-Fi 지원

모든 [IEEE 802](http://ieeexplore.ieee.org/browse/standards/get-program/page/) 프로토콜은 [OSI Model](https://en.wikipedia.org/wiki/OSI_model)로 보면 Layer 1 Physical Layer와 Layer2 Data Link Layer를 정의한 표준이다. IEEE 802 프로토콜 위에 요구사항에 맞도록 다양한 프로토콜을 사용할 수 있다. 802.15.4를 보면 chip 솔루션 업체에서 상위 레이어를 개발이 가능하도록 driver와 API를 제공 하고, [Zigbee](http://www.zigbee.org), [Thread](https://www.threadgroup.org), [WirelessHart](https://en.wikipedia.org/wiki/WirelessHART) 등 용도에 따라서 여러 프로토콜이 사용되고 있다.

802.15.4, 802.15.1 프로토콜은 PAN(Personal Area Network) 또는 센서 기기를 위한 프로토콜로 저속으로 작은 크기의 데이타를 전송하기 위한 용도가 주이다. 이와 대비하여 802.11 Wi-Fi는 근거리(Local Area Network)에서 802.3 ethernet과 유사한 수준의 무선 네트워크를 위한 프로토콜이다. LAN을 위한 표준 프로토콜인 TCP/IP가 있는 상태에서 다른 네트워크 프로토콜을 사용한다는 것은 넌센스 일 것이다. 이러므로 WiFi device 업체에서도 MS Windows, Linux 등의 OS에 TCP/IP 인터페이스에 연결되는 device driver 를 기본으로 제공한다.

802.15.4(LR-WPAN, Low-Rate Wireless PAN)와 같은 센서 전용 네트워크가 아닌 LAN용 802.11 Wi-Fi를 IoT 기기에서 지원하였을 때의 장점은 별도로 gateway(예를 들어서 zigbee to ip gatway) 구성 없이 기존 무선 AP를 연결하여 네트워크 구성이 가능하다는 것이다. 이같은 장점으로 인하여 Wi-Fi에 대한 요구는 당분간 꾸준히 있을 것으로 생각된다.

하지만 PC, Android가 아닌 작은 OS나 이마저도 사용하지 않는 사양이 제한된 센서 기기에서 Wi-Fi를 사용하는 것은 칩 솔루션 업체가 SDK를 제공하지 않는다면 쉬운 일이 아니다.

이들 기기를 위하여 Wi-Fi 솔루션을 제공하는 방법은 크게 두가지이다.
- Wi-Fi 모듈내에 802.11, TCP/IP, DHCP, HTTP 등 모든 네트워크 프로토콜 제공. 외부의 host와는 UART로 연결되어 AT command를 이용하여 무선랜 이용.

![module](/images/2018/target-wifi.png)

- Wi-Fi 모듈은 802.11 device 역할만 수행하고, host PC와는 SDIO, USB 등으로 연결. Host 컨트롤러에서 Wi-Fi driver 및 TCP/IP 지원.

![module](/images/2018/host-wifi.png)

PC 급에서는 두번째 방식이 일반적 이겠지만, 저사양 기기에서는 첫번째 방식이 기존에 사용하던 host 컨트롤러와 소프트웨어 환경을 크게 바꾸지 않고 사용할 수 있다는 장점으로 많이 사용한다.

다음 절에서는 이 두 방법의 특징 및 장단점을 정리해 본다.

## AT command 형태의 Wi-Fi interface 

UART를 이용한 데이타 통신은 AT 커맨드를 이용하는 것이 업계 표준이라 대부분이 AT 캐맨드 형태로 API를 제공하나 때에 따라서는 JSON 등 다른 데이타 형식을 사용하기도 한다.

이 방식의 가장 큰 장점은 다음과 같다.
- Host 모듈에 TCP/IP stack이나 OS가 없어도 가능
- 센서 기기의 펌웨어 개발자가 TCP/IP, Wi-Fi 등에 대한 세부 개발 지식없이도 개발 가능

기존의 기기에 WiFi 모듈 연결과 host CPU에서 AT 커맨드를 이용한 네트워크 데이타 송수신 구현으로 어렵지 않게 IoT 기기로 변화가 가능하다.

AT 커맨드는 Wi-Fi, TCP/IP, DHCP, Socket, HTTP 등에 대한 넓은 범위의 API를 제공하여야 하기 때문에 보통은 세부적인 컨트롤을 할 수 있는 부분은 제공하지 않고, 가능한 쉽게 사용할 수 있는 명령이나 이벤트를 제공한다. 제공하는 기능을 크게 나누어 보면 다음과 같을 것이다.
- 무선랜 스캔
- Station, AP, Direct 모드 설정 및 연결 관리
- DHCP / IP 설정 
- DNS, PING, TCP, UDP, HTTP 통신

TCP/UDP/HTTP 통신은 socket API가 아니라 간단히 말해서 [cURL](https://en.wikipedia.org/wiki/CURL) utility를 이용하여 HTTP 통신을 하는 것과 비슷하다고 보면 된다.

센서 기기를 개발하는 시스템 프로그래밍 개발자에게는 새로운 것을 배우는 부담을 줄일 수 있어 현실적인 개발 모델이 될 것이다. 하지만 이같은 편리함은 있지만 상대적인 단점이 몇가지 있다.

첫번째 단점은 Wi-Fi 호환성 문제가 발생 시 대응이 쉽지 않다는 것이다. 이 부분은 두번째 모델에도 공통된 문제 일테지만, AT커맨드 방식은 더 많이 감싸져 있어 문제 해결이 더 어렵다. 다음과 같은 사항이 문제가 될 것이다.
- 특정 Wi-Fi AP가 아닌 일반 AP를 사용하는 경우 AP의 버그 등으로 인하여 문제 발생 가능성 있음.
- 이 경우 무선랜 패킷 분석 능력이 없으면 Wi-Fi 모듈 업체에 문제 상황 리포트가 어려움

일반적인 AP의 버그라면 같이 사용하는 PC 에서도 쉽게 발생할 수 있어 그나마 다행이나, 다음과 같은 버그는 쉽게 재현이 되지 않아 센서 기기에 유독 치명적일 수 있다.
- Wi-Fi power save 모드의 버그로 인하여 power save 모드에서 패킷 수신 불가능
- 장시간 연결 시 Wi-Fi association 관리 오류로 AP 송수신 불안정

PC의 경우는 사용자에 의하여 웹 브라우저 등을 사용하게 되면 Wi-Fi power save에서 나온 후 외부로 패킷이 나가는 구조라 보통 위 상황이 문제가 되지 않고, 혹 문제가 되더라고 사용자가 바로 껏다 켜는 등의 조치가 가능하다. 하지만 홀로 동작하는 센서 기기는 외부로 리포트 주기가 긴 상태에서 외부 제어 명령도 수신을 하여야 하는 경우라면 제어 명령을 정상적으로 수신을 못하는 등의 치명적인 문제가 발생할 수 있고, 또한 이에 대한 대처도 쉽지 않다.

참고로 초창기 가정용 WiFi 전화기를 개발하였을 때도 이와 같은 타 AP의 문제로 인하여 수많은 장애 리포트가 있었는데, 이를 크게 줄인 방법 중 하나가 전화기를 충전 크래들에 올려둔 상태에서는 Wi-Fi power save를 나오도록 한 것이었다. 이렇게하여 AP의 버그로 인하여 수신전화를 못받는 경우를 크게 줄일 수 있었다.

두번째 단점은 AT 커맨드가 표준이 아니라는 것이다. BLE(Bluetooth Low Energy)의 경우도 마찬가지로 UART host 인터페이스를 제공하지만, 이는 표준의 HCI(Host Control Interface)로 정의된 사항을 이용한 것으로 일반 동작만이 아니라 production line test 까지도 모두 정의되어 있다(참고: [Bluetooth Core Speicificaiton](https://www.bluetooth.com/specifications/bluetooth-core-specification) Part E. Host Controller Interface Functional Specification).

Wi-Fi 디바이스의 경우 보통 chip vendor는 IEEE 802.11 표준 문서의 10장 Layer Management에 정의된 MLME(MAC Layer Management Entity) interface에 따라서 device driver와의 API를 구현한다. 하지만 AT 커맨드를 제공하는 모듈은 이 API가 아닌 Wi-Fi, socket, protocol을 모두 포함한 포괄적인 자체 AT 커맨드를 제공하여 구현 업체마다 범위가 제각각이라 이를 이용 시 모듈업체의 AT 커맨드 사양에 따라서 기능 제한이 될 수 있다. API 제한으로 인하여 제품의 기능을 구현하는데 문제가 될 수도 있다.

세번째 단점은 대역 병목 현상이 발생할 수 있다는 것이다. 802.11b/g/n의 경우 보통 54~72.2MBps의 최대 전송속도를 가진다. Wi-Fi device는 보통 controller와 4bits 25MHz SDIO (100Mbps)로 연결되어 있어 하드웨어적인 병목 구간이 없다. 하지만 UART를 사용하는 경우에는 보통 115,200bsp(112.5kbps)로 외부 대역에 비하여 UART가 병목 구간이 될 수 있다. 작은 데이타를 송수신하는 센서 노드 에서는 큰 문제가 안되나, OTA(Over-the-air programming) 등 짧은 시간에 큰 대역을 사용하려고 할 때 UART 병목에 따라 TCP handshaking이 컨트롤 되지 않는다면 손실이 되는 등의 문제가 발생할 수도 있다.

간단하게 정리하자면 항상 페어로 연결하는 AP 제품이 정해져 있고, 서버의 명령에 제어되는 것보다는 서버로의 주기적인 리포트가 주요 사항인 기기라면 큰 문제가 없을 것이나, 이 범위 이상이라면 초기 선정 시 업체의 신뢰도, API 제공 범위, 기본 성능 시험 등을 충분히 한 후에 적용을 검토하여야 할 것이다.


## 일반 Wi-Fi 모듈

일반 PC에서 사용하는 것과 같이 SDIO, USB 등의 host interface를 제공하고, MAC layer 이상을 host driver 루틴에서 구현하는 방식이다.

이 부분의 우선 큰 걸림돌은 Wi-Fi device, Host controller, OS를 모두 제공하는 솔루션이 아니라면 실제 적용이 불가능하다고 보아야 한다. Windows나 linux driver를 가지고 사용하는 host controller와 OS에 포팅을 하는 것도 고려 할 수 있겠지만 이 경우 SDIO driver도 구현하여야 하고, 802.11i(WPA) security를 지원하기 위하여 TCP/IP stack 만이 아니라 SSL/TLS library도 포팅하여야 한다. 그나마 최근에는 Apache 2.0 license인 [Mbed TLS(PolarSSL)](https://tls.mbed.org)가 있어 다른 RTOS로 포팅이 그나마 수월해졌다.

센서 기기를 위하여 위와 같은 일을 하기에는 개발 범위가 너무 커지므로 host, wifi 가 모듈로 포함되어 있고, OS, wi-fi 를 지원하는 솔루션을 찾아야 한다.

이런 솔루션을 찾다보면 Wi-Fi chip은 다음과 같은 세가지로 좁혀진다.
- [Espressif](https://www.espressif.com) [ESP8266](https://www.espressif.com/en/products/hardware/socs)
- [Cyperss(구 broadcom)](http://www.cypress.com/) CYW4334x, CYW4343x, CYW43362, CYW4390x (제품에 따라 IEEE 802.11a 5Ghz 있음)
- [Realtek](https://www.amebaiot.com/en/) RTL8195, RTL8710

위의 chip들은 IoT를 타겟으로 하여 마케팅을 하고 있어, 제한된 범위지만 인터넷에서 관련 자료와 오픈된 소스를 구할 수 있다.

사용할 수 있는 솔루션을 정리해보면 다음과 같다.

| 솔루션 | Wi-Fi | OS |
|-----|----|----|
| [Espressif](https://www.espressif.com/en/products/hardware/esp8266ex/overview) | ESP8266 | freertos |
| Cypress [Wiced](http://www.cypress.com/products/wiced-software) | Cypress | threadx / freertos |
| Realtek [Ameba](https://www.amebaiot.com/en/) | Realtek | freertos |
| MXHCIP [MiCO](http://developer.mxchip.com) | Cypress/Realtek | 자체 |
| ARM [MBed](https://www.mbed.com) | RTL8195 | MBed OS |

위의 링크들은 CPU controller + Wi-Fi + OS + TCP/IP + SSL/TLS 등이 모두 포함된 솔루션이다.

이와 같은 솔루션의 장점은 802.11 MAC layer 이상은 모두 소스로 제공되기 때문에 마음만 먹으면 원하는 기능을 구현할 수 있다는 것이다. Wiced 소스를 예를 들면 amazon alexa, apple homekit 등 다양한 샘플코드를 같이 제공한다.

이 방식의 가장 큰 장점은 모듈상의 컨트롤러에서 제공하는 GPIO, PWM, ADC, I2C, SPI 등 여러 외부 인터페이스를 사용할 수 있고, 이 CPU 위에 제품의 기능을 위한 코드를 올릴 수 있어 외부 host controller를 별도로 사용할 필요가 없어 제품의 가격을 낮출 수 있다는 것이다. ESP8266, RTL8195 와 같은 chip은 host controller까지 포함된 one chip 솔루션으로 작은 수량으로 모듈 구매시에도 1~2불대의 가격으로 구매가 가능하다.

이 장점이 단점이 될 수 있는데, 실 동작을 하기 위하여는 최소한 이들 솔루션에 대해서 이해하고 개발할 능력이 되어야 한다는 것이다. 이런 부분에 한 이해를 정확히 못하고 개발을 한다면 AT 방식을 사용하는 것이 비하여 더 많은 문제를 야기할 수도 있다.


## 마치며

개발에 적용시 두가지 방식의 솔루션 중 어느 것을 적용하는 것이 좋을 지는 결정하지 어려울 수 있다.

IoT 모듈 솔루션의 가장 중요한 부분은 security, ota(maintenance), stability 일 것이다. 하지만 위 방식 모두 개발자가 직접 이를 확보하기 위하여 많은 고민을 하여야 하는 상황이다. 그나마 AT 커맨드 방식이 단순하고 정형화 되어 있어 어느 정도 낫다고는 할 수 있다.

향후에는 [Mbed uVisor](https://www.mbed.com/en/technologies/security/uvisor/)와 같은 시스템 보호 매커니즘과 websock API 같은 정형화된 안정적인 네트워크 전송 방법 들이 갖추어진 솔루션이 만들어 진다면 좀더 확실한 솔루션이 되지 않을까 생각해본다.

여기에서는 이 정도로 마무리하고, 다음 기회엥는 두번째 방식인 Wi-Fi driver를 직접 사용하는 방식의 솔루션들에 대해서 좀더 세부적인 사항을 정리해 보기로 한다.