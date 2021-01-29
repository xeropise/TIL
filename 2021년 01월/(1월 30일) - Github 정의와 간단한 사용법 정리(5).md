### 도움말 보기

- 명령어에 대한 도움말이 필요한 경우, 두가지 방법으로 볼 수 있다.

```
$ git help <verb>
$ man git-<verb>
```

- 아래와 같이 실행하는 경우 git config 명령에 대한 도움말을 볼 수 있다.

```
git help config
```

 - Git 명령을 사용하기 위해 도움말 전체를 볼 필요 없이, 각 명령에서 사용할 수 있는 옵션들에 대해서 간략히  
   살펴볼수도 있다. -h, --help 옵션을 사용하면 Git 명령에서 사용할 수 있는 옵션들에 대한 간단한 도움말을 출력한다.
   
```
$ git add -h
usage: git add [<options>] [--] <pathspec>...

    -n, --dry-run         dry run
    -v, --verbose         be verbose

    -i, --interactive     interactive picking
    -p, --patch           select hunks interactively
    -e, --edit            edit current diff and apply
    -f, --force           allow adding otherwise ignored files
    -u, --update          update tracked files
    -N, --intent-to-add   record only the fact that the path will be added later
    -A, --all             add changes from all tracked and untracked files
    --ignore-removal      ignore paths removed in the working tree (same as --no-all)
    --refresh             don't add, only refresh the index
    --ignore-errors       just skip files which cannot be added because of errors
    --ignore-missing      check if - even missing - files are ignored in dry run
    --chmod <(+/-)x>      override the executable bit of the listed files
```


***


### Git 저장소 만들기

- 보통 2가지 방법으로 Git 저장소를 쓰기 시작한다. 

    1. 아직 버전관리를 하지 않는 로컬 디렉토리를 하나 선택해서 Git 저장소로 사용
    
    2. 다른 어딘가에서 Git 저장소를 Clone 하는 방법
    
***

__기존 디렉토리를 Git 저장소로 만들기__

Linux:
```
$ cd /home/user/my_project
```
Mac:
```
$ cd /Users/user/my_project
```
Windows:
```
$ cd /c/user/my_project
```
그리고 아래와 같은 명령을 실행한다:
```
$ git init
```

 - git init 명령은 .git 이라는 하위 디렉토리를 만들며 저장소에 필요한 뼈대(Skeleton) 파일이 들어 있다.  
   이 명령만으로는 아직 프로젝트의 어떤 파일도 관리하지 않는다. 
  
***

__기존 저장소를 Clone 하기__ 

 - 다른 프로젝트에 참여하거나 Git 저장소를 복사하고 싶을 때, git clone <url> 명령을 사용한다.  

 - Git 이 Subversion 과 다른 가장 큰 차이점은 서버에 있는 거의 모든 데이터를 복사한다는 것인데,  
   프로젝트 히스토리를 전부 받아온다. 서버의 디스크가 망가져도 클라이언트 저장소 중에서 아무거나  
   가져다 복구할 수 있다. (다만, 서버에 적용했던 설정은 복구하지 못한다)
   
```
$ git clone https://github.com/libgit2/libgit2
```
> 위 명령은 libgit2 라는 디렉토리를 만들고 그 안에 .git 디렉토리를 만든다. 그리고 저장소의 데이터를  
  모두 가져와서 자동으로 가장 최신 버전을 Checkout 해 놓는다.

```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```
> 위 명령으로 하는 경우 mylibgit 이라는 디렉토리가 생성되는 것만 제외하면, 모든 결과가 앞과 같다.

 - Git은 다양한 프로토콜을 지원하는데 https://, git:// , user@server:path/to/repo.git 처럼 [SSH 프로토콜](http://www.ktword.co.kr/abbr_view.php?m_temp1=2524)을 사용할 수도 있다.
 
 
