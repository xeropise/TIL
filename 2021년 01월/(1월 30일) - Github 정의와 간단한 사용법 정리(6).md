### 수정하고 저장소에 저장하기

- Git 저장소를 하나 만들었고, Working Directory 에 Checkout 한 상태, 이제는 파일을 수정하고,  
  파일의 스냅샷을 커밋해 보자. 파일을 수정하다가 저장하고 싶으면 스냅샷을 커밋한다.
  
- Working Directory의 모든 파일은 크게 Tracked (관리대상) 와 Untracked (관리대상아님) 으로 나뉜다.

- Untracked 파일은 Working Directory에 있는 파일 중 스냅샷에도 Staging Area 에도 포함되지 않은 파일이다.

- Tracked 파일은 이미 스냅샷에 포함되어 있는 파일로 또 Unmodified(수정하지 않음), Modified(수정함),  
  그리고 Staged(커밋으로 저장소에 기록됨) 3개의 상태로 나뉜다.   
  처음 저장소를 Clone 하면 모든 파일은 Tracked 이면서 Unmodified 상태이다. Checkout 하고 나서 아무것도  
  수정하지 않았기 때문에 그렇다.
  
- 마지막 commit 이후, 아직 아무것도 수정하지 않은 상태에서 어떤 파일을 수정하면 Git 은 그 파일을 Modified  
  상태로 인식한다. 
  
  
![lifecycle](https://user-images.githubusercontent.com/50399804/106302763-b7065d80-629c-11eb-80d6-e603a9f61e1c.png)  
> 이런 라이프 사이클을 계속 반복한다.  


***


__파일의 상태 확인하기__

- 파일의 상태를 확인하려면 git status 명령을 사용한다. Clone 한 후에 바로 이 명령을 실행하면 이렇다.

```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

- 파일은 하나도 수정하지 않았으며 Tracked 파일은 하나도 수정되지 않았다. 현재 작업 중인 Branch 를 알려주며,  
  서버의 같은 Branch 로부터 진행된 작없이 없는 것을 나타낸다. 기본 Branch 가 master 이기 때문에 현재 Branch  
  이름이 master 로 나온다. 
  
  > [Branch 란?](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80#ch03-git-branching)
  

- README 파일을 만든 후, git status 를 실행해 보자. 리눅스 명령어 "echo '파일내용' > 파일명" 파일을 생성할 수 있다. 

```
$ echo 'My Project' > README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    README

nothing added to commit but untracked files present (use "git add" to track)
```
> 새로 만든 파일이기 때문에, Untracked files 가 있다.  
  Git은 Tracked 상태가 되기 전까지는 Git은 절대 그 파일을 커밋하지 않는다.
  
  
***


__파일을 새로 추적하기(Tracked 상태로)__

- git add 명령으로 파일을 새로 추적(tracked) 할 수 있다. 추적하고 상태를 확인해 보자.
  > git add . 명령을 사용하면 모든 파일을 추적 상태로 바꿀 수 있다.
  
```
$ git add README
```

```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
```

- “Changes to be committed” 에 들어 있는 파일은 Staged 상태라는 것을 의미한다. 이후 커밋하면  
  git add 를 실행한 시점의 파일이 커밋되어 저장소 히스토리에 남는다.


***


__변경사항 커밋하기__

 - 수정한 것을 커밋하기 위해 Staging Area에 파일을 정리했고, Unstaged 상태의 파일은 커밋되지 않는다.  
   Git 은 생성하거나 수정하고 나서 git add 명령으로 추가하지 않은 팡리은 커밋하지 않는다. 그 파일은 여전히  
   Modified 상태로 남아 있다. git commit 을 실행하여 커밋한다.
  
```
$ git commit
```

 - Git 설정에 지정된 편집기가 실행되고, 아래와 같은 텍스트가 자동으로 포함된다.
 
```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Your branch is up-to-date with 'origin/master'.
#
# Changes to be committed:
#	new file:   README
#	modified:   CONTRIBUTING.md
#
~
~
~
".git/COMMIT_EDITMSG" 9L, 283C
```
> 자동으로 생성되는 커밋 메시지의 첫 라인은 비어 있고, 둘째 라인부터 git status 명령의 결과가  
  채워진다. 커밋한 내용을 쉽게 기억할 수 있도록 메시지를 포함할 수도 있고, 전부 지우고 새로 작성 가능
  
 - 정확히 뭘 수정했는지 확인하려면 -v 옵션을 추가하면 편집기에 diff 메시지가 추가된다.
 
```
$ git commit -v
```

 - 혹은 메시지를 인라인으로 첨부할 수도 있다. -m 옵션을 사용한다.
 
```
git commit -m
```

```
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```
 - commit 명령은 몇 가지 정보를 출력하는데, 위 예제에서는 master Branch 에 커밋했고, 체크섬은 (463dc4f) 라고   
   알려준다. 그리고 수정한 파일이 몇개 이고 삭제됐거나 추가된 라인이 몇 라인인지 알려준다.

 - Git은 Staging Area에 속한 스냅샷을 커밋한다는 것을 기억해야 한다. 수정은 했지만, 아직 Staging Area에  
   넣지 않은 것은 다음에 커밋할 수 있다. 커밋할 때마다 프로젝트의 스냅샷을 기록하기 때문에 나중에 스냅샷끼리 비교하거나 예전 스냅샷으로 되돌릴 수 있다.
   
 - Staging Area 는 커밋한 파일을 정리한다는 점에서 매우 유용하지만, 필요하지 않은 경우도 있다.  
   git commit 시 -a 옵션ㅇ르 추가하면 Tracked 상태의 파일을 자동으로 Staging Area 에 넣는다.
   
``` 
git commit -a
```

 - 이외에 다양한 사용법은 도움말 git help commit 참조
   

***

__파일 삭제하기__

 - Git에서 파일을 제거하려면 git rm 파일명 명령으로 Tracked 상태의 파일을 삭제한 후에 커밋해야 한다.
 
 - rm 파일명 명령을 사용하는 경우 Working Directory 에서도 파일을 삭제한다.
 
```
$ git rm README
$ rm README
```

 - 이미 파일을 수정했거나 Staging Area 에 추가했다면 -f 옵션을 주어 강제로 삭제해야 한다.  
   이 점은 실수로 데이터를 삭제하지 못하도록 하는 안전장치이다.
 
```
git rm README -f
```

 - Staging Area 에만 제거하고 Working Directory 에 있는 파일은 지우고 않고 남겨둘 수 있다. 
   다시 말해서 하드디스크에 있는 파일은 그대로 두고 Git만 추적하지 않게 한다.  
   이것은 .gitignore 파일에 추가하는 것을 빼먹었거나 대용량 로그 파일이나 컴파일된 파일인 .a 파일 같은 것을 실수로 추가했을 때 쓴다.  
   --cached 옵션을 사용하여 명령을 실행한다.
   
```
$ git rm --cached README
$ git rm \*~
```

 - 패턴을 넣어 여러 파일을 한번에 삭제할 수도 있다.
