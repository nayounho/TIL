# exhaustive-deps-warning ERROR

## 발생 ERROR

`React Hook useEffect has a missing dependency: 'xxx'. Either include it or remove the dependency array react-hooks/exhaustive-deps`

- useEffect내에 사용하고 있는 state를 배열안에 추가시켜 달라는 의미이다.
- 2가지 방법으로 해결할 수 있다.

## 1. useEffect 내에 state 넣어준 경우

```
const [state, setState] = useState(0);

useEffect(() => {
  if (state > 10) return;
  setState(state + 1);
}, [state]);
```

- 가장 일반적인 방법은 state를 디펜던시 배열에 넣어주는 것이다.
- 다른 방법으로는 함수형 업데이트를 하는 방법이 있다.

```
const [state, setState] = useState(0);

useEffect(() => {
  if (state > 10) return;
  num = 0
  setState(num => num + 1);
}, []);
```

## 2. useEffect 내부에 함수를 정의한 경우

```
const getInfo = async () => {
  const res = await api.userInfoGet(params);
  setState(res.data.user_info);
};

useEffect(() => {
  getInfo();
}, [getInfo]);

// React Hook useEffect has a missing dependency: ....react-hooks/exhaustive-deps
```

- 함수를 실행하는 것은 문제가 없지만 useEffect 내에서 getInfo 함수가 params 라는 변수를
  쓰기 때문에 이 값을 배열에 넣어줘야 경고가 나오지 않는다.

```
const getInfo = async () => {
  const res = await api.userInfoGet(params);
  setState(res.data.user_info);
};

useEffect(() => {
  getInfo();
}, [getInfo, params]);
```

- 다른 방법으로는 useCallback를 이용하는 방법이 있다.

```
const getInfo = useCallback(async () => {
  const res = await api.userInfoGet(params);
  setState(res.data.user_info);
}, [params]);

useEffect(() => {
  getInfo();
}, [getInfo]);
```

- uaeCallback이 없다면 위 컴포넌트가 리렌더링 될 때마다 getInfo함수를 만들고
  새로운 참조값을 받아 getInfo함수를 실행하게 된다.
  하지만, useCallback을 정의하게 되면 params가 바뀔 때만 getInfo가 실행되어 불필요한 함수 생성 및 실행을
  막을 수 있다.
