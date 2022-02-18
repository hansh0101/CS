<aside>
💡 BLE 통신을 알아보자.

</aside>

### BLE란?

스마트폰이 출시되어 대중화가 될 무렵, ‘스마트’한 개념의 밴드, 워치, 글래스 등이 출시되면서 웨어러블 디바이스 시장이 태동하기 시작했다. 그리고 2015년 상반기, 애플워치의 등장으로 작은 생태계를 이루고 있던 웨어러블 디바이스들이 다시 한 번 각광을 받게 되었다. 각기 생긴 모습은 다르지만 이들의 공통점은 스마트폰과 연동되어 작동한다는 것이다. 과거부터 기기들 간의 단거리 무선 통신은 `Bluetooth` 라는 기술이 이용되었다. Bluetooth가 공식적으로 등장한지는 오래 되었지만, 여전히 기기 간의 무선통신에는 Bluetooth가 사용된다. 하지만, 지금 사용되는 Bluetooth는 기존과는 다른 방식이다. 바로 **`BLE`** 라는 특징을 가진 Bluetooth인데, 바로 이것이 오늘날 다양한 종류의 웨어러블 디바이스들이 태어날 수 있었던 원동력이 되었다. 그렇다면 BLE라는 것은 도대체 무엇일까?

과거부터 기기들 간의 무선 연결은 주로 Bluetooth라는 기술을 이용했는데, 이들은 기기 간에 마스터, 슬레이브 관계를 형성하여 통신하는 `Bluetooth Classic`이라는 방식을 이용했다. 사람들이 이러한 기기들을 이용하면서 많이 염려했던 것은 ‘Bluetooth를 연결하면 배터리가 빨리 소모된다’, ‘사용하지 않을 때는 Bluetooth를 꺼야 한다’ 등과 같은 배터리 관련 문제들이었다. 실제로도 Bluetooth Classic은 다른 디바이스를 무선으로 연결을 하여 사용할 수 있는 편리함을 주었지만, 연결이 되는 동안에는 배터리를 빠르게 소모시켰기 때문에 사용하는 데에 많은 불편함이 있었다.

2010년, 새로운 Bluetooth 표준으로  **`Bluetooth 4.0`** 이 채택된다. 기존의 Bluetooth Classic과 가장 큰 차이는 훨씬 적은 전력을 사용해 Classic과 비슷한 수준의 무선 통신을 할 수 있다는 점이었다. 이는 당시 Bluetooth의 최대 단점이었던 과도한 배터리 소모 문제를 해결하는 기술이었기 때문에, Bluetooth 관련 업계에 큰 반향을 일으켰다. 이렇게 저전력을 이용하여 무선통신을 하는 특징을 **`Bluetooth Low Energy (이하 BLE)`** 라고 부르는데, Bluetooth 4.0 이후의 버전들은 대체로 이 용어로 대체되어 불리기도 한다. 최근 출시되고 있는 스마트 밴드, 워치, 글래스 등의 웨어러블 무선통신 기기들은 대부분 이 BLE 방식을 이용하여 무선 통신을 한다.

### Bluetooth Smart Ready, Smart, Classic

BLE 기술이 등장하면서 Bluetooth 디바이스들은 아래와 같이 3가지로 분류되었다.

![Untitled](BLE%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB%20d7cc4/Untitled.png)

Bluetooth 4.0과 함께 새롭게 등장한 **Bluetooth Smart Ready, Bluetooth Smart**에 대해서 살펴보면,

**Bluetooth Smart Ready** 디바이스는 Bluetooth Classic 및 저에너지 Bluetooth 무선통신(BLE)을 지원하기 때문에 ‘듀얼 모드’ 라디오라고 불린다. 따라서, 이들은 현재 시장에 나와 있는 수많은 종의 Bluetooth 디바이스들에 대한 역방향 호환성을 가진다. 종류에는 스마트폰, 태블릿, PC, TV 그리고 셋탑박스 및 게임 콘솔 등이 있다. 이런 디바이스들은 Bluetooth Classic 디바이스 및 Bluetooth Smart 디바이스들로부터 데이터를 받아, 이들을 유용한 정보로 변환시키는 Bluetooth 시스템의 허브라고 할 수 있다.

