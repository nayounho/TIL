# 405 Not Allowed Error

## 에러 발생

- 프로젝트 진행 중 CRUD를 구현하는 과정에서 member삭제 기능을 구현하고 있었다
- 그런데 아래와 같은 에러가 발생한 것이다. 405error는 처음봐서 굉장히 당황스러웠다

  <img src="./../Image/405%20ERROR.png" width="300px" height="150px" alt="405 error"></img></br>

- 에러가 나오면 한 번 읽어보고 모르겠다 싶으면 뭐다??
  구글링 시작...

## 에러 원인

- 구글링을 해보니 프론트의 Api method( Get 혹은 Post )와 Api 서버가 설정한 Api method가 다르면 발생하는 에러였다
- 예를 들어, 프론트에서는 Post 요청을 했는데, 서버에서는 해당 api 요청을 Get으로 받는 것과 같은 상황이다
  에러 원인을 확인하고, 문제의 프론트 요청이 제대로 되고 있는지 확인하였다.
- 에러 원인을 보고 왜 method가 다르게 갔을까 고민해봤다. 백엔드쪽은 정해진 method 규칙이 있기 때문에
  프론트에서 잘 못 보낸게 확실한데...
- 한참 고민해보다가 "Method Not Allowe" 라는 에러 메세지가 눈에 들어왔다
  허용되지 않는 방법이라...
  DELETE 요청의 규칙이 맞지 않는 건가 이런 생각을 하다가 원인을 찾게 되었다

- 내가 짠 코드는 현재 클릭한 곳의 id를 삭제 요청하는 api로직으로 전달하는 코드였는데
  문제는 삭제버튼을 클릭하면 모달창이 나오도록 설계하여 모달에서 확인버튼을 클릭해야
  삭제가 이루어지는 구조였다

  ```
   const deleteMember = async e => {
    const currentTargetMemberId = e.currentTarget.parentElement.parentElement.id;
    await memberApi.memberDelete(currentTargetMemberId);
  };
  ```

  모달창과 현재 이벤트가 발생하는 위치는 관계가 없기 때문에 제대로 id를 확인할 수가 없었다

- 모달창에서 원하는 id를 찾아내는 방법을 고민해야 겠지만 일단 에러원인을 파악하는데는 성공했다
