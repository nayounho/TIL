# useRef

## useRef의 활용
1. 컴포넌트 별로 데이터를 갖고 싶을 때 사용
2. 리렌더링과 상관없이 값을 값을 유지
3. useRef로 만든 값을 변경하여도 리렌더링이 이루어지지 않는다
4. .current 로 접근 가능

### useRef 사용 예시
```
import React, { useCallback, useRef } from 'react';

const number = () => {
  const numberRef = useRef(0);

  const onClick = useCallback(() => {
    numberRef.current++;
  }, []);

  return <div onClick={onClick}>Number</div>;
};

export default number;
```

1. 초기값은 useRef 생성시 인수로 넣어준다
2. numberRef의 값을 가져 올 때 numberRef.current 로 접근

### useRef의 다른 사용 예시
   - DOM노드를 저장하는 데 사용

```
import React, { useEffect, useRef } from 'react';

const number = () => {
  const numberRef = useRef(false);

  useEffect(() => {
    if (numberRef.current) {
      console.log('update');
    } else {
      numberRef.current = true;
    }
  });

  return <div>Number</div>;
};

export default number;
```

1. useEffect가 마운트될 때 한 번 실행되는데 그 때는 numberRef가 false 이기때문에 if문을 실행하지 않는다
2. 단, else문에서 false를 true로 바꾸어 놓았기 때문에 리렌더링부터 if문 실행

## 결론: seRef는 그 안의 데이터가 바뀌어도 화면을 리렌더링하지 않지만, 리렌더링 후에도 데이터를 유지