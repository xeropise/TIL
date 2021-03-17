### X-Forwarded-For, X-Forwarded-Host, X-Forwarded-Proto

***

__1) X-Forwarded-For__

  - XFF(X-Forwarded-For) 은 HTTP Header 중 하나로, HTTP Server 에 요청한 client 의 IP 를 식별하기 위한 표준으로, 웹서버나 WAS 앞에 L4 같은 Load balancer 나 
  Proxy Server, Caching Server, HTTP 서버용 WAS Connector (웹로직 커넥터 - mod_wl, 톰캣 커넥터 - mod_jk 등) 이 있을 경우, 웹서버/WAS 에 HTTP나 전용 프로토콜로 요청을 보낸후에
  받은 결과를 가공하여 클라이언트에 재전송하게 된다. 
  
  - 클라이언트(요청) - 중개 서버 - 중개 서버 - 최종 서버 로 보통 이루어져있는 경우가 많다.
  
  - 처리한 웹 서버나 WAS에서 _request.getRemoteAddr()_ 메서드로 클라이언트의 IP를 확인 하는 경우, L4나 Proxy 의 IP 를 얻게 되는데, 이는 요청을 보냈던 클라이언의 IP가 아니다.

  - XFF 는 이 문제를 해결하기 위해 사용되는 HTTP Header 로 콤마(,) 를 구분자로 client 와 proxy IP가 모두 기록된다.

  - 요청이 여러 프록시를 거치는 경우, 다음과 같이 기록이 되는데,
  ```
  X-Forwarded-For : OriginatingClientIPAddress, proxy1-IPAddress, proxy2-IPAddress
  ```
  
  여기서 __맨 왼쪽이 원래 클라이언트의 IP 주소, 오른쪽이 가장 최근의 프록시 서버의 IP 주소__ 이다. 근데 이것도 조작할 수 있다고하니 주의해서 사용하자.
  
  <br>
  
__2) X-Forwarded-Host__

  - XFF 비슷한 개념으로 [Host](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Host) 를 알 수 있다.
 
  <br> 

__3) X-Forwarded-Proto__

  - XFF 비슷한 개념으로 프로토콜을 알 수 있다.