**Bluetooth Smart** 디바이스 내에 있는 라디오는 ‘싱글 모드’ 라디오라고 불리는데, BLE 연결만을 지원한다는 의미이다. 이들은 기존의 Bluetooth Classic 디바이스들과 호환이 되지 않고 듀얼 모드 라디오를 가진 Bluetooth Smart Ready 디바이스 혹은 제조업체에 의해 호환성이 명시된 특정 디바이스에만 연결이 가능하다. Bluetooth Smart 디바이스들은 ‘우리 집의 창문은 모두 잠겨있는지’, ‘내 인슐린 농도는 얼마인지’, ‘오늘 내 몸무게는 몇 킬로그램인지’ 등과 같이 특정한 형태의 정보를 수집해, Bluetooth Smart Ready 디바이스로 보내기 위해 만들어진 디바이스이다. 종류에는 심박 모니터, 스마트 손목시계, 창문 및 현관 보안 센서, 자동차 키 체인, 그리고 혈압 팔찌 등이 있다.

이 글에서는 BLE를 사용하는 디바이스들이 **어떤 과정으로 서로 연결되어 통신을 하는지** 에 대해 내용을 정리해보고자 한다.

### 어떤 원리로 통신할까?

BLE를 지원하는 디바이스들은 기본적으로 **Advertise(Broadcast)**와 **Connection** 이라는 방법으로 외부와 통신한다.

**Advertise Mode(=Broadcast Mode)**

특정 디바이스를 지정하지 않고 주변의 모든 디바이스에게 Signal을 보낸다. 다시 말해, 주변에 디바이스가 있건 없건, 다른 디바이스가 Signal을 듣는 상태이건 아니건, 자신의 Signal을 일방적으로 보내는 것이라고 생각하면 된다. 이 때, Advertising type의 Signal을 일정 주기로 보내게 된다.

Advertise 관점에서, 디바이스의 역할은 다음과 같이 구분된다.

- **Advertiser(=Broadcaster)**: Non-Connectable Advertising Packet을 주기적으로 보내는 디바이스
- **Observer**: Advertiser가 Advertise하기 위해 보내는 Non-Connectable Advertising Packet을 듣기 위해 주기적으로 Scanning하는 디바이스

![Untitled](BLE%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB%20d7cc4/Untitled%201.png)

Advertise 방식은 한 번에 한 개 이상의 디바이스와 통신할 수 있는 유일한 방법이다. 주로 디바이스가 자신의 존재를 알리거나 적은 양(31Bytes 이하)의 User 데이터를 보낼 때도 사용된다. 한 번에 보내야 하는 데이터 크기가 작다면, 굳이 오버헤드가 큰 Connection 과정을 거쳐서 데이터를 보내기보다는, Advertise를 이용하는 것이 더 효율적이기 때문이다. 게다가 전송할 수 있는 데이터 크기 제한을 보완하기 위해 Scan Request, Scan Response를 이용해서 추가적인 데이터를 주고받을 수 있다. Advertise 방식은 말 그대로 Signal을 일방적으로 뿌리는 것이기 때문에, 보안에 취약하다.

**Connection Mode**

양방향으로 데이터를 주고받거나, Advertising Packet으로만 전달하기에는 많은 양의 데이터를 주고받아야 하는 경우에는, Connection Mode로 통신을 한다. Advertise처럼 ‘일대다’ 방식이 아닌, ‘일대일’ 방식으로 디바이스 간에 데이터 교환이 일어난다. 디바이스 간에 Channel hopping 규칙을 정해놓고 통신하기 때문에 Advertise보다 안전하다.

Connection 관점에서 디바이스들의 역할은 다음과 같이 구분된다.

- **Central(Master)**: Central 디바이스는 다른 디바이스와 Connection을 맺기 위해, Connectable Advertising Signal을 주기적으로 스캔하다가, 적절한 디바이스에 연결을 요청한다. 연결이 되고 나면, Central 디바이스는 timing을 설정하고 주기적인 데이터 교환을 주도한다. 여기서 timing이란, 두 디바이스가 매번 같은 Channel에서 데이터를 주고받기 위해 정하는 hopping 규칙이라고 생각하면 된다.
- **Peripheral(Slave)**: Peripheral 디바이스는 다른 디바이스와 Connection을 맺기 위해, Connectable Advertising Signal을 주기적으로 보낸다. 이를 수신한 Central 디바이스가 Connection Request를 보내면, 이를 수락하여 Connection을 맺는다. Connection을 맺고나면 Central 디바이스가 지정한 timing에 맞추어 Channel을 같이 hopping을 하면서 주기적으로 데이터를 교환한다.

![Untitled](BLE%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB%20d7cc4/Untitled%202.png)

### Protocol Stack

