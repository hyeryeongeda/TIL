git init  : git 시작 (로컬 저장소 생성)
rm -rf .git : git 삭제(git 종료)

git add . : staging area에 변경사항 올리기
git status : staging area의 작업 파일 확인

git config --global user.email "이메일주소"
git config --global [user.name](http://user.name/) "이름"
git config --global -l

git commit -m "메시지명" : repository에(staging area의 변경사항을)올리기
git log : repository 작업 파일 확인 (커밋 이력 확인)

---

참고자료

git commit --amend : vim 에디터가 열린다

수정 후(커서 위치를 확인 후 과감하게 타이핑)

esc -> :q -> 저장하지 않고 종료
esc -> :q! -> 저장하지 않고 강제 종료
esc -> :wq -> 저장하고 종료

git add .

git commit

push

pull

remote(URL)

status
---

- **git init**: 새로운 git 저장소를 초기화합니다.
- **git clone [URL]**: 원격 저장소를 로컬로 복제합니다.
- **git add [파일명]**: 특정 파일을 스테이징 영역에 추가합니다.
- **git commit -m "메시지"**: 변경 사항을 커밋합니다.
- **git push**: 로컬 커밋을 원격 저장소에 푸시합니다.
- **git pull**: 원격 저장소의 최신 변경 사항을 가져옵니다.
- **git branch**: 브랜치를 확인하거나 생성합니다.
- **git checkout [브랜치명]**: 특정 브랜치로 전환합니다.
- **git merge [브랜치명]**: 다른 브랜치를 현재 브랜치에 병합합니다.
- **git status**: 현재 상태(변경 사항, 스테이징된 파일 등)를 확인합니다.
- **git log**: 커밋 로그를 확인합니다.

---

Git 원격 저장소와의 데이터 통신
• git pull: 원격 저장소의 내용을 로컬 저장소로 가져옴
• git push: 로컬 저장소의 내용을 원격 저장소로 보냄 

강제로 가져오는 명령어

git pull origin +master
git push origin +master

bash에서 붙여넣기는 shift + insert

폴더가 삭제 됐을 경우 git clone URL하면 다시 받아올 수 잇다.

**자리 옮긴다면**

1. git clonfig 
2. 제어판→ 사용자계정→window 자격증명 들어가서 깃헙 증명 계정 지워야함 

revert

reset

reflog

git restore  —staged 파일명