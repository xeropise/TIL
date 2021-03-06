### 마이크로 서비스 아키텍처(MSA : Micro Service Architecture) 란?

![monolithic-vs-microservices](https://user-images.githubusercontent.com/50399804/123040817-589a7100-d42f-11eb-8bb8-a64ae395a89b.png)\



- 일체형 애플리케이션(모놀리식 아키텍처 - Monolithic Architecture) 을 작은 컴포넌트로 나누는 것
- 일체형 애플리케이션에서는 유지보수 및 개선방법이 점점 더 어려워질 뿐만 아니라 수직 스케일링(스케일 업)의 한계가 있음
- 독립적으로 출시 및 스케일링할 수 있는 작은 컴포넌트로 전환하는 것을 말한다.



***



#### 마이크로 서비스 정의

- 마이크로서비스는 기본적으로 독자적인 업그레이드와 스케일링이 가능한 독립 소프트웨어 컴포넌트로 다음의 2개를 목표로 한다. 
  - 빠르게 개발해 지속적으로 배포할 수 있어야 한다.
  - 수동 혹은 자동으로 쉽게 스케일링 할 수 있어야 한다.

  

- 독립 컴포넌트로 동작하려면 다음의 기준을 충족해야 한다.

  - 아무것도 공유하지 않는 아키텍처 유지, 즉 데이터베이스 데이터를 공유하지 않는다.
  - 명확한 인터페이스를 통해서만 통신해야 한다. 동기 서비스를 사용하거나 API를 이용한 메시징 방식을 이용할 수 있는데, 메시지 형식은 버전 관리 전략에 따라 안정적으로 문서화되고 개선되어야 한다.
  - 개별적인 런타임 프로세스로 배포해야 한다. 각 마이크로서비스 인스턴스는 도커 컨테이너와 같이 독립된 런타임 프로세스로 실행해야 한다.
  - 마이크로서비스 인스턴스는 상태가 없다.(stateless) 모든 마이크로 서비스 인스턴스가 마이크로서비스로 들어오는 요청을 처리할 수 있다.



***



#### 마이크로서비스의 문제

- 동기식 통신을 사용하는 다수의 소형 컴포넌트는 연쇄 장애를 일으킬 수 있다.
- 다수의 소형 컴포넌트를 최신 상태로 유지하는 건 어렵다.
- 많은 컴포넌트가 처리에 관여하는 요청을 추적하기 어렵다. 각 컴포넌트가 로컬에 로그 이벤트를 저장하는 경우 근본 원인 분석을 수행하기 어렵다.
- 컴포넌트 수준의 하드웨어 자원 사용량 분석이 어렵다.
- 다수의 소형 컴포넌트를 수동으로 구성하고 관리하는 건 비용이 많이 들고 오류가 발생하기 쉽다.



***



#### 마이크로서비스 문제를 해결하기 위한 디자인 패턴

1. __서비스 검색(Service discovery)__

![Richardson-microservices-part4-2_client-side-pattern](https://user-images.githubusercontent.com/50399804/123041162-e8d8b600-d42f-11eb-90d7-bd796882d08c.png)



- 컨테이너 등에서 실행되는 마이크로 서비스 인스턴스는 시작하면서 동적 IP 주소를 할당 받는게 일반적인데, 마이크로서비스가 노출하는 HTTP 기반의 REST API를 클라이언트에서 호출하는 것이 어렵다.

  

- 현재 사용 가능한 마이크로 서비스와 그 인스턴스를 추적하는 새 컴포넌트 (서비스 검색 서비스)를 시스템 환경에 추가하는 것이다.

  - 마이크로서비스와 마이크로서비스 인스턴스를 자동으로 등록 및 해지
  - 클라이언트는 마이크로서비스의 논리 엔드포인트에 요청을 보낼 수 있어야 한다. 사용 가능한 인스턴스 중 하나로 라우팅 되어야한다.
  - 마이크로서비스에 대한 요청은 가용 인스턴스로 로드 밸런시돼야 한다.
  - 요청을 라우팅하지 않고자 상태가 비정상인 인스턴스를 감지할 수 있어야 한다.



​	

2. __에지서버__
   - 모든 요청을 거치는 시스템 환경에 새 컴포넌트(에지 서버)를 추가한다.
   - 리버스 프록시로 동작하며, 동적 로드 밸런싱 기능을 제공하고자 검색 서비스와 통합될 수 있다.
   - 외부로 공개하면 안 되는 내부 서비스는 숨기고, 외부 요청을 허용하는 마이크로서비스만 요청을 라우팅한다.
   - 서비스를 외부로 공개하되 악의적인 요청으로부터 보호한다. OAuth, JWT 토큰, OIDC, API 키 등의 활용하여 신뢰할 수 있는 클라이언트인지 확인한다.





3. __리액티브 마이크로서비스__
   - 블로킹 I/O 모델로 동기식 통신을 구현하는 경우 요청을 처리하는 동안 운영체제의 스레드를 점유하게 된다.
   - 논블로킹 I/O를 사용해 데이터베이스나 다른 마이크로서비스가 처리하길 기다리는 동안 스레드가 할당되지 않게 한다.





 	4. __구성 중앙화__
     - 다수의 마이크로 서비스 인스턴스가 배포된 마이크로서비스 아키텍처 기반의 시스템 환경에서 각각 여러 환경 변수나 파일에 담긴 구성 정보를 가지고 있는 경우 문제가 있다.
     - 시스템 환경에 모든 마이크로서비스의 구성 정보를 저장하는 새 컴포넌트(구성 서버)를 추가한다.





 	5. __로그 분석 중앙화__
     - 각 마이크로서비스 인스턴스가 로컬에 로그 파일을 기록하는 경우에는 전체 시스템 환경에서 발생하는 일들을 추적하기가 매우 어렵다.
     - 로그를 중앙화해 관리하고, 다음 기능을 갖춘 새 컴포넌트를 추가 해야 한다.
       - 새 마이크로서비스 인스턴스를 감지해 로그 이벤트 수집
       - 로그 이벤트를 해석해 구조적이고 검색 가능한 형식으로 중앙 데이터베이스에 저장
       - 로그 이벤트를 조회 및 분석하기 위한 API와 그래픽 도구 제공





6. __분산 추적__
   - 시스템환경에 대한 외부 호출을 처리하는 동안 마이크로서비스 사이에서 흐르는 요청 및 메시지를 추적할 수 있어야 한다.
   - 관련된 모든 요청 및 메시지에 상관 ID(correlation ID)를 넣어, 모든 로그 이벤트에 상관 ID가 있게하고, 중앙화된 로깅 서비스에서 이를 검색해 관련된 로그 이벤트를 모두 찾을 수 있다.



7. __서킷 브레이커__

   <img width="279" alt="sketch" src="https://user-images.githubusercontent.com/50399804/123042676-23dbe900-d432-11eb-973d-79d357ca3d07.png">

   

   - 동기 방식으로 상호 통신하는 마이크로 서비스 시스템 환경은 연쇄 장애가 발생할 여지가 있다.
   - 대상 서비스에 문제가 있다는 것을 감지해 새 요청을 보내지 않도록 차단하는 서킷브레이커를 추가한다. 
     - 서비스에 문제가 감지되면 시간 초과를 무시하고 바로 실패하도록 서킷을 연다.
     - 반열림 서킷(half-open-circut) 이라고도 하는 장애 복구용 프로브를 사용한다. 서비스가 정상 동작하는지 확인하고자 주기적으로 요청을 보낸다.
     - 프로브가 서비스의 정상 동작을 감지하면 서킷을 닫는다.





 	8. __제어 루프__
     - 다수의 마이크로서비스 인스턴스가 여러 서버에 분산되어 있는 시스템 환경에선 중단되거나 지연된 마이크로서비스 인스턴스를 수동으로 감지하고 대처하는 것이 어렵다.
     - 시스템 환경의 상태를 관찰하는 새 컴포넌트(제어 루프 - control loop)를 시스템 환경에 추가한다. 





9. __모니터링 및 경고 중앙화__

   

   ![image](https://user-images.githubusercontent.com/50399804/123043161-dca22800-d432-11eb-9047-ba1f0ce3a8e0.png)

   

   - 응답 시간이나 하드웨어 자원 사용량이 지나치게 높은 경우 문제의 근본 원인을 찾는게 매우 어렵다.
   - 마이크로서비스별 하드웨어 자원 사용량을 분석할 수 있어야 한다 .
   - 마이크로서비스 인스턴스가 사용하는 하드웨어 자원 사용량에 대한 메트릭을 수집하는 새 컴포넌트(모니터 서비스)를 시스템 환경에 추가한다.





