### Branch 란 무엇인가?

- Git은 데이터를 일련의 스냅샷으로 기록하는데, commit 하는 경우, Staging Area 에 있는 데이터의 스냅샷에 대한  
  포인터, 저자나커밋 메시지 같은 메타데이터, 이전 커밋에 대한 포인터 등을 포함하는 커밋 개체를 저장한다. 
  
- 최초 커밋을 제외한 나머지 커밋은 커밋포인터가 적어도 하나씩 있고, Branch 를 합친 Merge 커밋 같은 경우, 이전 커밋  
  포인터가 여러 개 있다.
  
- 파일이 3개 있는 디렉토리가 하나 있고, 이 파일을 Staging Area에 저장하고 커밋하는 경우, Git 저장소에 파일을 저장하고  
  (Blob 라고 한다고 함) Staging Area에 해당 파일의 체크섬을 저장한다.
  
![commit-and-tree](https://user-images.githubusercontent.com/50399804/106341336-3f5a2200-62e0-11eb-8561-db47fa303568.png)


 - 다시 파일을 수정하고 커밋하면 이전 커밋이 무엇인지도 저장한다. 
 
![commits-and-parents](https://user-images.githubusercontent.com/50399804/106341450-b2fc2f00-62e0-11eb-995f-86b3aa84f7dc.png)


 - Git의 Branch 는 커밋 사이를 가볍게 이동할 수 있는 어떤 포인터 같은 것으로 기본적으로 master 로 만들어진다.   
   처음 commit 하면 이 master Branch 가 생성된 커밋을 가리키고, 이후 커밋을 만들면 master Branch 는 자동으로  
   가장 마지막 커밋을 가리킨다.
   
![branch-and-history](https://user-images.githubusercontent.com/50399804/106341549-08384080-62e1-11eb-9d25-9b7816ea3112.png)


***


__새 Branch 생성하기__

 - testing 이라는 Branch 를 만들어보자.
 
```
$ git branch testing
```

![two-branches](https://user-images.githubusercontent.com/50399804/106341593-2736d280-62e1-11eb-87e0-a8aea90498d7.png)

- 지금 작업 중인 Branch 는 어떻게 파악할까, 다른 VCS 와 달리 Git 은 HEAD 라는 특수 포인터가 있다. 이 포인터가 지금 작업하는 로컬 Branch 를 가리킨다.

![head-to-master](https://user-images.githubusercontent.com/50399804/106341783-d2e02280-62e1-11eb-81cf-e301be485f07.png)

- git log --oneline --decorate 의 --decorate 옵션을 사용하면 쉽게 Branch 가 어떤 커밋을 가리키는지도 확인할 수 있다.


***


__Branch 이동하기__

- git checkout 명령으로 다른 Branch 로 이동할 수 있다.

```
$ git checkout testing
```

![head-to-testing](https://user-images.githubusercontent.com/50399804/106346197-9bc93b80-62f8-11eb-8fe5-77cefb1c4c1c.png)

- testing Branch 에서 commit 을 실시하고, master 로 checkout 을 해보자.

```
$ vim test.rb
$ git commit -a -m 'made a change'
```

![advance-testing](https://user-images.githubusercontent.com/50399804/106346255-02e6f000-62f9-11eb-9373-9a06162e0644.png)

```
$ git checkout master
```

![checkout-master](https://user-images.githubusercontent.com/50399804/106346256-037f8680-62f9-11eb-96f3-f57573c2d7bd.png)

 - 방금 실행한 명령이 한 일은 두가지다. master Branch 가 가리키는 커밋을 HEAD가 가리키게 하고, Working Directory  
   의 파일도 그 시점으로 되돌려 놓았다. 
 
 - 파일을 수정하고 다시 커밋을 해보자.
```
$ vim test.rb
$ git commit -a -m 'made other changes'
```

![advance-master](https://user-images.githubusercontent.com/50399804/106346307-830d5580-62f9-11eb-93f1-4f9a250deef6.png)

- 프로젝트 히스토리가 분리되어 진행된다. 커밋 사이를 자유롭게 이동하다가 때가 되면 두 Branch 를 Merge 해야 한다.  
  명령어를 통해 현재 Branch 가 가리키고 있는 히스토리가 무엇이고 어떻게 갈라져 있는지 볼 수 있다.
  
```
$ git log --oneline --decorate --graph -all
```