디바이스들은 Bluetooth로 통신을 하기위한 Protocol Stack을 가지고 있다. 일반적으로 네트워크 통신을 하기 위해서는, 통신을 위한 규약인 Protocol을 정의해야 되는데, 이렇게 정의된 Protocol들을 층층이 쌓아놓은 그룹이 Protocol Stack이다. Bluetooth Signal Packet을 수신하거나 송신할 때, 이 Protocol Stack을 거치면서 Packet들이 분석되거나 생성된다.

![Untitled](BLE%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB%20d7cc4/Untitled%203.png)

위 그림에서 볼 수 있듯이, Protocol Stack은 가장 아랫단부터 크게 **Controller, Host, Application**으로 나뉜다. 여기서는 Connection 과정에 필요한 부분인 **Physical Layer, Link Layer, Generic Access Profile(GAP), Generic Attribute Profile(GATT)**에 대해서 알아본다.

**Physical Layer**

Physical Layer에는 실제 Bluetooth Analog Signal과 통신할 수 있는 회로가 구성되어 있어서, Analog 신호를 Digital 신호로 바꾸어 주거나 Digital 신호를 Analog 신호로 바꾼다. 또한 Bluetooth에서는 2.4 GHz 밴드를 총 40개의 Channel로 나누어 통신을 한다. 40개 Channel 중 3개 Channel은 **Advertising Channel**로써 각종 Advertising Packet을 비롯하여 Connection을 맺기 위해 주고 받는 Packet들의 교환에 이용된다. 나머지 37개의 Channel은 **Data Channel**로써 Connection 이후의 Data Packet 교환에 이용된다.

![Untitled](BLE%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB%20d7cc4/Untitled%204.png)

**Link Layer**

Physical Layer의 바로 윗단에는 Link Layer이 있다. Link Layer은 하드웨어와 소프트웨어의 조합으로 구성되어 있다. 하드웨어 단에서는 높은 컴퓨팅 능력이 요구되는 작업들 (Preamble, Access Address, and Air Protocol framing, CRC generation and verification, Data whitening, Random number generation, AES encryption 등)이 처리되고, 소프트웨어 단에서는 디바이스의 연결 상태를 관리한다. 또한 통신하는데 있어서 디바이스의 Role을 정의하고 이에 따라 변경되는 State를 가지고 있다.

**Role**

- **Master**: 연결을 시도하고, 연결 후에 전체 connection을 관리하는 역할
- **Slave**: Master의 연결 요청을 받고, Master의 timing 규약을 따르는 역할
- **Advertiser**: Advertising Packet을 보내는 역할.
- **Scanner**: Advertising Packet을 Scanning하는 역할. Scanner는 아래와 같은 2가지 Scanning 모드가 있다.
    - **Passive Scanning**: Scanner는 Advertising Packet을 받고 이에 대해 따로 응답을 보내지 않는다. 따라서 해당 Packet을 보낸 Advertiser는 Scanner가 Packet을 수신했는지에 대해서 알지 못한다.
    - **Active Scanning**: Advertising Packet을 받은 Scanner는 Advertiser에게 추가적인 데이터를 요구하기 위해 **Scan Request**라는 것을 보낸다. 이를 받은 Advertiser는 **Scan Response**로 응답한다.
        - **Scan Request, Scan Response** : Advertising Packet type의 한 종류이다. 앞서, 31bytes 이하의 User data에 대해서는 Advertising Signal Packet에 넣어서 보낼 수 있다고 하였다. 하지만 31bytes보다는 크지만, Commection까지 맺어서 보내기는 오버헤드가 큰 데이터가 있을 때, Scan Request, Scan Response를 이용하면 두 번에 걸쳐서 데이터를 나눠 보낼 수 있게 된다. Advertising Packet을 받은 Scanner는 추가적인 User Data(예를 들어, Peripheral 디바이스의 이름)를 얻기 위해 Scan Request를 보내게 된다. Scan Request를 받은 Advertiser는 나머지 데이터를 Scan Response Signal에 담아서 보낸다.

**State**

Link Layer는 5가지 State를 가지고 있는데, 각 디바이스는 서로 연결이 되는 과정에서 이 State를 변화시킨다. 다음과 같은 5개의 State가 존재한다.

- **Standby State**: Signal Packet을 보내지도, 받지도 않는 상태.
- **Advertising State**: Advertising Packet을 보내고, 해당 Advertising Packet에 대한 상대 디바이스의 Response를 받을 수 있고 이에 응답할 수 있는 상태.
- **Scanning State**: Advertising Channel에서 Scaning하고 있는 상태.
- **Initiating State**: Advertiser의 Connectable Advertising Packet을 받고난 후 Connetion Request를 보내는 상태.
- **Connection State**: Connection 이후의 상태.

