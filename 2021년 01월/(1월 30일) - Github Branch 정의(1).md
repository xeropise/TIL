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


   
