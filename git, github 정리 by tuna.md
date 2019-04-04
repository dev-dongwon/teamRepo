# GIT과 GITHUB

## 특징

### SVN과는 많이 다르다!
- 개별 파일의 관리가 아닌 전체 프로젝트의 스냅샷 개념
-  대부분의 명령이 로컬 기반 -> 빠르다

### 3가지 파일 상태
- comitted : 데이터가 로컬 데이터베이스에 저장
- modified : 수정한 파일을 커밋하지 않음
- staged : 수정한 파일을 커밋할 거라고 표시

### GIT에는 3가지 공간이 있다
- Working directory
	- 프로젝트의 특정 버전을 **checkout** 한 것
	- 	.git 에서 압축된 데이터베이스에서 파일을 가져와 워킹 트리를 만듦
- Staging area
	- .git 디렉토리 안에 존재
	- 곧 커밋할 파일에 대한 정보를 저장
- .git directory
	- 프로젝트의 **메타 데이터**와 **객체 데이터베이스** 저장
	- 저장소를 clone 하거나 git init 명령으로 만들어진다

### 기본적인 작업 흐름
1. 워킹 트리에서 파일을 수정
2. Staging Area 에 파일을 stage해서 커밋할 스냅샷을 만든다
3. Staging Area에 있는 파일들을 커밋, git 디렉토리에 영구적인 스냅샷으로 저장

## 설치 및 설정
### 설치
```bash
$ sudo apt-get install git-all
```
### 기본 설정 파일

1. /etc/gitconfig : 시스템의 모든 사용자와 모든 저장소에 적용되는 설정
```bash
$ git config --system
```
2. ~/.gitconfig, ~/.config/git/config 파일: 특정 사용자에게만 적용되는 설정
```bash
$ git config --global
```
3. .git/config : .git 디렉토리에 있고 특정 저장소(혹은 현재 작업 중인 프로젝트)에만 적용  

각 설정은 역순으로 우선시 된다

### 최초 설정
1. 사용자 설정
```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
2. 편집기 설정
```bash
$ git config --global core.editor Editor
```
3. 설정 확인
```bash
$ git config --list // 설정 리스트 보기
$ git config <key>

예를 들어 $ git config user.name 
// dongwon
```

4. 도움말 확인
```bash
$ git help config
```
## Git 기초
### git 저장소 만들기
- 기존 디렉토리를 git 저장소로 만들기
	- 프로젝트 디렉토리로 이동 후
		```
		$ git init
		```
	- .git이라는 하위 디렉토리가 만들어진다
	- 만들어졌다고 해서 바로 관리가 되는 것은 아니다. 아직 아무 파일도 관리하지 않는 상태다
	- 파일 관리를 하려면 ```$ git add``` 로 추가하고 ```$ git commit``` 으로 저장소에 파일을 추가하고 커밋해야 한다

- 기존 저장소 clone 하기
	```bash
	$ git clone [저장소 url] [저장할 이름(optional)]
### 기본적인 동작 흐름
>모든 파일은 **Tracked(관리 대상)**, **Untracked(관리 대상 아님)**, 둘 중 하나다
	- **Untracked** 파일은 스냅샷과 스테이지 둘 다 포함되지 않은 파일이다
	- **Tracked** 파일은 스냅샷에 이미 포함된 파일이다
	- **Tracked** 파일은 다시 세 가지 상태로 나눠진다
			1. unmodified(수정하지 않음)
			2. Modified(수정함)
			3. Staged (커밋으로 저장소에 기록할)

만약 내가 어떤 파일을 수정하면
1. GIT은 그 파일을 Modified 상태로 인식한다
2. 커밋을 하기 위해 수정 파일을 staged 파일로 만들고
3. Stage 파일을 커밋한다
4. 계속 반복



### 파일 상태 확인
```bash
$ git status
```
### 만약 Staged 상태의 파일을 한 번 더 수정하면 어떻게 될까?
```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.

Changes to be committed: (use "git reset HEAD ..." to unstage)
new file: README
modified: CONTRIBUTING.md 

Changes not staged for commit:
(use "git add ..." to update what will be committed)
(use "git checkout -- ..." to discard changes in working directory) 

modified: CONTRIBUTING.md
```

Staged 면서 동시에 unstaged가 된다.
이 상태에서 commit을 하면 마지막 staged 파일만 커밋 되겠지?

```$ git add``` 후에 파일을 수정한다면,
한 번 더 ``` $ git add ``` 명령을 실행해서 최신 버전을 staged 상태로 만든 후 commit 해야 한다

### 파일 무시하기
- 로그나 파일 시스템 상태 등은 관리할 필요가 없을 것이다. .gitignore 파일에 이런 파일들을 등록하자
- .gitignore 파일을 만든 후, 그 안에 무시할 파일 패턴을 적는다
	~~~bash
	$ cat .gitignore
	*.[oa]
	*~
	~~~ 
- 다음과 같이 패턴을 적용할 수 있다
	```bash
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


출처 : [Pro Git BOOK](https://git-scm.com/book/ko/v2)
