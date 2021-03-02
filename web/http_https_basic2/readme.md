HTTP - HTTPS 2

해당 정리 세가지 깃 허브를 참고하였습니다.

(https://github.com/WeareSoft/tech-interview)

(https://github.com/JaeYeopHan/Interview_Question_for_Beginner)

(https://github.com/gyoogle/tech-interview-for-developer)



추가 적으로 참고한 자료

(https://developer.mozilla.org/ko/docs/Web/HTTP)\

(https://samsikworld.tistory.com/489)

(https://nsinc.tistory.com/192)

(https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake)



- <b>HTTP헤더</b>
  - HTTP헤더는 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 서로 통신할 수 있도록 하는 기능을 한다. 
  - HTTP 헤더는 대소문자를 구분하지 않는 이름과 콜론 다음에 오는 값으로 이루어져있다
  - HTTP 헤더 종류 <a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Headers">클릭</a>
- <b>HTTP의 동작과정</b>
  1. <b>사용자가 웹 브라우저에 URL 주소를 입력한다</b>
  2. <b>DNS 서버에 웹 서버의 호스트 이름을 IP주소로 변경요청한다</b>
     - DNS란?
       - DNS란 TCP/IP 네트워크 상에서 사람이 기억하기 쉽게 문자로 만들어진 도메인을 컴퓨터가 처리할 수 있는 숫자로 된 인터넷주소(IP)로 바꾸는 시스템 혹은 이런 역할을 하는 서버 컴퓨터를 의미함
       - 즉 www.naver.com을 검색하면 dns는 이 주소를 naver의 ip주소로 연결 시켜줌
     - DNS의 탄생배경
       - 1. 네트워크가 개발되면서 개인의 PC는 서로를 식별할 수 있는 IP를 할당 받음
         2. 그리하여 우리는 다른사람과 통신을 할때 그 사람의 IP를 입력함으로서 손쉽게 할 수 있게됨
         3. 그러나 네트워크의 이용자가 기하급수적으로 늘어남에 따라 모든 IP를 익혀서 사용하기가 불가능에 가까워짐
         4. 그래서 DNS라는 시스템이 고안됨. DNS는 특정 문자열등에 특정 IP를 저장하여 사용자에게 제공함. 그리하여 사용자는 특정 사이트에 접속할때 IP가 아닌 저장된 문자열을 입력하면 됨 www.naver.com이 하나의 문자열을 의미한다고 생각하면됨
         5. 물론 www.naver.com의 입력을 누군가 받아서 네이버의 고유 ip로 바꿔주는 역할을 해야함. 그역할을 하는게 바로 dns서버임 
  3. <b>웹 서버와 TCP는 연결을 시도함</b>
     - 3 way-handshaking
       - TCP는 장치들 사이에 논리적인 접속을 성립하기 위하여 3-way-handshaking을 사용함
       - 3-way-handshaking이란 TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정
       - Client > Server : TCP SYN
       - Server > Client : TCP SYN ACK
       - Client > Server : TCP ACK
       - `SYN과 ack는 tcp의 접속을 성공적으로 성립하기 위해 반드시 필요하다`
       - <b>step1</b>
         - a클라이언트는 b서버에 접속을 요청하는 syn 패킷을 보낸다.  a클라이언트는 syn을 보내고 syn/ack응답을 기다리는 stn_sent상태가 된다
       - <b>step2</b>
         - b서버는 syn요청을 받고 a클라이언트에게 요청을 수락한다는 ack와 syn flag가 설정된 패킷을 발송하고 a가 다시 ack로 응답하기를 기다린다. b서버는 syn_received상태가 된다
       - <b>step3</b>
         - a클라이언트는 b서버에게 ack를 보내고 이후부터는 연결이 이루어지고 데이터가 오간다. 이때 b서버는 established상태가 된다.
       - <b>tcp의 연결을 초기화 할때 사용한다고 볼 수 있음</b>
  4. <b>클라이언트가 서버에게 요청</b>
     - HTTP Request Message = Request Header + 빈 줄 + Request Body
     - Request Header
       - 요청메소드 (ex.GET, POST 등) + 요청 URI + HTTP 프로토콜 버전
       - Header 정보는 (key-value쌍을 이룸)
       - `URI란?`
         - `네트워크 상에 존재하는 자원을 구분하는 식별자(ID)`
         - URL(네트워상에 있는 자원의 위치) 와 URN(자원의 이름: 유일한 값)과 미세하게 다르며 이 둘을 포함함
         - URI는 URL과 URN을 포함함
           - `예를 들자면 최우석의 주민등록번호는 URN 최우석의 집주소는 URL`
     - 빈줄
       - 메타정보가 전송되었음을 알리는 용도
     - Request Body
       - GET,HEAD,DELETE등 리소스를 가져오는 요청은 바디를 포함하지 않음
       - 데이터 업데이트 요청과 관련된 내용 (HTML 폼 콘텐츠)
  5. <b>서버가 클라이언트에게 데이터 응답</b>
     - HTTP Response Message = Response Header + 빈 줄 + Response Body
     - Response Header
       - HTTP 프로토콜 버전 + 응답 코드 + 응답메세지
       - <a href="https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C"> HTTP 응답코드 보기</a>
       - Header 정보 (key-value쌍으로 이루어짐)
     - 빈줄
     - Response Body
       - 응답리소스 데이터
         - 201과 204 상태코드느느 바디가 포함되지 않음
  6. <b>서버와 클라이언트간 연결 종료</b>
     - 4way-handshaking
     - <b>step1</b>
       - 클라이언트가 연결을 종료하겠다는 FIN 플래그를 전송한다
     - <b>step2</b>
       - 서버는 일단 확인 메세지를 보내고 자신의 통신이 끝날때 까지 기다리는데 이때 상태가 TIME_WAIT이다. -> 서버에서 보내는 응답이 느릴 수 있어서 대기시간을 주는 느낌
     - <b>step3</b>
       - 서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 Fin 플래그를 전송한다
     - <b>step4</b>
       - 클라이언트는 확인했다는 메세지를 보낸다
     - 만약 Server에서 보낸 fin보다 패킷이 늦게 도착할 경우, 데이터는 유실된다. 그래서 client는 server로부터 fin을 수신해도 일정시간(디폴트 240초)동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정을 거친다. 이것이 step2의 time_wait
     - <b>세션을 종료하기 위해 수행되는 절차이다</b>
  7. <b>웹브라우저는 응답받은 웹 문서를 출력한다</b>