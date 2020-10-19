## 새로운 repository 생성
# A
1. git init : 현재 작업 중인 폴더에서 git 사용
2. git add : 폴더 내의 모든 파일을 stagy 추가
3. git commit -m"commit message" : 커밋과 동시에 -m으로 커밋 메시지 작성
4. git remote add origin repostory 주소 : 원격저장소, github repository 주소를 origin이라는 이름으로 등록
5. git push -u origin main/master : origin이라는 원격 저장소에 master/main 브랜치에 푸시, 이때 -u를 쓴다면 다음번 부터 git push만 입력해도 origin의 master/main으로 푸시가 된다.

# B
- A 이후 변경사항이 있어 push를 하고 싶을 경우
1. git add
2. git commit -m "commit message"
3. git push