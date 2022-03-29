# TypeScript 기본 적용 방법

## Typing the Props

```
//App.tsx

import Circle from './Circle';

function App() {
  return (
    <div>
      <Circle bgColor='tomato' borderColor='yellow'/>
      <Circle bgColor='teal'text='New Circle'/>
    </div>
  )
}
```

```
//Circle.tsx

import styled from 'styled-components';

cosnt Circle = ({borderColor, bgColor, text='Default Circle'}} => {
  const Container = styled.div`
  width: 200px;
  +height: 200px;
  background-color: ${(props) => props.bgColor};
  border-radius: 100px;
  border: 2px solid ${(props) => props.borderColor};
  display: flex;
  justify-content: center;
  align-items: center;
  `
  return (
    <Container bgColor={bgColor} borderColor={borderColor}>
      {text}
    </Container>
  )
}

export default Circle;
```

- TypeScript Code

```
//Circle.tsx

import styled from 'styled-components';

interface ContainerProps<ContainerProps> {
  bgColor: string;
  borderColor: string;
}

interface CircleProps {
  bgColor: string;
  borderColor?: string; //Tip:  Optional Props
  text?: string; // App.ts에서 Circle 컴포넌트의 props로 text를 주지 않는 컴포넌트도 있기 때문에 text의
                 // 타입을 지정해 주지 않으면 에러가 발생한다
}

cosnt Circle = ({borderColor, bgColor, text='Default Circle'}: CircleProps} => {
  const Container = styled.div`
  width: 200px;
  +height: 200px;
  background-color: ${(props) => props.bgColor};
  border-radius: 100px;
  border: 2px solid ${(props) => props.borderColor};
  display: flex;
  justify-content: center;
  align-items: center;
  `
  return (
    <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}> //borderColor가 props로 전달 되지 않았다면 bgColor로 하겠다는 뜻
      {text}
    </Container>
  )
}

export default Circle;
```