아래 그림은 각각의 State를 Diagram으로 나타낸 것이다.

![Untitled](BLE%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB%20d7cc4/Untitled%205.png)

**Generic Access Profile(GAP)**

Generic Access Profile (GAP)는 서로 다른 제조사가 만든 BLE 디바이스들끼리 서로 호환되어 통신할 수 있도록 해주는 주춧돌 역할을 한다. 즉, 어떻게 디바이스간에 서로를 인지하고, Data를 Advertising하고, Connection을 맺을지에 대한 프레임워크를 제공한다. 그래서 GAP는 최상위 Control Layer라고도 불린다. Advertising Mode일 때, GAP에서 Advertising Data Payload와 Scan Response Payload를 포함할 수 있다.

또한 GAP에서는 BLE 통신을 위해 Role, Mode, Procedure, Security, Additional GAP Data Format 등을 정의한다. 이들은 실제 API와 직접적으로 많은 연관이 있기 때문에 그 내용이 상당히 많지만, 여기서는 BLE Connection과 관련이 있는 Role에 대해서만 알아보겠다.

**Role**

- **Broadcaster** : Link Layer에서 Advertiser 역할에 상응한다. 주기적으로 Advertising Packet을 보낸다. 예를 들면, 온도센서는 온도데이터를 자신과 연결된 디바이스에게 일정주기로 보낸다.
- **Observer** : Link Layer에서 Scanner 역할에 상응한다. Broadcaster가 뿌리는 Advertising Packet에서 data를 얻는다. 온도센서로부터 온도데이터를 받아서 디스플레이에 나타내는 테블릿 컴퓨터의 역할이다.
- **Central** : Link Layer에서 Master 역할과 상응한다. Central 역할은 다른 디바이스의 Advertising Packet을 듣고 Connection을 시작할 때 시작된다. 좋은 성능의 CPU를 가지고 있는 스마트폰이나, 테블릿 컴퓨터들의 역할이다.
- **Peripheral** : Link Layer에서 Slave 역할과 상응한다. Advertising Packet을 보내서 Central 역할의 디바이스가 Connection을 시작할 수 있도록 하게끔 유도한다. 센서기능이 달린 디바이스들의 역할이다.

**Generic Attribute Profile(GATT)**

BLE Data 교환을 관리하는 GATT는 디바이스들이 Data를 발견하고, 읽고, 쓰는 것을 가능하게 하는 기초적인 Data Model과 Procedure를 정의한다. 그래서 GATT는 최상위 Data Layer라고도 불린다. 디바이스간에 low-level에서의 모든 인터렉션을 정의하는 GAP와는 달리, GATT는 오직 Data의 Format 및 전달에 대해서만 처리한다. Connection Mode일 때, GATT Service와 Characteristic을 이용하여 양방향 통신을 하게 된다. Service와 Characteristic에 대한 내용은 [여기](https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gatt)를 참고하길 바란다.

GATT도 Data 처리와 관련해서 다음과 같은 역할을 정의한다.

**Role**

- **Client**: Server에 Data를 요청한다. 하지만 처음에는 Server에 대해서 아는 것이 없기 때문에, Service Discovery라는 것을 수행한다. 이 후, Server에서 전송된 Response, Indication, Notification을 수신할 수 있다.
- **Server**: Client에게 Request를 받으면 Response를 보낸다. 또한 Client가 사용할 수 있는 User Data를 생성하고 저장해놓는 역할을 한다.

### Packet Type

BLE 통신에서는 두 가지 종류의 패킷인 Advertising Packet, Data Packet만이 존재한다. Connection을 맺기 전에는 Advertising Packet type, 맺은 후에는 Data Packet type으로 Signal을 생성한다. Data Packet은 하나로 통일되지만, Advertising Packet은 특정 기준에 따라서 다음과 같은 성질들을 갖는다.

**Connectability**

- **Connectable**: Scanner가 Connectable Advertising Packet을 받으면, Scanner는 이를 Advertiser가 Connection을 맺고 싶어한다는 신호로 받아들인다. 그러면 Scanner는 Connection Request (이하 CONNECT*REQ)를 보낼 수 있다. 해당 Connectable Signal을 보낸 Advertiser는 Scanner가 CONNECT*REQ가 아닌 다른 타입의 Signal을 보내면 해당 Packet을 무시하고 다음 Channel로 이동하여 계속 Advertising을 진행한다.
- **Non-Connectable**: Non-Connectable Packet을 받은 Scanner는 CONNECT_REQ를 보낼 수 없다. 주로 Connection 목적이 아닌, Data 전달이 목적일 때 쓰인다.

