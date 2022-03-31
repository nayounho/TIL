# git 협업 방법

## 1. 진행 순서

fork -> git clone -> git remote add pmorigin -> 작업 진행 -> (작업 후) commit / origin push ->
git pull pmorigin -> (만약 conflictrk 발생) conflict resolve -> commit / push ->
pull request -> (main 레포지토리에서) request 확인 후 결합

## 2. 진행 순서 상세

1. fork

   - main github에서 해당 레포지토리로 이동 후 fork를 눌러 내 github으로 복제하는 과정이다

<img src="./../Image/fork.png" width="1200px" height="500px" alt=""></img>

2. `git clone 해당 레포지토리 주소(내 github)`

   - fork를 떠오면 내 github에 해당 레포지토리가 생성된다
   - 생성된 레포지토리를 내 local로 clone하는 과정이다

3. git remote add pmorigin

   - 나중에 pull을 받기 위해서 내 local과 pmorigin을 연결하는 과정이다

`git remote add pmorigin 해당 레포지토리 주소(main 레포지토리 주소)`

4. 작업진행

   - local에서 작업 진행

5. (작업 후) commit / origin push

   - 작업 완료 후 fork로 복제해 온 내 github에 commit / push를 진행하는 과정이다

6. `git pull pmorigin`

   - main 레포지토리에 변경사항이 있을 수 있으니 pull를 받아서 확인하는 과정이다

7. (만약 conflictrk 발생) conflict resolve

   - conflict가 발생하면 출동 부분을 해결하는 과정이다

8. commit / push

   - conflict를 해결하고 commit / push를 다시 한 번 해주어 버전관리를 하는 과정이다

9. pull request

   - pull request를 진행하여 main 레포지토리에 내가 작업한 내용을 합쳐달라고 요청하는 과정이다

<img src="./../Image/pull%20request%20접근.png" width="1200px" height="400px" alt=""></img>

10. (main 레포지토리에서) request 확인 후 결합

    - main 레포지토리를 관리하는 사람이 request를 확인하고 main code와 결합하는 과정이다
