# JSX 란

## JSX

`const hello = <h1>Hello world!</h1>`

- 위 문법은 js를 확장한 문법인 JSX이다.
- JSX는 js의 모든 기능이 포함되어 있다.
- JSX는 React Element를 생성한다.

## 1. JSX에 표현식 포함하기

```
const name = 'Na';
const element = <h1>Hello {name}</h1>
```

- JSX의 중괄호 안에는 js의 모든 표현식을 넣을 수 있다.
- 컴파일이 끝나면 JSX 표현식이 정규 js 함수 호출이 되고, js 객체로 인식한다.

## 2. JSX 속성 정의

- 속성에 따옴표를 이용해 문자열 리터럴을 정의할 수 있다.

```
const element = <div tabIndex = "0"></div>;
// 어트리뷰트에 표현식 삽입도 가능
// 중괄호 주변에 따옴표를 입력하지 않는다.
const element = <img src={user.images}/>
```

## 3. JSX는 객체를 표현한다.

- 다음 두 예시는 동일한 결과를 도출한다.

```
const element = (
  <h1 className="greeting">
  Hello world
  </h1>
);

const element = React.createElement(
  'h1',
  {className: "greeting"},
  'hello world'
);

// React.createElement()는 몇 가지 검사를 수행해 다음과 같은 객체를 생성한다.
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello world'
  }
}
```

- 이러한 객체를 React Element라고 한다.
- React는 이 객체를 읽어서 DOM을 구성하고 최신 상태를 유지하는 데 사용한다.