**Scannability**

- **Scannable**: Scanner가 Scannable Advertising Packet을 받으면, Scan Request (이하 SCAN*REQ)를 보낼수 있다. Scannable Signal을 보낸 디바이스는 Scanner가 SCAN*REQ가 아닌 다른 타입의 Signal을 보내면 해당 Packet을 무시하고 버린다.
- **Non-Scannable**: Non-Scannable Signal을 받은 Scanner는 SCAN_REQ를 보낼 수 없다.

**Directability**

- **Directed**: Packet안에 해당 Signal을 보내는 디바이스의 MAC Address와 받는 디바이스의 MAC Address가 들어있다. MAC Address 이외의 데이터는 넣을 수 없다. 모든 Directed Advertising Packet은 Connectable 성질을 갖는다.
- **Undirected**: 해당 Signal을 받는 대상이 지정되어 있지 않다. Directed Advertising Packet과는 다르게, 사용자가 원하는 데이터를 넣을 수 있다.

위의 내용을 종합하면, Advertising pakcet을 아래와 같이 4가지 type으로 나눌 수 있다.

![Untitled](BLE%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB%20d7cc4/Untitled%206.png)

### 실제로는 어떻게 통신할까?

BLE 통신의 핵심은 ‘timing’이다.

**Before Connection**

Connection 전, 디바이스는 3개의 Advertising Channel을 이용해서 데이터를 주고 받는다고 했다. 이들은 이 3개의 Channel을 자신만의 time interval로 hopping한다. 서로의 hopping 규칙이 일치하지 않기 때문에 Channel이 서로 엇갈리는 경우가 많을 것이다. 예를 들어, Advertiser는 1번 Channel에 Advertising Packet을 보냈는데, 같은 시간에 Scanner는 3번 Channel에 대해서 Scanning을 하게 되면 데이터 전달이 되지 않는 것이다. 하지만 이러한 hopping이 빠르게 자주 일어나기 때문에, 두 디바이스가 같은 Channel에 대해 Advertising와 Scanning이 발생하는 경우도 많이 생긴다. 이 경우에 서로 데이터를 주고 받을 수 있다.

**After Connection**

Connection이 되면, Advertising은 종료되고 기기들은 Central, Peripheral 중 하나의 역할을 하게된다. Connection을 개시한 기기가 Central이며, Advertiser가 Peripheral이 된다. 그리고 두 디바이스는 엇갈렸던 hopping 규칙을 통일시킨다. 그렇게 함으로써, 매번 같은 채널로 동시에 hopping하면서 Signal을 주고 받을 수 있게 된다. 이는 둘 간의 Connection이 끊어질 때까지 지속된다.

### 실제로는 어떻게 통신할까? - 예시

Zikto Walk라는 스마트 밴드가 있다. 디바이스간의 BLE 연결을 iPhone과 Zikto Walk와의 연결과정으로 설명하면 다음과 같다.

1.  Zikto Walk가 Advertising Channel을 hopping하면서 Advertising Packet을 보낸다.(Zikto Walk의 Advertising Packet 유형은 ADV_IND이다)
2. iPhone Bluetooth를 켠 후, Zikto 앱에 Zikto Walk를 등록한다. iPhone은 Advertising Channel을 hopping하면서 Scan을 하다가 연결하려는 Zikto의 디바이스 이름 등의 추가적인 정보를 얻기위해 SCAN_REQ를 보낸다.
3. SCAN*REQ를 받은 Zikto Walk는 SCAN*RSP를 보낸다.
4. Pairing이 완료되고, Zikto Walk는 다시 Advertising Packet을 다시 일정 주기마다 보낸다.
5. iPhone에서 Zikto Walk로부터 걸음 수 등의 Data를 받기 위해 Sync 버튼을 누른다. 이 버튼을 누르면 iPhone은 CONNECT_REQ를 보낸다.
6. Zikto와 iPhone은 서로 Acknowledging을 시작하고, timing 정보 등을 동기화 한다.
7. Connection이 완료된다.
8. Connection이 완료된 후, Service Data, Characteristic Data 등에 대한 Data 교환이 일어난다.
9. iPhone과 Zikto Walk간에 Data Sync가 완료되면, Connection이 해제되고, 다시 Advertising Packet을 보낸다. 이를 그림으로 표현하면 아래와 같다.

![Untitled](BLE%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%86%AB%20d7cc4/Untitled%207.png)

> 참고링크:
[https://www.theteams.kr/teams/2664/post/67853](https://www.theteams.kr/teams/2664/post/67853)
>