### Branch 와 Merge 기초

 - 보통 Branch 와 Merge 는 이런 식으로 진행한다.

      1. 웹사이트가 있고, 뭔가 작업을 진행  
      2. 새로운 이슈를 처리할 새 Branch  생성  
      3. 새로 만든 Branch에서 작업을 진행 
  
 - 이때 중요한 문제가 생겨서 그것을 해결하는 Hotfix Branch 를 만들어야 한다. 그러면 다음과 같이 한다.
 
      1. 새로운 이슈를 처리하기 이전의 운영 Branch로 이동  
      2. Hotfix Branch 새로 생성  
      3. 수정한 Hotfix 테스트를 마치고 운영 Branch로 Merge  
      4. 다시 작업하던 Branch 로 옮겨가서 하던 일 진행 
      
      
***

__Branch 기초__

 - 이슈 관리 시스템에 등록된 53번 이슈를 처리한다고 하면, 이 이슈에 집중할 Branch 를 새로 생성하자.
 
```
$ git checkout -b iss53
```
> -b 라는 옵션을 추가하면 Branch 를 만들면서 Checkout 까지 한 번에 한다.

![basic-branching-2](https://user-images.githubusercontent.com/50399804/106346596-9b7e6f80-62fb-11eb-89ba-75f629b61d27.png)

 - iss53 Branch 를 Checkout 했기 때문에, 뭔가 일을 하고 커밋하면 iss53 Branch 가 앞선다.
 
![basic-branching-3](https://user-images.githubusercontent.com/50399804/106346614-b650e400-62fb-11eb-9e4b-8b45742ea430.png)

 - 여기서 다른 상황을 가정하자, 문제가 생겨 Hotfix 를 해야 한다. iss53 Branch 와 섞이는 것을 방지하기 위해, iss53과 관련된 코드를 어딘가에  
   저장해두고 원래 운영 환경의 소스로 복구해야 한다. Git 을 사용하면 노력을 들일 필요 없이 그냥 master Branch 로 이동하면 된다.
   
```
$ git checkout master
```
> 하지만 Branch 이동 전에 아직 커밋하지 않은 파일이 Checkout 할 Branch 와 충돌 나면 변경할 수 없다.  
  변경 전, 반드시 Working Directory 를 정리하는 것이 좋으며, [Stashing, Cleaning](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Stashing%EA%B3%BC-Cleaning#_git_stashing) 으로도 해결가능하다.  
  
 - 이때 Working Directory 는 53번 이슈를 시작하기 이전 모습이므로, Hotfix Branch 를 만들고, 새로운 이슈를 해결한다.
 
```
$ git chekcout -b hotfix
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
```

![basic-branching-4](https://user-images.githubusercontent.com/50399804/106346810-17c58280-62fd-11eb-81d4-f84da3db87a6.png)

- 운영 환경에 적용하려면 문제를 제대로 고쳤는지 테스트하고, 최종적으로 운영환경에 배포하기 위해 hotfix Branch 를 master Branch 에 합쳐야 한다.
 
```
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```
> "Fast-forward" 는 Hotfix Branch 가 C4 커밋이 C2 커밋에 기반한 Branch 이기 때문에 이러한 메시지를 보여준다.

![basic-branching-5](https://user-images.githubusercontent.com/50399804/106346796-f2d10f80-62fc-11eb-9379-f436e35fef7d.png) 
 
 - 다시 iss53번으로 이동하여, 이제 일을 계속하면 된다.
 
 ```
 $ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'finished the new footer [issue 53]'
[iss53 ad82d7a] finished the new footer [issue 53]
1 file changed, 1 insertion(+)
 ```
 
 ![basic-branching-6](https://user-images.githubusercontent.com/50399804/106346850-5fe4a500-62fd-11eb-8b5d-d887899114c8.png)


***


__Merge 의 기초__


 - iss53 을 전부 구현하고 다시 master Branch 에 Merge 해보자. 앞서 살펴본 Hotfix Branch를 Merge 하는 것과 비슷하다.
 
```
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```
> 현재 Branch가 가리키고 있는 커밋이 Merge 할 Branch 의 조상이 아니므로 'Fast-forward' Merge 하지 않는다.

![basic-merging-1](https://user-images.githubusercontent.com/50399804/106346923-d1bcee80-62fd-11eb-804f-21f47697f6f8.png)


![basic-merging-2](https://user-images.githubusercontent.com/50399804/106346961-05981400-62fe-11eb-8539-e87aec42ea5a.png)

 - 단순 Branch 포인터를 최신 커밋으로 옮기는게 아니라, Merge의 결과를 별도의 커밋으로 만들고 나서 해당 Branch 가 그 커밋을 가리키도록 이동한다.  
   이런 커밋은 부모가 여러 개고 Merge 커밋이라고 부른다.
   
 - 이제 더이상 iss53 Branch 가 필요 없다, 다음 명령으로 Branch 를 삭제하고 이슈의 상태를 처리 완료로 한다.
 
```
$ git branch -d iss53
```

***


__충돌의 기초__


 - 위에서 실시한 Merge (3-way Merge)가 실패 할 경우도 있는데, Merge 하는 두 Branch에서 같은 파일의 한 부분을   
    동시에 수정하고 Merge 하면 Git은 해당 부분을 Merge 하지 못한다.
    
```
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

 - Git이 어떤 파일을 Merge 할 수 없었는지 살펴보려면 git status 명령을 이용한다.
 
```
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```
 
 - 충돌이 일어난 파일은 unmerged 상태로 표시해주며 충돌이 난 부분을 표준 형식에 따라 표시해준다. 그러면 개발자는   
   해당 부분을 수동으로 해결하면 된다.
   
```
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```
 
 - ====== 위쪽의 내용이 HEAD 버전(Merge 명령을 실행할 때 작업하던 master Branch)의 내용이고, 아래쪽은 iss53 Branch의 내용이다.   
   해결하려면 한쪽 내용을 고르거나 새로 작성하여 Merge 한다. 위 코드 문제를 해결하면 이렇다.
    
```
<div id="footer">
please contact us at email.support@github.com
</div>
```
  
 - 충돌한 양쪽에서 조금씩 가져와서 새로 수정했다. 그리고 <<<<<<<, =======, >>>>>>> 가 포함된 행을 삭제 했다. 이렇게 해결하고  
   git add 명령으로다시 Git에 저장한다.
   
 - git status 명령으로 충돌이 해결된 상태인지 다시 한번 확인해볼 수 있다.
 
```
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

    modified:   index.html
```

 
 
