### Git 최초 설정

 - Git을 설치하고 나면 Git의 사용환경을 적절하게 설정해 주어야 하며, 환경 설정은 한 컴퓨터에서 한번만 하면 된다.  
   설정한 내용은 Git을 업그레이드해도 유지되며 언제든지 바꿀 수 있다.
   
 - _gitconfig_ 라는 도구로 설정 내용을 확인하고 변경할 수 있다. Git 은 이 설정에 따라 동작  
   이때 사용하는 설정 파일은 세 종류 이다.
   
      1) /etc/gitconfig 파일  
      
        - 시스템의 모든 사용자와 모든 저장소에 적용되는 설정, git config --system 명령어로 이 파일을 읽고 쓸 수 있다.  
          이 파일은 시스템 전체 설정파일이기 때문에 수정하려면 시스템의 관리자 권한이 필요
          
      2) ~/.gitconfig,  ~/.config/git/config 파일
      
        - 특정 사용자(즉 현재 사용자) 에게만 적용되는 설정, git config --global 옵션으로 이 파일을 읽고 쓸 수 있다.  
          특정 사용자의 모든 저장소 설정에 적용된다.
      
      3) .git/config 파일
      
        - 이 파일은 Git 디렉토리에 있고 특정 저장소(혹은 현재 작업 중인 프로젝트)에만 적용된다. --local 옵션을 사용하면  
          이 파일을 사용하도록 지정할 수 있다.. 하지만 기본적으로 이 옵션이 적용되어 있다.  
          (당연히, 이 옵션을 적용하려면 Git 저장소인 디렉토리로 이동 한 후 적용할 수 있다.)
          
 - 각 설정은 역순으로 우선시 되어 .git/config 가 /etc/gitconfig 보다 우선한다.
   이 시스템의 설정 파일의 경로는 git config -f <file> 명령으로 변경할 수 있다. 관리자 권한이 필요
   

__사용자 정보__

- Git 을 설치하고 가장 먼저 해야 하는 것은 사용자이름과 이메일 주소를 설정하는 것이다. Git 은 커밋할 때마다 이 정보를 사용한다.  
  한번 커밋한 후에는 정보를 변경할 수 없다.
  
```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
> 다시 말하면, --global 옵션으로 설정하는 것으로 딱 한번만 하면 된다. 해당 시스템에서 해당 사용자가 사용할 때는  
  이 정보를 사용한다. 만약 프로젝트마다 다른 이름과 이메일 주소를 사용하고 싶으면 --global 옵션을 빼고,  
  명령을 실행한다. GUI 도구들은 처음 실행할 때 이 설정을 묻는다.
 
 
***


__편집기__

- 사용자 정보를 설정하고 나면 Git 에서 사용할 텍스트 편집기를 고른다. 기본적으로 Git은 시스템의 기본 편집기를 사용하나  
  하지만, 다른 텍스트 편집기를 사용할 수 있는데 다음과 같이 명령어를 실행한다.
  
```
$ git config --global core.editor  emacs
```

- Windows 사용자라면 다른 텍스트 편집기를 사용할 수 있는데, 실행파일의 전체 경로를 설정해주면 된다.

- Windows 환경에서 많이 사용되는 Notepad 편집기의 경우, 주로 32비트 버전을 사용하게 된다.  
  현재 기준으로 64비트 버전을 사용하면 동작하지 않는 플러그인이 많아 32비트 Windows 시스템이거나,  
  64비트 Windows 시스템에서 64비트 Notepad를 설치했다면 다음과 같이 설정한다.
  
```
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -nosession
```

 - 64비트 Windows 시스템에서 32비트 Notepad++를 설치했다면 다음을 사용한다.
 
```
$ git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -nosession"
```

- 실행파일을 받아 설치하는 경우, 기본 편집기를 GUI로 설치 가능하더라.. 나중에 설명


***


__설정 확인__

 - git config --list 명령을 실행하면 설정한 모든 것을 보여주어 바로 확인할 수 있다.
 
```
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

 - Git 은 같은 키를 여러 파일(/etc/gitconfig 와 ~/.gitconfig ..) 에서 읽기 때문에 같은 키가 여러 개 있을 수도 있다.  
   그러면 Git 은 나중 값을 사용한다.
  
 - git config <key> 명령으로 Git 이 특정 key에 대해 어떤 값을 사용하는지 확인 할 수 있다.
 
 ```
 $ git config user.name
John Doe
 ```
 
 
 - Git이 설정된 값을 읽을 때 여러 파일에서 동일한 키에 대해 다른 값을 설정하고 있을 수 있다.  
   값이 기대한 값과 다를 수 있는데 값만 보고 쉽게 그 원인을 찾을 수 없다.  
   이 때 키에 설정된 값이 어디에서 설정되었는지 확인할 수 있는데 다음과 같은 명령으로 어떤 파일로부터 설정된 값인지를 확인할 수 있다.
          
```
$ git config --show-origin rerere.autoUpdate
file:/home/johndoe/.gitconfig	false
```
