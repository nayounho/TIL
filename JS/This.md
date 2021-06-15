# This

- this는 자신이 속한 객체 또는 생성할 인스턴스를 가리키는 자기참조변수
- this를 참조하는 3가지 방법
    1. 일반 함수 호출
    2. 메서드 호출
    3. 생성자 함수 호출
- this 바인딩은 함수 호출 방식에 따라 동적으로 결정
- this 바인딩이란
```
바인딩(name binding)이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어, 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. this 바인딩은 this(키워드로 분류되지만 식별자 역할을 한다)와 this가 가리킬 객체를 바인딩하는 것
```

### 일반 함수 호출
- 기본적으로 this에는 전역 객체(global object)가 바인딩
  
```
function foo() {
  console.log(this);  // window

  function bar() {
    console.log(this); // window
  }
  bar();
}
foo();
```

- 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩

### 메서드 호출
- 메서드 내부의 this에는 메서드를 호출한 객체
    - 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩

```
const family = {
  name: 'kim',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(famaily.getName()); // kim
```

### 생성자 함수 호출
- 생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩

```
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### apply / call / bind
- 호출하는 방법에 따라 this가 가리키게 되는 객체는 동적으로 바뀌게 되는데 이렇게 동적으로 변하는 객체를 지정하는 방법이 apply / call / bind

- apply / call
  - apply와 call 메서드의 본질적인 기능은 함수를 호출

```
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

- bind
  - apply와 call 메서드와 달리 함수를 호출하지 않고 this로 사용할 객체만 전달

```
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 함수에 this로 사용할 객체를 전달한다.
// bind 메서드는 함수를 호출하지는 않는다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```