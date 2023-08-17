## 무선 인터페이스 구조
### 1. 전반적인 시스템 구조
Core Network & Random Access Network로 구성
>### Core Network
>무선 접속과 직접적인 관련은 없지만 네트워크를 원활하게 제공하기 위해 필요한 기능들을 담당
>
>3G -> 4G 때에는 4G의 무선 접속 기술을 3G의 코어 네트워크에 연결할 수 없었으나 4G -> 5G 때에는 기존 LTE의 코어 네트워크 EPC(Evolved Packet Core)를 기반으로 설계하여 5G 무선접속기술을 4G 코어네트워크에 연결하는 혼용모드 NSA(Non-Standalone Mode)도 사용 가능
>
>NSA를 사용함으로써 4G의 부족한 서비스 품질과 5G의 부족한 인프라를 서로 보완하는 형식으로 사용 가능
>
>4G의 코어 네트워크를 기반으로 설계된 5G의 5GCN은 4G와 비교했을 때 서비스 기반의 아키텍처, 네트워크 슬라이싱 지원, 컨트롤/유저 플레인 분리의 3 가지 부분에서 업그레이드 됨
>
>1. 서비스 기반의 아키텍처
>   
>     5G 자체가 지향하는 바와 맞닿아있음
>
>     다양한 사용자의 다양한 요구에 맞춰 가상화를 바탕으로 다양한 서비스를 제공하는 것을 목표로하기 때문에 자연스러운 부분
>
>2. 네트워크 슬라이싱 지원
>  
>     서비스 기반 아키텍처와 맞물려 가상화 바탕의 네트워크를 분리하여 사용자의 요구사항에 맞는 서비스를 지원
>
>     이를 통해 5G의 세가지 시나리오를 사용자의 요구에 맞게 제공 가능
>   
>     코어 네트워크의 구조상 저지연, 고속 통신이 어려웠던 4G와 다르게 5G 코어 네트워크는 가상화를 기반으로하기 때문에 edge cloud로 전국에 형성된 코어망으로 사용자 바로 근처에서 저지연, 고속 통신을 비롯한 다양한 요구사항을 보장 가능하게 하는 edge computing이 가능하게 됨
>   
>     이러한 엣지컴퓨팅도 초 저지연을 요구하는 서비스를 제공하기 위한 방법의 하나이므로 네트워크 슬라이싱의 일환으로 볼 수 있음
>3. CUPS
>
>     User Plane : 통신의 주요 신호인 사용자 데이터가 오가는 영역
>
>     Control Plane : 통신 및 인증을 설정하기 위한 제어신호들이 오가는 영역
>
>     기존의 단일 환경에서는 사용자 데이터와 제어 데이터가 같은 영역에서 처리되기 때문에 용량 초과 문제가 발생할 수 있음
>   
>     예를 들어 사용자 데이터가 급증하여 용량을 많이 잡아먹는 경우, 제어 데이터 처리에 필요한 용량이 부족해지는 상황이 발생할 수 있는 것
>   
>     CUPS는 CP과 UP를 분리하여 독립적으로 각각의 용량을 조절하기 때문에 그러한 문제 상황을 피할 수 있음
>
>     또한 C-RAN에서 BBU를 따로 빼내서 중앙 집중형으로 사용한 것과 같은 맥락으로, 하나의 CP로 수많은 UP을 제어하도록 배치하여 효율을 높일 수 있으며 트래픽이 많은 곳에 UP를 많이, 적은 곳에 적게 배치한다거나 특정 트래픽 특성에 특화된 UP를 배치하는 등 UP를 유연하게 배치함으로써 사용자의 요구사항도 잘 보장할 수 있고 CP를 소수만 설치하면 되므로 비용 절감도 되는 등 다방면으로 효율을 높일 수 있게 됨
>   
>     UP의 기능은 RAN과 외부 네트워크 사이의 게이트웨이인 UPF라는 유닛에서 이루어지며 패킷 라우팅, 포워딩, 패킷 검사, QoS (Quality of Service) 처리, 패킷 필터링, 트래픽 측정 등의 역할을 수행
>   
>     CP의 기능은 여러 유닛들이 특정한 기능을 수행하도록 구성되어있음
>
>   - SMF (Session Management Function): 단말에 대한 IP 주소 할당, 정책 집행 제어, 일반적인 세션 관리
>   - AMF (Access Managenent Function): 코어 네트워크와 단말 간의 제어, 유저 데이터의 보안, idle state(프로세스가 실행되지 않는 상태)에서의 이동성, 인증
>   - PCF (Policy Control Function): 정책 규칙
>   - UDM (Unified Data Management): 인증 증명서와 엑세스 허가 기능
>   - NEF (Network Exposure Function)
>   - NRF (Network Repository Function)
>   - AUSF (Authentication Server Function)
>   - AF (Application Function)
>     등 다양한 유닛들이 존재하여 각각의 기능을 수행
>     
>     network function 단위로 나뉘어진 구성요소들이 서로 interface를 통해 연결되어있으며 이러한 network function과 interface들은 SDN, NFV 기반으로 구현 가능
>

