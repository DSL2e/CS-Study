# 1. Presentation Layer

프레젠테이션 계층은 OSI 7 모델의 6번째 계층이다.

데이터를 전달하기 전에 송신 측에서 데이터를 수신 측에서 이해할 수 있는 형식으로 변환하고, 수신 측에서 이를 원래의 형식으로 변환하는 역할을 수행한다.

- 통신 시스템 간에 교환되는 데이터가 송신자와 수신자가 모두 이해할 수 있는 형식으로 표현되도록 보장하는 역할을 수행한다.
  - 한 시스템의 애플리케이션 계층에서 다른 시스템의 애플리케이션 계층에서 읽을 수 있는 데이터를 전송하는 것을 보장



## 1-1. 기능
데이터 표현, 암호화/복호화 및 압축/해제 등의 기능을 제공한다.

### A. 데이터 변환

시스템에 따라 달라지는 데이터 표현을 독립적인 형태로 설정하여 서로 다른 시스템 간에 구문적으로 올바른 데이터 교환을 가능하게 한다.
- 문자 인코딩(예: ASCII, EBCDIC)
- 데이터 구조 변환(예: XML, JSON)
- 이미지 형식 변환(JPEG, PNG)

### B. 암호화/복호화
송신 측에서 데이터를 암호화하여 전송하고, 수신 측에서 이를 복호화 한다.

 SSL(Secure Socket Layer) / TLS(Transport Layer Security)는 프레젠테이션 계층에서 암호화된 데이터 통신을 제공한다.

### C. 데이터 압축/해제
대역폭 사용을 줄이기 위해 데이터를 압축하여 전송하고, 수신 측에서 이를 해제한다.

---

# 2. Session Layer

세션 계층은 OSI 모델의 5번째 계층으로 데이터가 통신하기 위한 논리적 연결을 담당한다.

- 두 통신 기기 간의 **세션(연결)**을 설정, 관리, 종료하는 역할을 수행
- 통신 중에 연결이 끊어지거나 오류가 발생할 경우 세션을 복구하거나, 데이터를 순서대로 전달할 수 있도록 보장

최종 사용자 애플리케이션 프로세스 간의 세션을 열고, 닫고, 관리하는 메커니즘을 제공

## 2-1. 기능
### A. 세션 관리 매커니즘 제공
#### a. 세션 설정
클라이언트와 서버 간의 세션을 설정하여 두 장치가 서로 통신할 수 있게 한다.

#### b. 세션 관리
세션 동안 데이터 전송의 동기화, 체크포인트 설정, 흐름 제어 등을 관리하여 데이터가 손실되거나 중복되지 않도록 한다.

#### c. 세션 종료
데이터 전송이 완료되면 세션을 종료하고, 자원을 해제한다.

#### d. 세션 예시
RPC(Remote Procedure Call): 원격 프로시저 호출은 세션 계층에서 세션을 설정하고 관리하여, 클라이언트가 서버의 특정 함수나 프로시저를 호출할 수 있게 합니다.

SQL 데이터베이스 연결: 클라이언트 애플리케이션과 데이터베이스 서버 간의 세션을 설정하고, 쿼리 요청 및 응답을 관리하는 과정도 세션 계층에서 수행됩니다.

파일 전송: FTP(File Transfer Protocol)에서 파일을 전송할 때, 세션 계층은 파일 전송이 도중에 중단되었을 때 복구하는 기능을 제공합니다.

화상 회의 및 VoIP: 화상 회의나 VoIP(Voice over IP)에서 세션 계층은 회의나 통화 중에 세션을 관리하며, 오류 발생 시 재연결을 시도하거나 회의를 종료합니다.

# References
> [Presentation Layer in OSI model, GeeksForGeeks](https://www.geeksforgeeks.org/presentation-layer-in-osi-model/)

> [Session Layer in OSI model, GeeksForGeeks
](https://www.geeksforgeeks.org/session-layer-in-osi-model/) [Presentation Layer, osi-model.com](https://osi-model.com/presentation-layer/)

> [Seesion Layer, osi-model.com](https://osi-model.com/session-layer/)

> [InfosecTrain, What is Presentation Layer in the OSI Model?, Medium](https://medium.com/@Infosec-Train/what-is-presentation-layer-in-the-osi-model-4f587383fec0)
