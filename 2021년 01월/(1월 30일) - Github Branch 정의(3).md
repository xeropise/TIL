### Branch 관리

 - git branch 명령은 단순히 Branch 를 만들고 삭제하는 것이 아니다, 아무런 옵션없이 실행하면 Branch 목록을 보여준다.
 
```
$ git branch
  iss53
* master
  testing
```
> * 기호가 붙어 있는 master Branch 는 현재 Checkout 해서 작업하는 Branch 를 나타낸다.

 - git branch -v 옵션을 붙여 실행하면 마지막 커밋 메시지도 함께 보여준다.
 
```
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```
 
 - 각 Branch 가 어떤 상태인지 확인하기에 좋은 옵션도 있다. 필터링 가능
 
```
$ git branch --merged
  iss53
* master
```

 - Merge 하지 않은 Branch 를 강제로 삭제하려면 -D 옵션으로 삭제한다.
 
```
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.
```


***

> 여기부터는 공홈 주소를 태그하겠다. 설명이 더 잘되어 있다.

__Remote Branch__

 [리모트 저장소](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EB%B8%8C%EB%9E%9C%EC%B9%98)
 
*** 

__Rebase 하기__

 [Rebase](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)
 
 
***

__Git 서버 - 프로토콜__

 [프로토콜](https://git-scm.com/book/ko/v2/Git-%EC%84%9C%EB%B2%84-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
 

***

__Git 서버 - SSH 공개키 만들기__

 (SSH_KEYGEN)[https://git-scm.com/book/ko/v2/Git-%EC%84%9C%EB%B2%84-SSH-%EA%B3%B5%EA%B0%9C%ED%82%A4-%EB%A7%8C%EB%93%A4%EA%B8%B0


 
