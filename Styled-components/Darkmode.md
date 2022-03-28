# Dark mode 만들기

## Basic

- Dark mode 시 글씨가 흰색, 배경색이 검은색으로 만들고 싶다
- Styled-Components의 ThemeProvider를 이용한다

```
import styled from 'styled-components';

fucntion App() {
  const Wrapper = styled.div`
  display: flex;
  flex-flow: column;
  width: 100vw;
  height: 100vh;
  justify-content: center;
  align-items: center;
  `;

  const Title = styled.h1`
    font-size: 50px;
  `;

  const Button = styled.button`
    padding: 10px;
  `;

  return (
    <Wrapper>
    <Title>Hello</Title>
    <Button>Dark Mode</Button>
    </Wrapper>
  )
}
```

## ThemeProvider

- ThemeProvider를 import 한다
  `import styled, { ThemeProvider } from 'styled-components'`
- <ThemeProvider>를 전체 태그를 감싸주면 지정된 테마의 프로퍼티를 가져다 쓸 수 있다

```
import styled { ThemeProvider} from 'styled-components';

fucntion App() {
  const darkmode = {
    textColor: 'white',
    background-color: 'black'
  }

  const lightmode = {
    textColor: 'black',
    background-color: 'white'
  }

  const Wrapper = styled.div`
  display: flex;
  flex-flow: column;
  width: 100vw;
  height: 100vh;
  justify-content: center;
  align-items: center;
  background-color: ${props => props.theme.backgroundColor}
  `;

  const Title = styled.h1`
    font-size: 50px;
    color: ${props => props.theme.textColor}
  `;

  const Button = styled.button`
    padding: 10px;
    background-color: ${props => props.theme.backgroundColor}
    color: ${props => props.theme.textColor}
    border: 1px solid ${props => props.theme.textColor}
  `;

  return (
    <ThemeProvider theme={darkMode}>
    <Wrapper>
    <Title>Hello</Title>
    <Button>Dark Mode</Button>
    </Wrapper>
    </ThemeProvider>
  )
}
```

## onClick Event

- 위까지 되면 <ThemeProvider> theme 부분에 넣어준 값으로 색 선택이 가능해지는데 Button에 onClick Event를 줘서 동적으로
  dark mode와 light mode를 변경하고 싶다
- useState를 이용하여 상태에 따라 theme를 변경해주자

```
import styled { ThemeProvider} from 'styled-components';
import useState from 'react';

fucntion App() {
  const [mode, setMode] = useState(true);

  const darkmode = {
    textColor: 'white',
    background-color: 'black'
  }

  const lightmode = {
    textColor: 'black',
    background-color: 'white'
  }

  const Wrapper = styled.div`
  display: flex;
  flex-flow: column;
  width: 100vw;
  height: 100vh;
  justify-content: center;
  align-items: center;
  background-color: ${props => props.theme.backgroundColor}
  `;

  const Title = styled.h1`
    font-size: 50px;
    color: ${props => props.theme.textColor}
  `;

  const Button = styled.button`
    padding: 10px;
    background-color: ${props => props.theme.backgroundColor}
    color: ${props => props.theme.textColor}
    border: 1px solid ${props => props.theme.textColor}
  `;

  const onClick = () => {
    setMode(pre => !pre)
  }

  return (
    <ThemeProvider theme={mode ? lightMode : darkMode}>
    <Wrapper> // Tip: <Wrapper>를 h1태그가 아닌 h2나 h3로 하고 싶다면 styled를 바꾸지 않고 <Wrapper as='h2'>로 바꿀 수 있다
    <Title>Hello</Title>
    <Button onClick={onClick}>Dark Mode</Button>
    </Wrapper>
    </ThemeProvider>
  )
}
```