>### Random Access Network
>5G는 아직 4G와 혼용되고있는 부분이 있어 기지국의 종류를 gNB, eNB, ng-eNB로 구분할 수 있는데, NR 기지국을 gNB, ECP와 연결된 LTE 기지국을 eNB, 5GCN과 연결된 LTE 기지국을 ng-eNB라고 하며, eNB, gNB가 공존하는 RAN을 ng-RAN이라고 함
>
>gNB들은 5GCN과 NG 인터페이스를 통해 CP는 AMF (Access Managenent Function)와, UP는 UPF와 연결되고, gNB들 사이는 Xn 인터페이스로 연결됨
>
>ng-ran은 RU-DU-CU로 disaggrigate되어있으며, DU-CU 사이를 연결하는 midhaul interface를 F1으로 정의
>
>gNB들이 서로 연결된 Xn 인터페이스는 RRC (Radio Resorce Control)가 active state일 때의 손실 없는 mobility 지원, 다중 셀의 RRM (Radio Resorce Management), dual connectivity 지원 등에 사용됨
>
>기지국 eNB, gNB-DU와 UE 사이를 연결하는 인터페이스는 Uu interface로, 이를 통해 UE는 하나 이상의 셀과 연결되어 통신을 할 수 있음
>
>하나의 단말이 두개의 셀그룹에 붙는 것을 dual connectivity라고 하는데, LTE와 NR 간 dual connectivity는 NR의 첫 번째 버전인 NSA option3의 기반이 되는 형태로, LTE 기반의 마스터셀 eNB는 CP, UP 모두를, NR 기반의 세컨더리셀 gNB는 UP만 다룸으로서 높은 데이터 전송 속도를 얻을 수 있음

#### Carrier Aggrigation VS Dual Connectivity
##### 둘 모두 단말이 여러 셀에 접속하는 상황이라는 점에서 비슷해 보이지만 가장 큰 차이는 셀들이 같은 gNB에 위치하는지 다른 gNB에 위치하는지 여부
##### carrier aggregation은 같은 gNB 내의 셀들에 접속을 함으로서 서로 다른 두 셀의 서로 다른 두 carrier를 마치 하나의 넓은 BW를 갖는 carrier처럼 사용할 수 있게 되고, 그로부터 높은 data rate를 보장받는 방식
##### 같은 MAC 레이어의 스케줄러에 의해 스케줄링 결정이 이루어지므로 같은 MAC 레이어로부터 두 링크가 갈라져나옴
##### 그에 반해 dual connectivity는 셀들이 서로 다른 gNB에 접속하거나 NAS 모드처럼 아예 서로 다른 무선 접속 기술을 사용하는 등의 상황을 말하며, 이 경우 서로 다른 대역과 커버리지를 갖는 두 기지국을 동시에 사용함으로서 LTE의 안정된 커버리지에 NR의 고속 데이터 전송을 추가로 사용하여 높은 서비스 품질을 제공하는 방식
##### 서로 다른 gNB 또는 gNB와 eNB에 연결되는 것으로, 서로 다른 기지국과의 무선 연결이 이루어져야하므로 이를 관리하는 PDCP 레이어로부터 두 링크가 나옴

### 2. QoS 처리
### 3. 무선 프로토콜 아키텍처
### 4. 유저 플레인 프로토콜
### 5. 컨트롤 플레인 프로토콜
