---
template: main.html
---

![Setup-Banner](https://github.com/ExpressLRS/ExpressLRS-Hardware/raw/master/img/quick-start.png)

이 페이지에서는 ExpressLRS를 특정 하드웨어에서 사용할 때 어떤 옵션을 설정해야 하는지 설명합니다.
이 가이드를 통해 여러분이 사용하려는 목적에 맞도록 어떤 옵션을 기본으로 놔 두고, 어떤 옵션을 활성화 해야 하는지 도와드리겠습니다.


어떤 설정들은 TX(송신기)와 RX(수신기) 타겟 양 쪽에서 같이 보이는 경우가 있습니다. 이 때, 이 옵션들은 **반드시** 송신기와 수신기 양 쪽에서 동일하게 설정되어 있어야 서로 바인드가 가능해집니다.
`team2400` (2.4 Ghz) and `team900` (900 Mhz) 간에는 공통된 옵션도 있고, 특정 주파수 대역에만 적용되는 옵션들도 있습니다.
아래의 그림들은 각각 `team2400`과 `team900` 의 일반적인 TX 옵션 설정입니다.

![2400 TX Options](../assets/images/ConfigurationOptions2400tx.jpg)

![900 TX Options](../assets/images/ConfigurationOptions900tx.jpg)

## 전파 규제 지역
```
Regulatory_Domain_AU_915
Regulatory_Domain_EU_868
Regulatory_Domain_FCC_915
Regulatory_Domain_ISM_2400
```
이 옵션을 설정하는 법은 간단합니다 - 단순히, 여러분이 사는 지역의 전파 규제를 선택하면 됩니다. `team2400`의 경우 ISM_2400 옵션을, `team900`의 경우 지역에 따라 `AU`, `EU`, `FCC`를 설정하면 됩니다. 참고로 `EU 868`는 LBT 규제를 **준수하지 않습니다** 👂. 다른 옵션들은 각 전파의 규제를 거의 👿 준수하지만, 여러분의 지역 규제를 완전하게 따르지는 않을 수도 있습니다.

## 바인딩 문구

이 옵션은 간단하지만 **매우 중요합니다**. TX와 RX 양 쪽에 서로 다른 바인딩 문구가 설정될 경우, **ExpressLRS는 동작하지 않습니다**.
만약 다른 사람이 여러분과 같은 바인딩 문구를 설정해 두었다면, 그 사람 역시 여러분의 기체를 조작할 수 있게 됩니다.
바인딩 문구에는 알파벳과 숫자만 사용 가능하며, 이를 섞어서 여러분이 기억하기 쉬우면서도 **겹치지 않을만한** 바인딩 문구를 선택하세요.
만약 RX 모듈이 바인딩 문구 없이 빌드되었다면 전통적인 [바인딩 방법](../binding/) 📜 이 활성화됩니다.

## 성능 옵션들
```
NO_SYNC_ON_ARM
```
이 옵션을 선택한 경우, 기체가 아밍되어있는 동안 sync 패킷을 보내지 않게 됩니다.
이 옵션은 sync 패킷 때문에 발생하는 시간 낭비를 줄여줄 수 있어, 레이싱 기체에서 유용한 옵션입니다. **하지만**, 본격적인 장거리 (Long-range ⛰️) 비행을 하실 경우 이 옵션은 **꺼 두세요**. 만약 failsafe (=노콘) 상황이 발생하면, 아밍 중에는 링크가 다시 연결되지 않게 되어 기체를 조종할 수 없게 됩니다.

ExpressLRS는 "아밍" 감지에 AUX1 채널을 사용하며, **낮은 값은 아밍 해제 상태, 높은 값은 아밍 상태** 로 인식합니다. 만약 조종기에서 아밍 스위치의 방향을 반대 방향으로 설정하고 싶으신 경우, OpenTX에서 전송하는 값을 뒤집어 줄 수 있으므로 이를 활용해 주세요. 만약 ExpressLRS를 처음 사용하는 경우, 불필요한 혼란을 방지하기 위해 이 옵션은 꺼 두시기를 추천합니다.

```
FEATURE_OPENTX_SYNC
```
이 옵션은 OpenTX 조종기와 송신기 모듈 사이의 **지연 시간을 줄여줍니다** 🏃‍♂️. 가급적 항상 활성화 해 두세요.
이 옵션을 사용하려면 [OpenTX 2.3.12 이상](https://www.open-tx.org/2021/06/14/opentx-2.3.12), [EdgeTX](https://github.com/EdgeTX/edgetx), 또는 CRSFShot나 Mixer Sync를 지원하는 조종기 펌웨어가 필요합니다.

```
USE-500HZ
```
이 옵션은 2.4 Ghz 송수신기에서 500Hz 모드를 활성화합니다.
대신, 25Hz 모드는 없어집니다. 
이 옵션을 사용하려면 [OpenTX 2.3.12 이상](https://www.open-tx.org/2021/06/14/opentx-2.3.12), [EdgeTX](https://github.com/EdgeTX/edgetx), 또는 CRSFShot나 Mixer Sync를 지원하는 조종기 펌웨어가 필요합니다. 
**주의: 1.0.0-RC9 버전 이후부터는 이 옵션은 강제로 적용되며 선택할 수 없습니다. 25Hz 모드는 제거되었습니다.**

## 추가 데이터들

```
HYBRID_SWITCHES_8
```
이 옵션은 AUX 채널 값들이 전송되는 방식을 바꿉니다.
이 옵션을 해제하면 Normal Mode로 작동하고, 8 개의 2-pos 스위치(on/off로만 사용 가능) 값을 저지연(low-latency)으로 전송합니다.
`HYBRID_SWITCHES_8` 옵션이 선택되면 Hybrid mode로 작동하며, 2-pos 스위치 1 개(AUX1), 7-pos 스위치 6 개(AUX2-7), 16-pos 스위치 1 개(AUX8)를 사용할 수 있으나, 2-pos 스위치 한 개만 낮은 지연시간을 보장합니다. Normal mode에서는 모든 패킷에 AUX 채널의 값이 한꺼번에 전송됩니다.
그러나 Hybrid mode에서는 AUX1 채널의 값만 모든 패킷에 전송되며, 다른 AUX채널의 값들은 여러 패킷에 나뉘어 순차적으로 전송됩니다.

**주의:** 이 스위치 모드 옵션은 **TX** 와 **RX** 양 쪽에 **반드시 동일하게** 설정하여야 합니다. 두 모드에 대해 좀 더 자세한 설명은 <a href="/software/switch-config/">Switch Modes</a> 페이지를 참고하세요.

```
ENABLE_TELEMETRY
```
이 옵션은 고급 텔레메트리 정보를 활성화합니다. 이 기능은 **TX** 및 **RX** 양 쪽에 동일하게 활성화 되어 있어야 사용할 수 있습니다.
이 옵션을 활성화하면 다음과 같은 텔레메트리 정보를 추가로 사용할 수 있습니다.

* GPS
* BATTERY_SENSOR
* ATTITUDE
* DEVICE_INFO
* FLIGHT_MODE
* MSP_RESP

**주의 1**: "sensor lost" 경고가 발생할 경우, ExpressLRS Lua 스크립트를 이용해서 텔레메트리 속도(telemtry rate)를 높여주세요. 200 Hz 모드를 쓰는 경우 1:16 정도를 선택하면 됩니다.

**주의 2**: 이 옵션은 **반드시 HYBRID_SWITCHES_8** 옵션과 함께 활성화 되어야 합니다.

이 옵션을 꺼 둘 경우에도 1RRS (RSSI dbm), RQLY (LQ) 등의 기본 텔레메트리 정보는 사용할 수 있습니다.

*팁: 이 옵션을 켠 상태로 펌웨어를 빌드하고, 텔레메트리가 필요 없는 경우(예: 레이싱) Lua 스크립트상에서 TLM Ratio를 OFF로 설정하면 이 옵션을 선택하지 않은 것과 동일한 효과를 낼 수 있습니다. 텔레메트리가 다시 필요해졌을 때(프리스타일이나 장거리 비행 등) 원하는 속도(1:16이나 1:8 등)로 다시 활성화 할 수 있습니다.*


```
TLM_REPORT_INTERVAL_MS
```
이 옵션을 설정하면 여기에 설정한 간격(ms)마다 송신기 모듈에서 OpenTX로 텔레메트리 정보를 보냅니다.
이를 통해 Telemetry rate를아주 높게 설정하거나 느린 링크 속도 (예: 50 Hz)를 사용할 경우 "Telemetry lost" 경고를 방지할 수 있습니다.

기본값은 **320LU** 입니다. 원하는 값을 밀리초(ms) 단위로 설정한 뒤, 숫자 뒤에 **LU** 를 붙여주세요. 예를 들어 100 ms 간격으로 텔레메트리 정보를 전송할 경우 **100LU** 라고 입력하면 됩니다.

일반적으로, OpenTX 조종기에서는 **320LU** 값을 그대로 사용하고, ErskyTx 에서는 **100LU** 값을 쓰면 됩니다.

*팁: 기본으로 설정된 320LU 외의 값을 사용할 때만 이 옵션을 활성화 하면 됩니다.*

## 기타 옵션들

```
JUST_BEEP_ONCE
DISABLE_STARTUP_BEEP
MY_STARTUP_MELODY="<music string>|<bpm>|<semitone offset>"
```
R9M 등의 부저가 달린 송신 모듈을 사용할 때, 부저가 한 번만 울리게 하거나, 아예 울리지 않게 하거나, 또는 커스텀 시작음을 내게 합니다.
기본값은 ExpressLRS 시작음 🎼 입니다. 이를 바꾸고 싶으면 이들 중 하나의 옵션을 선택하면 됩니다.

시작음을 고치고 싶으면, `MY_STARTUP_MELODY` 옵션을 사용하세요. BlHeli32 또는 RTTL 문법을 사용합니다. BlHeli32 문법에서는 `music string`와 `bpm`를 필수로 사용해야 하고, 선택적으로 `semitone offset` 을 이용해서 전체 음을 올리거나 내릴 수 있습니다. BlHeli32 멜로디의 예시들은 [Rox Wolfs youtube channel](https://www.youtube.com/playlist?list=PL_O0XT_1mZinetucKyuBUvkju8P7DEg-v) 채널에서 찾아보실 수 있습다만, 적용하는 데는 추가로 실험을 좀 해 뵈야 할 수도 있습니다. :musical_note: 
직접 시작음을 편집하려면 **[Sheet Music 101](https://github.com/nseidle/AxelF_DoorBell/wiki/How-to-convert-sheet-music-into-an-Arduino-Sketch)** 또는 **[BLHeli Piano)](https://dra6n.github.io/blhelikeyboard.github.io/)** 등을 사용해 보세요.


RTTL 문법은 예전에 핸드폰 벨소리를 만들 때 쓰던 것이고, [여기](http://esctunes.com/)에서 많은 예시들을 찾아볼 수 있습니다. 


```
UNLOCK_HIGHER_POWER 
```
대부분의 ExpressLRS 모듈은 250mW가 최고 출력입니다만, 이 옵션을 선택하면 지원하는 모듈에 한해 더 높은 출력을 사용할 수 있습니다.
그러나 [냉각팬 개조](https://github.com/AlessandroAU/ExpressLRS/wiki/R9M-Fan-Mod-Cover)를 사용하여 모듈 내부에서 충분한 냉각을 해 주어야 합니다. 
이는 특히 R9M에 필요한데, 기본 펌웨어에서부터 1W 출력을 지원하거니와, ExpressLRS을 올리면 더 높은 듀티 사이클로 돌아가기 때문에 열이 훨씬 빠르게 오릅니다.
안정적인 동작을 위해서 냉각은 필수이며, 냉각 없이는 모듈이 불타는 벽돌로 변해버릴 위험이 있습니다.


```
UART_INVERTED
```
이 옵션은 **ESP 기반의 모듈에서만** 동작합니다. 내장 신호 반전기가 없는 모듈에서는 동작하지 않습니다.
이 옵션은 반전된 CRSF 신호를 요구하는 FrSky QX7, TBS Tango 2, RadioMaster TX16S 등의 조종기를 사용할 때 필요합니다.
대부분의 경우 이 옵션을 켜 두고, 만약 T8SG V2 또는 기타 변형된 펌웨어를 사용하는 경우에만 이 설정을 끄세요.

## 수신기(RX) 한정 옵션 ##

![2400 RX Options](../assets/images/ConfigurationOptions2400rx.jpg)

![900 RX Options](../assets/images/ConfigurationOptions900rx.jpg)

*주의: 사용하고자 하는 모든 수신기의 옵션 설정은 송신기의 옵션 설정과 반드시 일치해야 합니다. 그렇지 않으면 바인딩에 실패할 수 있습니다.*

송신기 옵션들의 설명은 수신기 옵션에도 그대로 적용됩니다.

그러나 다음과 같이, 수신기 전용 설정들이 몇 가지 있습니다.

```
LOCK_ON_FIRST_CONNECTION
```
이 옵션은 RF Mode를 고정합니다. 

수신기가 처음 켜지면 사용 가능한 모든 송수신 속도(RF Mode)를 번갈아가며 연결을 기다립니다. 이 옵션을 사용하지 않을 경우, ExpressLRS는 연결이 끊기면 (failsafe, 노콘) 이 모드로 돌아갑니다. 

이 옵션이 선택된 경우 ExpressLRS는 처음 연결된 송수신 속도로 고정되며, 연결이 끊긴 뒤에도 해당 속도의 신호만을 기다리게 됩니다. 
이 옵션은 failsafe 이후 재연결을 빠르게 하지만, 항상 고정된 속도로만 통신해야 합니다.
일반적으로는 이러한 동작이 바람직 하지만, 특수한 경우, 예를 들어 먼 거리 또는 전파가 불량한 지역에서 통신이 끊긴 기체와 연결하려고 할 때, 이미 통신 속도가 고정되어버렸기 때문에 송신 속도를 낮추어 더 높은 감도로 통신할 수 없게 됩니다.

이 옵션이 설정된 경우에도 Lua 스크립트에서 값을 바꾸거나, 수신기를 재부팅하면 송수신 속도를 바꿀 수 있습니다.

*참고:*
수신기가 여러 송신 속도를 테스트 할 때에는 가장 빠른 속도에서 시작해서 점점 느린 속도로 통신을 시도하고, 이를 계속 반복합니다. 한 속도에서는 `PACKET_INTERVAL * PACKS_PER_HOP * HOP_COUNT * 1.1` 만큼의 시간을 대기합니다 (예: 250Hz의 경우 4ms * 4 * 80 * 1.1 = 1.408s). 만약 이 기간 동안 정상 패킷이 하나라도 수신되면 대기 시간은 10 배가 됩니다. 


```
AUTO_WIFI_ON_INTERVAL
```
⚠️ WiFi를 통해 수신기 펌웨어를 업데이트 하려는 경우, 이 옵션을 반드시 설정해야 합니다 ⚠️

ESP8285를 사용한 모듈의 경우, 송신기와 연결되지 않은 상태에서 이 옵션에서 설정한 시간(초)이 지나면 자동으로 WiFi 📶 를 켭니다. HappyModel 기본 펌웨어는 이 시간이 20초로 설정되어 있으나, RC8 이후로 30초를 기본값으로 사용합니다. 수신기에서 WiFi가 켜 지면 "ExpressLRS RX" AP에 연결하고 (비밀번호: expresslrs), http://10.0.0.1 으로 접속하여 펌웨어를 업데이트 할 수 있습니다.

```
USE_DIVERSITY
```
하드웨어가 Diversity(두 개 이상의 안테나 사용)를 지원하는 경우 이 옵션을 활성화 합니다.


```
USE_R9MM_R9MINI_SBUS
```
**주의: 이 옵션은 SBUS 프로토콜을 의미하지 않습니다**

이 옵션은 단순하게 R9MM과 R9 Mini 모듈에서 측면에 위치한 두 개의 핀들(A9, A10) 대신, "SBUS" 표식이 달려 있는 핀을 사용하도록 합니다. 이는 UART RX에 고정된 하드웨어 인버터가 달려 있는 F4 FC를 사용할때 특히 유용합니다. 다만, 이 경우 단방향 통신만 가능하기 때문에 텔레메트리 다운링크와 passthrough 펌웨어 업로드 등을 사용할 수 없습니다.
이 옵션을 사용하여 S.BUS 🚌 핀에 리시버를 연결한 경우에도 베타플라이트🐝에서는 CRSF 프로토콜을 선택해야 하며, CLI에서 `set serialrx_inverted = ON`을 설정할 필요가 있을지도 모릅니다.

*모든 설정에 대한 추가 설명이 필요하면 [User Defines 페이지](/software/user-defines.md) 를 참고하세요.*

**끝! 이제 송신기에 펌웨어를 올려봅시다!**