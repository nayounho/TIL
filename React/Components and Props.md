# Components and Props

## 1. 함수형 컴포넌트와 클래스형 컴포넌트

```
// 함수형 컴포넌트
function Welcome(props) {
   return <h1>Hello {props.name}</h1>;
}

// 클래스형 컴포넌트
class Welcome extends React.Component {
  render() {
    return  <h1>Hello {this.props.name}</h1>;
  }
}
```

- 이 함수는 하나의 props 객체를 인자로 받은후 React 엘리먼트를 반환하므로 유효한 React 컴포넌트다.
- React 관점에서 볼때 두 가지 유형의 컴포넌트는 동일하다.

## 2. 컴포넌트 렌더링

- React 엘리먼트는 사용자 정의 컴포넌트로도 나타낼 수 있다.

```
const element = <Welcome name="Youn"/>;
```

- React가 사용자 정의 컴포넌트로 작성한 엘리먼트를 발견하면 JSX 어트리뷰트와 자식을 해당 컴포넌트에 객체로 전달하는데 이 객체가 props 이다.

## 3. 컴포넌트 합성

- 컴포넌트는 다른 컴포넌트를 참조할 수 있습니다.

```
function Welcome(props) {
  return <h1>Hello {props.name}</h1>;
}

function App() {
  return (
    <div>
    <Welcome name="Youn">
    <Welcome name="Kuk">
    <Welcome name="Goo">
    </div>
  )
}
```

## 4. 컴포넌트 추출

- 다음과 같은 컴포넌트가 있다.

```
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
        {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
      {props.text}
      </div>
      <div className="Comment-data">
      {formatData(props.data)}
      </div>
    </div>
  );
}
```

- 이 컴포넌트는 구성요소들이 모두 중첩 구조로 이루어져 있어서 변경하기 어렵고 개별적 재사용하기도 힘들다.

```
function Avatar(props) {
  return (
    <img className="Avatar"
          src={props.user.avatarUrl}
          alt={props.user.name}
        />
  );
}
```

- Avatar 는 자신이 Commnet 내에서 렌더링 된다는 것을 알 필요가 없다.

```
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user}>
      <div className="UserInfo-name">
      {props.user.name}
      </div>
    </div>
  );
}
```

- 이제 Comment 컴포넌트는 좀더 단순해 질 수 있다.

```
function Comment(props) {
  return (
    <div className="Comment">
     <div className="UserInfo" />
     <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-data">
        {formatData(props.data)}
      </div>
    </div>
  );
}
```

## 5. Props는 읽기 전용이다.

- 모든 컴포넌트는 자체 props를 수정해서는 안된다. 자신의 props를 다룰 때는 반드시 순수 함수처럼 동작해야 한다.
