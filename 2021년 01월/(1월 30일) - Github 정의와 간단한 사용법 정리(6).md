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
