# [React] GlobelStyle 설정

- styled-components와 styled-reset을 사용하여 GlobalStyle 설정 방법을 기록하자

---

## 1. styled-components, styled-reset 설치

- createGlobalStyle을 사용하기 위하여 `npm install styled-components` 설치
- css 초기화를 위한 `npm install styled-reset` 설치

## 2. GlobalStyles.js 파일 생성

- styled-components는 local로 동작하기 때문에 GlobalStyles.js 파일을 생성해서 Global Style을 관리한다.

scr/styles/GlobalStyle.js

```
import { createGlobalStyle } from 'styled-components';
import reset from 'styled-reset';

const GlobalStyle = createGlobalStyle`
    ${reset};
    body * { // body 자식 요소들의 기본 설정
      padding: 3px;
      margin: 0;
      list-style: none;
      box-sizing:border-box;
    };
    button{ // button의 기본 설정
        display: flex;
        cursor: pointer;
        outline: none;
        border-radius: 3px;
    };
    input{ // input의 기본 설정
        display: flex;
        outline: none;
        padding-left: 10px;
    }
`;

export default GlobalStyle;
```

- 여기서 font도 설정해 놓으면 좋다.

## 3. GlobalStyle 파일 사용

- 웹의 전반적인 곳에서 사용되기 위하여 App.js에서 불러와 사용

src/App.js

```
import Login from 'pages/login/Login';
import Main from 'pages/main/Main';
import { Route, Routes } from 'react-router-dom';
import GlobalStyle from 'styles/GlobalStyle';

function App() {
	return (
		<>
			<GlobalStyle />
			<Routes>
				<Route path="/login" element={<Login />} />
				<Route path="/" element={<Main />} />
			</Routes>
		</>
	);
}

export default App;
```
