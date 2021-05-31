# useMemo, useCallback

- 성능 최적화와 관련한 hook

## useMemo:

- 값을 기억(memorized), 이전에 계산 한 값을 재사용
- useMemo
    - 첫번째 파라미터: 어떻게 연산할지 정의하는 함수
    - 두번째 파라미터: deps 배열 → 이 배열 안에 넣은 내용이 바뀌면 우리가 등록한 함수가 호출, 만약 바뀌지 않으면 이전에 연산한 값을 재사용

## useCallback:

- 함수를 기억
- useCallback의 두 번째 인자는 매우 중요 (어떨때 다시 실행 되는지 정함)
- 자식 컴포넌트에 props로 함수를 넘길 때는 useCallback를 해주어야 한다.
`
useCallback(() => {}, [])
`