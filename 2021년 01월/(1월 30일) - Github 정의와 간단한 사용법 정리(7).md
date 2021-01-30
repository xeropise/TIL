### commit 히스토리 조회하기

- 새로 저장소를 만들어서 몇 번 commit  했을 수도 있고, commit 히스토리가 있는 저장소를 Clone 했을 수도 있다.

- 가끔 저장소의 히스토리를 보고 싶을 때가 있다. 이 때 사용하는 명령어인 git log 가 있다.

```
$ git clone https://github.com/schacon/simplegit-progit
```
> 위 예제는 Git 에서 제공 

![캡처](https://user-images.githubusercontent.com/50399804/106336489-5c87f400-62d2-11eb-9da4-8607ec32df9a.JPG)

 - 원하는 히스토리를 검색할 수 있도록 매우 다양한 옵션을 지원하는데 다음과 같다 
 
```
$ git log -p -2
```
> 각 commit 의 diff 결과를 보여주고 최근 두 개의 결과만 보여준다.

```
$ git log --stat
```
> 각 commit 의 통계 정보를 조회할 수 있다.

```
git log --pretty=oneline
git log --pretty=short
git log --pretty=full
git log --pretty=fuller
```
> 히스토리 내용을 보여줄 때 기본 형식 이외에 여러 가지 중에 하나를 선택할 수 있다.

```


$ git log --pretty=format:"%h - %an, %ar : %s"
```
> 원하는 포맷 형태로 볼 수도 있다. 유용한 옵션들은 [여기](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%BB%A4%EB%B0%8B-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0#pretty_format)

> [주요 옵션](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%BB%A4%EB%B0%8B-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0#log_options)도 다양하다.


***


### 되돌리기

- 종종 완료한 commit 을 수정해야 할 때가 있다. 너무 일찍 commit 했거나 어떤 파일을 빼먹었을 때, 그리고 commit 메시지를 잘못 적었을 때 한다.  
  다시 commit 하려는 경우, 수정 작업을 하고 Staging Area 에 추가한 다음 --amend 옵션을 사용하여 commit 을 재작성 할 수 있다.
  
```
$ git commit -amend
```

 - commit 을 했는데 Stage 하는 것을 깜빡하고 빠트린 파일이 있으면 아래와 같이 고칠 수 있다.
 
```
$ git commit -m 'commit 메시지'
$ git add forgoteen_file
$git commit --amend
```
> commit 한 개로 기록된다.


***


__파일 상태를 Unstage 로 변경하기__

 - git reset HEAD <file>.. 명령어를 사용한다. 자세한 것은 [여기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0#_git_reset)에서 파악
 
 
***

__Modified 파일 되돌리기__

 - 다시 check하는 git checkout -- 파일명 을 사용하여, 변화한 이력을 날릴 수 있다.

 - 이외의 방법들은 나중에 설명 예정


***


### 원격 저장소 (Remote repository) 

 - git remote 명령으로 현재 프로젝트에 등록된 리모트 저장소를 확인할 수 있다.
 
![캡처](https://user-images.githubusercontent.com/50399804/106338580-74ae4200-62d7-11eb-9d5c-2d49a5745b9f.JPG)


***

__리모트 저장소 추가하기__

 - 원격 저장소를 clone 으로 불러오는게 아니라, 다음과 같이 직접 추가할 수도 있다. 
 
![캡처](https://user-images.githubusercontent.com/50399804/106338711-db336000-62d7-11eb-9bbc-dfddd9f97a4c.JPG)

![캡처](https://user-images.githubusercontent.com/50399804/106338894-4f6e0380-62d8-11eb-842a-41503a8f233f.JPG)


***

__리모트저장소를 Pull 하거나 Fetch 하기

 - 앞서 설명했듯이 리모트 저장소에서 데이터를 가져오려면 다음과 같이 실행한다
 
 ```
 $ git fetch <remote>
 ```
 
 - 이 명령은 로컬에는 없지만, 리모트 저장소에 있는 데이터를 모두 가져온다. 리모트 저장소의 모든 Branch 를  
   로컬에서 접근할 수 있어서, 언제든지 Merge 하거나 내용을 살펴볼 수 있다. 
   
 - 저장소를 Clone 하면 명령은 자동으로 'origin' 이라는 이름의 저장소를 추가한다.
 
 - git pull 명령은 fetch 하는 동시에 Merge 한다.
 
 
***
 
 __리모트 저장소에 Push 하기__
 
 - 프로젝트를 공유하고 싶을 때 사용한다. 'git push <remote 저장소 이름> <Branch 이름>' 으로 사용한다.
  
```
git push origin master
```

 - Push 하려는 경우, 다른사람이 Push 한 후에 하려면 진행되지 않으므로, 다른 사람이 작업한 것을 Merge 한 다음에  
   Push 할 수 있다.
   

***


__리모트 저장소 살펴보기__

 - 다음의 명령어로 확인 가능하다.
 
```
git remote show <Remote 저장소 이름>
```

![캡처](https://user-images.githubusercontent.com/50399804/106339359-ab855780-62d9-11eb-9fd7-c190824aa227.JPG)


 - 저장소 이름을 바꾸거나 삭제할 수도 있따.
 
```
git remote rename pb paul
git remote remove paul
```
