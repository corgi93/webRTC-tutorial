# webRTC 정리

### 구현되어 있는 api

- MediaStream (getUserMedia라 불리는)
- RTCPeerConnection
- RTCDataChannel


<b>getUserMedia</b>는 chrome , opera, firefox에서 가능.<br>
<b>RTCPeerConnection</b>은 Chrome(데스크탑, 안드로이드) , Opera(데스크탑, 최신 안드로이드 beta)와 firefox에서 구현되 있다.

chrome - webkitRTCPeerConnection으로 구현되 있다.<br> 
firefox - mozRTCPeerConnection으로 구현되어 있다.<br>
이외에 거쳐간 다른 이름들이 많이 있었는데, 이제 표준화 과정이 안정화 되어가고 있다.<br>

<b>RTCDataChannel</b>은 Chrome 25버전, Opera 18버전, Firefox 22버전 이상에서 지원하고 있다.

**IE에서는 Chrome Frame을 통해 WebRTC기능을 사용 가능. Skype에서는 WebRTC를 사용하려 계획 중.

!주의 : webRTC를 지원하는 플랫폼 중 RTC컴포넌트들은 전혀 지원하지 않고, getUserMedia만 지원하는 플랫폼도 많다.

--- 


## webRTC 
### webRTC 애플리케이션에 반드시 해야하는 순서

1. 사용자의 streaming 오디오, 비디오 또는 데이터를 가져옴
2. ip주소, 포트 등의 네트워크 정보 가져와 다른 WebRTC클라이언트들(peer)과 연결하기 위해 이 정보들을 교환. 심지어 NATs와 방화벽을 통해서도 교환해야 합니다.
3. 애러들의 보고, 세션들의 초기화와 종료를 위한 신호 통신을 과리
4. 해상도와 코덱들 같은 미디어와 클라이언트의 capability에 대한 정보를 교환
5. 스트리밍 오디오, 비디오 또는 데이터를 주고 받는다.

### 스트리밍 데이터를 얻고 통신하기 위해 다음 api들을 사용
- MediaStream : 사용자의 카메라, 마이크 같은 곳의 데이터 스트림에 접근
- RTCPeerConnection : 암호화 및 대역폭 관리를 하는 기능을 가지고 있고, 오디오 또는 비디오를 연결
- RTCDataChannel : 일반적인 데이터 P2P 통신

보통 이 3가지의 객체를 통해 데이터 교환이 이뤄지며 RTCPeerConnection들이 적절하게 데이터를 교환할 수 있게 처리하는 과정이 시그널링이라 한다.


## 용어 설명

--- 

### STUN서버와 TURN서버 

webRTC는 p2p로 작동하도록 설계되었으므로 사용자는 가능한 가장 직접적인 경로로 연결할 수 있다.
하지만 webRTC는 실제 네트워킹에 대처하도록 구축됨. 
<br>webRTC 클라이언트 애플리케이션은 NAT과 방화벽을 통과해야하고 직접 연결에 실패하면 p2p 네트워킹이 필요.

<br> 이 프로세스의 일부로 WebRTC API는 STUN 서버를 사용해 컴퓨터의 IP주소를 가져오고 p2p통신이 실패하는 경우 TURN 서버가 릴레이 서버로 작동한다.

"
webRTC는 peer-to-peer로 작동하게 설계되어있다. peer간의 공인 네트워크 주소(ip)를 알아내어 서로 데이터를 교환해야 하는데, 실제 각각의 컴퓨터는 NAT, 방화벽 같은 보호장치들이 있고 그것들을 통과해야한다. 실제 네트워킹을 대처하도록 만들어진 webRTC는 이런 쉽지 않은 연결 문제를, 서로 연결을 위한 정보를 공유하며 peer-to-peer통신을 가능하게 한 것이 stun/turn서버이다.
"

### Signaling
RTCPeerConnection을 사용해 브라우저 간에 스트리밍 데이터를 통신하지만 통신을 조정하고 메세지들을 컨트롤하는 메커니즘(과정)을 시그널링이다.
