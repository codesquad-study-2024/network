# story 01 케이블과 리피터, 허브속을 신호가 흘러간다

## 1 하나하나의 패킷이 독립된 것으로 동작한다
- 리피터 허브, 스위치 허브 같은 중계 장치는 패킷 내부에 관심이 없다.
  - `서버 - 클라이언트` 관계인지 관심이 없다.
- 따라서 모든 패킷은 아무 관련없는 별개의 패킷으로 간주한다.
  - 우체국이 편지 내용에 관심없는 것과 같다.

## 2 LAN 케이블은 신호를 약화시키지 않는 것이 핵심이다
![리피터와 LAN](https://gist.github.com/user-attachments/assets/54ccf76f-3b51-4b0f-bb09-506cdd2942cd)
- 패킷은 LAN 어댑터의 PHY(MAU) 회로에서 전기 신호로 변환된다.
- 변환된 신호(패킷)는 RJ-45 커넥터에 연결된 케이블(트위스티드 페어 케이블)에 들어간다.
  - 신호는 플러스(+)와 마이너스(-) 신호 단자에서 흘러나온다.
  - 케이블은 8개의 구리선으로 되어있고, 4개만 사용된다. (2쌍만 사용)

![신호](https://gist.github.com/user-attachments/assets/145d9e4e-1721-460d-84a3-44ba4d4a5932)
- 케이블의 길이가 길어질수록 신호가 약해진다.
- 높은 주파수는 멀어질수록 신호가 약해지는 정도(신호 감쇄)가 심해진다.
  - 즉, 급격하게 변화가 줄어들어 각이 뭉개진다.
- 노이즈까지 끼게되면 왜곡이 더욱 심해진다.
  - `0`과 `1`을 잘못 판독 --> 통신 오류의 원인이 된다.

## 3 '꼼'은 잡음을 방지하지하기 위한 방법이다
### 전자파(=전자기파)란?
![전자기파](https://mblogthumb-phinf.pstatic.net/MjAyMDA5MjlfMjMg/MDAxNjAxMzUxNzIzNzYw.6Kmqs7bwWIG9j9FjmqM5urXOxynksVz2LwfjVMnQDfEg.9wJUGCXTFHroqM9BL5kK8IyqROZmI5gKevldEObzf4Eg.PNG.ktojja/2.png?type=w800)
![전자파의 발생 및 전파](https://emf.kca.kr/resources/img/sub/sub0101_04.png)
- 주기적으로 세기가 변화하는 전자기장이 공간 속으로 전파해 나가는 현상
  - `전기장`, `자기장`, `진행 방향` 은 서로 수직
- 전자기파가 구리선에 닿을 때 전류 방향은 오른손 법칙에 의해 결정된다.
  - 엄지가 `전자기파`의 진행방향
  - 집게 손가락이 `전기장`의 방향. `전기장`이 구리선의 전하를 움직여 전류를 생성한다.
  - 중지가 `전류`의 방향 (도체에 닿으면 전기장에 의해 유도되서 전류가 발생)
- 출처: [네이버 블로그](https://blog.naver.com/ktojja/222103790571), [KCA](https://emf.kca.kr/web/overview/emwave.do)

### 외부에서 발생한 전자기파
- 노이즈의 원인은 케이블 주위에서 발생하는 전자기파다.
![주위에서 발생한 잡음](https://mblogthumb-phinf.pstatic.net/20140603_263/kwshop89_14017770522251p3hq_JPEG/UTP-TWIST_%BF%DC%BA%CE.jpg?type=w2)
- 전자기파가 케이블에 닿으면 플러스 선과 마이너스 선에 동일한 전류를 발생시킨다.
- 하지만 플러스 선과 마이너스 선이 맞닿는 곳에서 서로 상쇄되어 노이즈 전류만 상쇄된다.

### 크로스토크(crosstalk)
![크로스토크](https://mblogthumb-phinf.pstatic.net/20140603_287/kwshop89_1401777052021YQBlr_JPEG/UTP-TWIST_%B3%BB%BA%CE.jpg?type=w2)
- 같은 케이블 내부에서 맞닿는 신호선에서 발생하는 전자기파
- 신호선을 꼬는 간격을 미세하게 다르게 해서 해결한다.
![꼬는 간격 다르게 하기](https://mblogthumb-phinf.pstatic.net/20140603_182/kwshop89_1401777051853R98oN_JPEG/LS-CAT5E-Cable_04.jpg?type=w2)
![간격이 동일](https://gist.github.com/user-attachments/assets/239bfc41-f1a7-4b20-8826-59f9db9f3ab8)

### STP(Shielded Twisted-Pair)
- 전자파를 차단하기 위해 `금속성의 실드(차폐)라는 피복`을 입힌 트위스티드 페어 케이블
- 신호선 사이에 구분판을 넣은 것도 STP 라고 함

## 4 리피터 허브는 연결되어 있는 전체 케이블에 신호를 송신한다 
- 리피터 허브에 신호가 도달하면 LAN `전체`에 신호를 뿌린다.
  - 기본 동작은 신호를 `그대로` 커넥터 부분에 송출한다.
  - 잡음 때문에 왜곡된 신호도 `그대로` 송출한다. --> FCS(Frame Check Sequence) 검사하는 곳에서 검사하기 떄문
- 신호를 수신한 기기들은 `MAC 헤더` 를 확인한다음 자신의 `MAC 주소` 와 일치하면 신호를 수신하고, 그렇지 않으면 폐기한다.
- 패킷이 폐기 --> TCP 계층까지 전달이 되지 않음 --> ACK 응답을 보내지 않음 --> 송신처 `프로토콜 스택의 TCP 담당`이 ACK 를 못받았기 때문에 패킷을 재전송

### 신호를 주고 받기 위한 커넥터 연결
![리피터와 LAN](https://gist.github.com/user-attachments/assets/54ccf76f-3b51-4b0f-bb09-506cdd2942cd)
- 신호를 제대로 수신하려면, 송신측 송신 단자(플러스, 마이너스 선)에서 보낸 신호를 수신측 수신 단자(플러스, 마이너스 선)에 연결
  - 한쪽이 송신만 하기 때문에 다른 한쪽은 수신만 해야하는 원리
  - 따라서 신호를 수신하려면 `수신측 PHY(MAU) 회로`와 `교차(MDI-X)`시켜서 연결

### 크로스 케이블
- 허브의 커넥터 부분(RJ-45)은 보통 `MDI-X` 형태
  - 허브끼리 접속하려면 한 쪽은 `MDI`로 설정해야 한다.
  - 만약 연결하려는 허브들의 모든 커넥터가 `MDI-X` 라면 `크로스 케이블`을 사용한다.
  - ![크로스 케이블 2](https://gist.github.com/user-attachments/assets/5cf840f9-a0ef-4280-abb1-607e38faff32)
- PC 끼리 (= LAN 어댑터 끼리) 접속하려면?
  - LAN 어댑터의 커넥터 부분(RJ-45)은 `MDI` 형태
  - 따라서 `크로스 케이블`을 사용하면 된다.
  - ![크로스 케이블](https://gist.github.com/user-attachments/assets/5a726a47-7aaf-4c3d-bef1-32479e861a3a)
