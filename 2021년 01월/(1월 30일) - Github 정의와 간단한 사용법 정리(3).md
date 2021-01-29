### Git 설치


***


__Linux에 설치__

- Linux에서는 패키지로 Git을 설치할 대는 보통 각 배포판에서 사용하는 패키지 관리도구를 사용하여 설치  
  Fedora( RPM 기반 패키지 시스템을 사용하는 RHEL, CentOS) 에서는 'dnf' 명령어를 사용 한다.
  
```Linux
$ sudo dnf install git-all
```

 - Ubuntu 등의 데비안 계열 배포판에서는 'apt' 를 사용
 
 ```Linux
 $ sudo apt install git-all
 ```
> 다른 Unix 배포판에 설치하려는 경우, [여기](http://git-scm.com/download/linux)를 참조


***


__Mac에 설치__

- XcodCommand Line Tools 를 설치하는 방법이 가장 쉽다. Mavericks(10.9) 부터는 Terminal에 단지 처음으로  
  'git'을 실행하는 것으로 설치가 시작된다. 설치안되어 있는 경우, 설치 안내 해준다.
  
```Mac
$ git --version
```

- 혹은 [여기서](http://git-scm.com/download/mac) 받을 수 있다. GitHub for Mac 을 설치하는 방법도 있는데  
  이 도구에는 CLI 도구를 설치하는 옵션이 있다. 이건 [여기](http://mac.github.com)에서 내려받는다.
  
  
***


__Windows에 설치__

- 공식 배포판은 [Git 웹사이트](http://git-scm.com/download/win)에 가면 자동으로 다운로드가 시작된다.  
  > 위 프로젝트는 'Git for Windows’ 인데, Git 자체와는 다른 별도의 프로젝트다. 
  
- [Git Chocolate 패키지](https://chocolatey.org/packages/git)를 통해 자동화된 설치 방식을 이용해 볼 수도 있다.  
  > 패키지는 커뮤니티에 의해 운영되는 프로그램이라고 한다.
  
- 'GitHub Desktop' 을 설치하는 방법 이 있다. CLI 와 GUI 를 모두 설치해주는데, 설치하는 경우 Git 을 Powershell 에서  
   사용할 수 있다. 인증정보(Crendential) 캐싱과 CRLF 설정까지 잘 된다. 이런 것들은 뒤에 설명할 예정  
  [여기](https://desktop.github.com/)서 내려받는다.


***

__소스코드로 설치__

 - [공식 홈페이지 참조](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%84%A4%EC%B9%98)
