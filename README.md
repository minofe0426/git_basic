## 지옥에서 온 관리자 Git
> CVCS 중앙버전관리   
DVCS 분산버전관리(Git)   
SCM 형상관리- 변경사항을 체계적ㅇ츠로 추적, 통제하는 것

### 기본 명령어
```bash
clear // 현재 작성하고 있는 cmd화면을 깔금하게 정리

$git init // 생성
$git status // 상태확인
$git add . // 추가
$git commit -m '내용' //커밋
```

### 설정
```bash
$git --list // 저장소 별 설정 정보조회
$git config --global --list //전역 설정 정보 조회
$git config --global user.name “이름” //이름 등록
$git config --global user.email "이메일” //이메일 등록
```

### 커밋취소(reset)
> reset에는 soft, mixed, hard 3가지가 있다.   
- soft - log만 날리는 것, commit log만 날린다.   
(commit 전으로 돌림)
- mixed - 작업영역 add . 전으로 날리는 것   
(거의 쓸일이 없음)(commit과 add전으로? 돌림)
- hard - 해당 기록을 모두 삭제해버리는 것   
(선택한 작업영역 다 삭제)

```bash
$git reset --soft 변경할 로그 앞자리 4글자 이상
$git reset --mixed 변경할 로그 앞자리 4글자 이상
$git reset --hard 삭제할 로그 앞자리 4글자 이상
```

### 로그 기록에서 삭제내역 살리기
> git reflog - 내가 한번이라도 log한 기록이 다 나온다.
```bash
1. $git reflog //로그 기록에서 복구하고 싶은 로그번호를 찾는다.
2. $git reset --hard 로그번호 //복구할 로그 번호를 입력하면 삭제되었던 작업내역이 살아난다.
```

### amend
> 최종 로그 기록의 commmit을 다시 되돌려서 commmit을 초기화 해준다.
```bash
$git --amend -m '내용' // git reset --soft와 비슷한 기능
```

### fast-forward merge와 three-way-merge
- fast-forward merge 형상이 똑같을 때   
 (기존 브랜치에 수정 내용이 나만 있을 때)
- three-way-merge 형상이 다를때   
(사용 기존 브랜치에서 수정 내용이 나말고 또 다른 사람이 하거나 다른 곳에 또 다른 새로운 브런치 작업이 생겼을 때)

### fast-forward merge
```bash
1.$git branch //현재 브랜치 확인
2.$git branch topic //topic이라는 브랜치 생성
3.$git log //작업에 브랜치들이 가리키는 지점 확인할 수 있다.
4/$git checkout topic//topic이라는 브랜치만 가리키게 된다.
5.$git checkout master//새로운 브랜치에서 작업하다가 다시 마스터로 돌아오면 새 브랜치에서 작업한 내용 전 master(mian)의 저장내역으로 돌아간다.
6.$git merge topic // 마스터브랜치에 새 작업으로 만들었던 topic의 내용이 합쳐진다.
```

### three-way-merge
```bash
$git init 
$touch 회원가입.txt //파일생성
$git add .
$git commit -m '회원가입'
$touch 로그인.txt
$git add .
$git commit -m '로그인'
$git log //로그 기록 확인
$git checkout -b topic //체크아웃 하면서 새로운 브랜치 생성
$git branch //브랜치확인
$touch 아이디중복체크.txt
$git add .
$git commit -m '아이디중복체크'
$git checkout master //다시 메인 브랜치로 이동
$touch 글쓰기.txt
$git add .
$git commit -m '글쓰기'
$git checkout topic
$git log
$git checkout master //현재 메인과 새브랜치에 각각 새로운 내용이 있고 merge를 하면 한쪽이 사라지는 상황 이때는 fast-forward merge 할 수 없는 상황
$git merge  topic //하면 fast-forward merge 다른 내용의 메세지가 뜬다.
1.이때 esc를 누르고 :wq 를 입력하고 엔터를 해준다.
2.그러면 three-way-merge가 된다.
$git log //three-way-merge 내역을 확인할 수 있다.

```

### git merge 충돌
> 동일한 파일을 다르게 1번, 2번으로 만들어 merge가 충돌되는 상황 이때 1번 파일을 쓸건지, 2번 파일을 쓸것인지 정해야한다. 

