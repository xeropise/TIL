### 태그 

 - 다른 VCS 처럼 Git 태그도 지원하는데 다음과 같이 조회할 수 있다.
 
```
$ git tag
```
 
 - Git의 태그는 Lightweight 태그와 Annotated 태그로 두 종류가 있다.
  
      1. Lightweight - Branch 와 비슷한데 가리키는 지점을 최신 commit 으로 이동시키지 않는다. 특정 커밋에 대한 포인터
      
      2. Annotated - Git 데이터베이스에 태그를 만든 사람의 이름, 이메일과 태그를 만든 날짜, 태그 메시지 저장, 서명도 사용 가능하다.
  
 - Lightweight 태그는 기본적으로 커밋 체크섬만 저장하기 때문에 -a, -s, -m 옵션을 사용하지 않는다.
 
```
$ git tag v1.4-lw
```

 - Annotated 태그를 만들려면 -a 옵션을 추가한다.
 
```
$ git tag -a v1.4 -m "my version 1.4"
```
> -m 옵션으로 태그를 저장할 때 메시지를 함께 저장할 수 있다. 명령을 실행할 때 메시지를 입력하지 않으면  
  Git은 편집기를 실행시킨다.
      
 - git show 명령으로 태그 정보와 커밋 정보를 모두 확인할 수 있다.
 
```
$ git show 
```


***


__나중에 태그하기__

 - 명령의 끝에 commit 체크섬을 명시한다. (전부 명시 안해도 된다)
 
```
$ git tag -a v1.2 <Branch 체크썸>
```


***


__태그 공유하기__

 - git push 명령은 자동으로 리모트 서버에 태그를 전송하지 않으므로, 태그를 만들었으면  
   별도로 push 해야 한다.
   
```
$ git push origin <태그 이름>
$ git push origin --tags  
```
> 아래 명령으로 리모트 서버에 없는 태그를 모두 전송할 수 있다.


***
***


### Git Alias

 - Git 명령어를 나만의 명령어로 커스텀해서 사용할 수 있다. 다음과 같이 사용한다.
 
```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

 - 외부 명령어도 커스텀해서 사용할 수 있다.
 
```
$ git config --global alias.visual '!gitk'
```
