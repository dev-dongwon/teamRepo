# GIT

## GIT이란?

분산 버젼 관리 시스템(DVCS(Distributed Virsion Control System))으로 프로그램 및 소스코드 관리를 위한 시스템이다.





## GIT 기초

### GIT의 세 가지 상태

GIT은 파일을 세 가지 상태(Committed, Modified, Staged)로 관리한다. 

- Committed : 데이터가 로컬 디렉토리에 안전하게 저장된 상태.
- Modified : 수정한 파일을 로컬 디렉토리에 Commit하지 않은 상태
- Staged : 수정한 파일을 곧 Commit할 것임을 표시한 상태

![areas](https://git-scm.com/book/en/v2/images/areas.png)

### GIT 디렉토리

Git이 파일들을 저장하는 곳.

### Working Tree (Working Directory)

프로젝트의 특정 버전을 checkout 한 것.

### Staging Area

Git 디렉토리에 있는 파일로 곧 commit할 파일에 대한 정보를 저장. Git에서는 Index라는 용어를 사용한다.

### Git이 하는 기본적인 일

- 워킹 트리에서 파일을 수정
- Staging Area에 commit할 파일의 스냅샷을 만든다.
- Staging Area에 있는 파일들을 commit해서 Git directory에 영구적 스냅샷으로 저장.

### GIT Repository 만들기

- git init : 버전관리를 하고 있지 않는 로컬 디렉토리로 이동하여 git init 명령어를 사용하면 .git 디렉토리를 생성하면서 버전 관리를 시작한다. 
- git clone : 기존에 존재하는 repository를 로컬 디렉토리로 받아온다.

### GIT 수정 후 저장하기

- 워킹 디렉토리의 모든 파일은 tracked 와 untracked 파일로 관리된다. tracked는 이미 한번 스냅샷으로 남겨졌던 파일이다. tracked 파일은 modified와 unmodified, 그리고 staged로  관리한다. modified는 commit 이후에  수정 된 파일이고 이 파일을 add 하면 staged가 된다. ![lifecycle](https://git-scm.com/book/en/v2/images/lifecycle.png)

### 파일 무시하기

GIT이 관리할 필요가 없는 파일은 .gitignore 파일을 만들고 무시할 패턴을 적으면 된다.

```console
$ cat .gitignore
*.[oa]
*~
```

```
# 확장자가 .a인 파일 무시
*.a

# 윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
!lib.a

# 현재 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않음
/TODO

# build/ 디렉토리에 있는 모든 파일은 무시
build/

# doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
doc/*.txt

# doc 디렉토리 아래의 모든 .pdf 파일을 무시
doc/**/*.pdf
```

### 변경 내용 확인하기

변경 여부 확인이 아니라 변경된 내용을 보고 싶다면 git diff를 사용한다. 그러면 수정된 파일 중 unstaged 상태인 파일의 변경 내용을 보여준다.  staged file의 변경내용을 보고 싶다면 git diff —staged 를 사용한다.

### Staging Area 생략하기

add 를 안하고 바로 commit 하고 싶으면 git commit -a -m "commit message" 를 입력하면 된다. 그러면 tracked file 중 modified 된 파일을 commit 하게 된다.

### 파일 삭제하기

git에서 파일을 삭제하기 위해선 git rm 을 사용하여 staging Area에서 삭제한 후에 commit한다. 이 명령어는 working directory에서도 파일을 삭제한다. Git에서는 삭제하고 working directory에는 남겨두려면 git rm —cached 옵션을 사용한다. 파일을 수정했다면 -f 옵션을 주어서 강제로 삭제해야한다. 

### 파일 이름 변경하기

git에서 파일이름을 변경하기 위해서는 git mv를 사용한다. git mv는 git rm/git add를 순차적으로 수행한 것과 같다.

### Commit history 조회

git log를 사용하는데 유용한 옵션들이 있다.

- -p : commit의 diff 결과를 보여준다
- -2 : 최근 2개의 결과만 보여준다
- oneline : 각 commit을 한 줄에 보여준다.
- —graph : 브랜치와 머지 히스토리를 아스키 그래프로 보여준다.
- —since or —until : 명시된 날짜의 commit만 보여준다.

### Commit 덮어쓰기

잘못된 commit을 덮어쓰는 방법은 commit —amend 를 사용하면 된다.

```console
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

### 파일을 unstated 로 돌리기

Git reset HEAD <File>로 unstaged로 돌릴 수 있다.

### 리모트 저장소 관리하기

Git remote -v 로 현재 프로젝트에 등록된 리모트 저장소를 확인할 수 있다. 저장소를 clone하면 origin 이 자동으로 등록된다. (Push)는 주어진 권한을 의미한다.

```console
$ cd grit
$ git remote -v
bakkdoor  https://github.com/bakkdoor/grit (fetch)
bakkdoor  https://github.com/bakkdoor/grit (push)
cho45     https://github.com/cho45/grit (fetch)
cho45     https://github.com/cho45/grit (push)
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
```

리모트 저장소를 추가하기 위해선 git remote add <별명> <주소>를 입력하면 된다.

git fetch(pull) <remote 별명> 을 하면 모든 브랜치를 로컬에 접근할 수 있다.

git push <리모트 저장소 이름> <리모트 저장소 브랜치> 를 하면 푸쉬할 수 있다.

### git tag annotate

```console
$ git tag -a v1.4 -m "my version 1.4"
$ git tag
v0.1
v1.3
v1.4
```

### git alias

git config 를 사용하여 alias를 지정할 수 있다.

```console
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

외부 명령어는 !를 추가하여 사용할 수 있다.

```console
$ git config --global alias.visual '!gitk'
```





## Git Branch

Git의 Branch는 각 Commit을 오고 갈 수 있는 포인터같은 역할을 한다. 기본적으로 git은 master branch를 만든다. commit을 하게 되면 master branch는 가장 마지막의 commit을 가리키게 된다. 

![branch-and-history](https://git-scm.com/book/en/v2/images/branch-and-history.png)

새로운 브랜치를 만들게 되면 다음과 같이 된다.

```console
$ git branch testing
```

![two-branches](https://git-scm.com/book/en/v2/images/two-branches.png)

새로 만든 브랜치도 마지막 commit을 가리킨다.

GIT은 HEAD라는 포인터를 통해서 작업중인 로컬 브랜치를 가리킨다.

![head-to-master](https://git-scm.com/book/en/v2/images/head-to-master.png)

git checkout testing을 하게 되면 HEAD는 testing branch를 가리키게 되며 브랜치를 이동한다.

![head-to-testing](https://git-scm.com/book/en/v2/images/head-to-testing.png)

이 상태에서 새로운 commit을 하게 되면 다음과 같이 된다.

![advance-testing](https://git-scm.com/book/en/v2/images/advance-testing.png)

다시 master로 head를 옮기면

```console
$ git checkout master
```

![checkout-master](https://git-scm.com/book/en/v2/images/checkout-master.png)

그리고 다시 커밋을 하게 되면 다음과 같이 된다.

![advance-master](https://git-scm.com/book/en/v2/images/advance-master.png)

두 커밋 사이를 이동하다가 필요시 merge를 하게 된다.

### GIT Rebase 하기

Git에서 두 브랜치를 합치는 방법은 두가지가 있다. 하나는 merge를 하는 것이고 두번째는 rebase를 하는 것이다. ![basic-rebase-1](https://git-scm.com/book/en/v2/images/basic-rebase-1.png)

Master branch 와 experiment branch를 merge하게 되면 3-way merge를 하게 된다. 그리고 새로운 commit C5를 만든다.![basic-rebase-2](https://git-scm.com/book/en/v2/images/basic-rebase-2.png)

이를 one way로 하기 위해 rebase를 사용하기도 한다.

```console
$ git checkout experiment
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command
```

이러면 일단 두 브랜치가 나눠지기 전인 공통 commit(C2)까지 가서 experiment branch까지의 diff를 차례로 만들어 임시로 저장한다.  experiment브랜치가 master가 가리키는 commit(C2)를 가리키게 하고 저장해둔 diff를 적용한다. ![basic-rebase-3](https://git-scm.com/book/en/v2/images/basic-rebase-3.png)

Master branch를 fast-forward시킨다.

```console
$ git checkout master
$ git merge experiment
```

![basic-rebase-4](https://git-scm.com/book/en/v2/images/basic-rebase-4.png)

