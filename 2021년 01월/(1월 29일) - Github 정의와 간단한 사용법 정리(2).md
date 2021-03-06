### Git 기초, Git의 핵심

- Subversion 이나 Perforce 같은 다른 VCS 와는 조금 차이가 있어, 사용자 인터페이스는 매우 비슷하나  
  정보를 취급하는 방식이 다르다.
  
***

__차이가 아니라 스냅샷__

- Subversion과 비슷한 VCS 와 Git의 가장 큰 차이점은 데이터를 다루는 방법에 있다.

- 보통 위에서 언급한 VCS 들은 각 파일의 변화를 시간순으로 관리하면서 파일들의 집합을 관리한다.  
  (델타 기반 버전관리 시스템 이라고 한다네요)
  
![deltas](https://user-images.githubusercontent.com/50399804/106287094-c4661c80-6289-11eb-9db2-d479b500905b.png)
 > 각 파일에 대한 변화를 저장하는 시스템들
 
- Git은 이런식으로 데이터를 저장하지도 취급하지 않고, 데이터를 파일 시스템 스냅샷의 연속으로 취급하고 크기가 아주 작다.

- Git은 Commit 하거나 프로젝트의 상태를 저장할 대마다 파일이 존재하는 그 순간을 중요하게 여긴다. 파일이 달라지지 않았으면,  
  Git은 성능을 위해 파일을 새로 저장하지 않는다. 단지 이전 상태의 파일에 대한 링크만 저장한다. 데이터를 스냅샷의 스트림처럼 취급
  
![snapshots](https://user-images.githubusercontent.com/50399804/106287608-51a97100-628a-11eb-91dd-bed973475e8d.png)


***


__모든 명령을 로컬에서 실행__

- 거의 모든 명령이 로컬 파일과 데이터만 사용하기 떄문에 네트워크에 있는 다른 컴퓨터는 필요 없다.  
  프로젝트의 모든 히스토리가 로컬 디스크에 있기 때문에 모든 명령이 순식간에 실행된다.
  
- 프로젝트 히스토리를 조회할 때 서버 없이 조회한다.(로컬 데이터베이스에 저장하기 때문) 리모트에 접근할 필요 없음

- 오프라인 상태이거나 VPN에 연결하지 않아도 막힘 없이 일을 할 수 있다. 비행기나 기차 등에서 작업하고, 네트워크에  
  접속하고 있지 않아도 커밋할 수 있다.
  
  
__Git의 무결성__  

- Git은 데이터를 저장하기 전에 항상 체크섬을 구하고, 그 체크섬으로 데이터를 관리한다.  
  그래서 체크섬을 이해하는 Git 없이는 어떠한 파일이나 디렉토리도 변경할 수 없다.  
  SHA-1 해시를 사용하여 만드는데, 40자 길이의 16진수 문자열이다.  
    
      24b9da6552252987aa493b52f8696cd6d3b00373
      
  Git은 모든 것을 해시로 식별하기 때문에 이런 값은 여기저기서 보인다. 실제로 Git은 파일의  
  이름으로 저장하지 않고 해당 파일의 해시로 저장한다.  
  
  > [체크섬이란?](https://galid1.tistory.com/310)  
  

__Git은 데이터를 추가할 뿐__

 - Git으로 무얼 하든 Git 데이터베이스에 데이터가 추가 된다. 되돌리거나 데이터를 삭제할 방법이 없다.  
   다른 VCS 처럼 Git 도 커밋하지 않으면, 변경사항을 잃어버릴 수 있다. 하지만, 일단 스냅샷을 커밋하고 나면  
   데이터를 잃어버리기 어렵다.
   
 
__세 가지 상태__
  
 - Git 을 알기 위해, __반드시__ 알아야 하는 부분이다. Git은 파일을 세가지 상태로 관리하는데   
   Committed, Modified, Staged 세 가지 상태로 관리한다.
   
   - Committed 란 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미
   
   - Modified 란 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것을 말한다.
   
   - Staged 란 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태를 의미한다.
   
 - 이 세가지 상태는 세가지 단계와 연결되어 있는데,  Staging Area, Working Directory, Git.Directory (Repository)  
  이렇게 세단계가 있다.  
   
![areas](https://user-images.githubusercontent.com/50399804/106289624-d5645d00-628c-11eb-9108-b863b02f1c4f.png)  


   - Working Directory 는 프로젝트의 특정 버전을 Checkout 한 것이다.

   - Staging Area는 .git Directory 안에 있는데, 단순한 파일이며 곧 커밋할 파일에 대한 정보를 저장한다.  
     기술용어로는 Index 라고 한다. 
      
   - .git Directory 는 Git 이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳을 말한다.  
      다른 컴퓨터에 있는 저장소(리모트) 를 Clone 할 때 .git Directory 가 만들어진다. 
    
