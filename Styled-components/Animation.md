# Styled-Components에서 Animation 사용

## 1. Basic

```
function App() {

  const Box = Styled.div`
  width: 200px;
  height: 200px;
  background-color: tomato;
  `

  return (
    <Box />
  )
}
```

- 위와 같은 코드를 렌더링하면 토마토색의 200px \* 200px의 사각형이 나타날 것이다
- 내가 하고 싶은 것은 사각형이 빙글빙글 돌아가는 것을 원한다

## 2. keyframes

- 먼저 keyframes를 import 시킨다
  `import { keyframes } from 'styled-components'`

- import한 후 css와 같이 animation 동작을 설정한다
- 빙글빙글 돌아가는 것과 더불어 원으로 변하는 것을 만들고 싶었다

```
import styled from 'styled-components'

const animation = keyframes`
  0% {
    transform: rotate(0deg);
    border-radius: 0px;
  }
  50% {
    border-radius: 100px; // Tip: 원으로 만들기 위해서는 사각형의 반을 border-radius 주면 된다
  }
  100% {
    transform: rotate(360deg);
    border-radius: 0px;
  }

  const Box = styled.div`
    width: 200px;
    height: 200px;
    background-color: tomato;
    display: flex;
    justify-content: center;
    align-items: center;
    animation: ${animation} 1s linear infinite; // animation 적용을 위에 만들었던 animation style을 가져와서 만든다
  `

   return (
    <Box />
  )
`
```