```bash
$git init
$touch 로그인.txt //만든 상황에서 내용입력
$git add .
$git commit -m '로그인 버튼'
$git log
$git checkout -b topic //새로운 브랜치를 만들고 로그인.txt 내용을 수정
$git add.
$git commit -m '로그인 체크박스'
$git checkout master //다시 메인 브랜치로 이동해서 로그인.txt 내용을 또 수정
$git add .
$git commit -m '로그인 라디오박스'
$git merge topic //같은 파일을 수정한 상황에서 메인브랜치에 topic브랜치를 merge하려고 하면 complict라는 에러 메시지가 뜬다.
```
```
* complict메시지 *
CONFLICT (content): Merge confilct in 파일명 Automatic merge failed; fix conflicts and then commit the result
```
```bash
* 충돌 메시지가 뜨면서 파일에 충돌되는 내용들이 뜨는데 거기서 지울 내용과 남길 내용을 수정하고
$git add .
$git status //상태 다시확인
$git commit -m '충돌 수정' //하면 충돌 되었던 것을 해결된다.
** 왠만하면 같은 파일을 건드려 충돌을 만들지 않는 것이 중요하다.
```

### rebase
> 로그를 깔끔하게 다시 정리한다.

```bash
$git init
$touch 환경설정.txt
$git add .
$git commit -m '환경설정'
$touch 로그인123.txt
$git add .
$git commit -m '로그인123'
$touch 로그인345.txt
$git add .
$git commit -m '로그인345'
$touch 로그인완료.txt
$git add .
$git commit -m '로그인완료'
$git log
$git rebase -i HEAD~3 //rebase 명령어를 입력하고 화면이 뜨면 i를 눌러서 rebase할 대상들 설정
```
```
pick af37cb4 로그인123 //pick 선택
d 9528bf8 로그인456 //drop한다는 명령 (삭제)
r 95sdbf8 로그인789->로그인777로 수정 //로그내용을 수정한다는 명령 (수정)
pick b6bb999 로그인완료 //pick선택

# Rebase ec70aaa..b6bb999 onto ec70aaa (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified); use -c <commit> to reword the commit message
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
* 설정을 다하고 esc누르고 :wq로 저장하고 나가기

```
#### 찌그려트리기?
```
pick af37cb4 로그인123 
s 9528bf8 로그인456 
s b6bb999 로그인완료 
//항상 맨위에 pick하고 나머지를 s
//esc누르고 :wq를 입력하고 엔터하면 수정화면이 뜬다.
//맨밑에 내용 빼고 삭제
//esc하고 :wq
```

### github?
> 분산버전관리가 가능한 클라우드 저장소
- 깃헙컴퓨터 = origin
- 원격관리 = remote
```bash
$git remote add origin 레포지토리 주소 // 연결
$git ls remote //연결된 레포지토리 확인
$git push origin master //업로드+merge
```

### github 올린 파일 다운로드
```bash
$git init
$git remote add origin 레포지토리 주소
$git ls-remote //주소를 잘못 연결했을 때 삭제하는 명령어
$git pull origin master // 입력하며 주소의 파일이 다운로드 된다.
$git log // 잘 받았는지 확인
```

### github clone
> clone은? git init, remote, pull을 한방에 해버리는 명령어
```bash
$git clone 레포지토리 주소 //한번에 받아짐 한번도 받지 않은 곳에서는 clone사용 그 이후는 pull사용
```

### github branch
> git과 동일
```bash
$git checkout -b topic //새로운 브랜치생성
$git add .
$git commit -m '50프로 완료'
$git push origin topic //새 브랜치에 업로드
```
> github에는 새 브랜치가 만들어져 있지만, 새로운 컴퓨터에서 pull했을 때는 새 브랜치가 생성되지 않는다 이럴때는 브랜치를 만들어 받아주어야 한다.
```bash
$git fetch origin // 모든 브랜치 다운로드
$git checkout checkout -b topic origin/topic //생성과 동시에 데이터를 병합해서 받아줌
```
### 모든 작업이 끝난 브랜치 merge(병합)
```bash
$git checkout master //메인 브랜치로 이동
$git merge --squash topic(병합할 브랜치) // commit이 안된 상태로 돌아감 rebase와 같은 역할 더 쉽다
$git commit -m '완료'// commit을 다시 해주고
$git push origin master// 업로드 해주된 끝
```