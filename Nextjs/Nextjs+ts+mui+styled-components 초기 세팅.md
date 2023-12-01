# Nextjs + TS + Mui + Styled-components 초기 세팅

1. create-next-app

```
npx create-next-app --ts 프로젝트이름
```

2. Emotion 설치

```
npm i @emotion/react @emotion/styled @mui/icons-material @mui/material @mui/styles
```

- MUI 설치를 진행합니다. MUI는 styling 엔진을 같이 사용할 수 있는데, 보통 styled-components 또는 emotion를 같이 사용합니다. 이번에는 styled-components를 사용해보겠습니다.

- 설치시 유의사항으로는 nextjs 프레임워크 설치시 질문 중 "Would you like to use App Router?(recommended)" 라는 질문을 하는데 프레임워크 추천이라고 되어 있어 yes라고 하게되면 'app/pages.tsx' 이런식으로 폴더 구조가 만들어 진다. 이 폴더 구조가 좋다면 yes로 하는게 맞지만 본인이 'pages/index.tsx' 구조가 편하다면 no라고 대답하는 것이 좋다.

```
/tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    // 초기에 moduleResolution: "Bundle"로 되어 있는데 Node로 변경해준다.
    "moduleResolution": "Node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```

- config.json 수정

3. `_document.tsx` 수정

- `_document.tsx` 수정은 서버에서 받아온 html, css와 클라이언트가 렌더링한 html, css가 다르면 next에서 warning을 띄우게 된다. 그래서 서버단에서 mui를 지원함으로 서버와 클라이언트간 간극을 맞추기 위해 아래와 같이 구현.

```
// pages/_document.tsx
import Document, {
  Html,
  Head,
  Main,
  NextScript,
  DocumentContext,
} from "next/document";
import { ServerStyleSheets } from "@mui/styles";

export default class MyCocument extends Document {
  static async getInitalProps(ctx: DocumentContext) {
    const materialSheets = new ServerStyleSheets();
    const originalRanderPage = ctx.renderPage;


      ctx.renderPage = () =>
        originalRanderPage({
          enhanceApp: (App) => (props) =>
            materialSheets.collect(<App {...props} />),
        });

      const initialProps = await Document.getInitialProps(ctx);
      return {
        ...initialProps,
        styles: (
          <>
            {initialProps.styles}
          </>
        ),
      };
   }

  render() {
    return (
      <Html>
        <Head>
          <style />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}
```

4. styled-components 설치 및 적용

```
// styled-components 설치
npm i styled-components
```

```
// babel-plugin-styled-components 설치
npm i -D babel-plugin-styled-components
```

- 바벨 수정
  - 바벨 수정은 next.js의 ssr과 연관이 있다.
  - ssr에 의해 styled-components 스타일이 적용 전에 화면 렌더링이 되는
    문제를 방지
- root 폴더에 .babelrc 생성

```
// .babelrc
{
  "presets": ["next/babel"],
  "plugins": [["styled-components", { "ssr": true }]]
}
```

5. `_document.tsx` 수정

- styled-component 적용을 위해 `_document.tsx`파일을 수정한다.

```
// pages/_document.tsx
import Document, {
  Html,
  Head,
  Main,
  NextScript,
  DocumentContext,
} from "next/document";
// 추가된 부분
import { ServerStyleSheet } from "styled-components";
import { ServerStyleSheets } from "@mui/styles";

export default class MyCocument extends Document {
  static async getInitalProps(ctx: DocumentContext) {
    // 추가된 부분
    const sheet = new ServerStyleSheet();
    const materialSheets = new ServerStyleSheets();
    const originalRanderPage = ctx.renderPage;

    try {
      ctx.renderPage = () =>
        originalRanderPage({
          enhanceApp: (App) => (props) =>
          // 추가된 부분: sheet.collectStyles로 기존 mui 코드를 감싸준다.
            sheet.collectStyles(materialSheets.collect(<App {...props} />)),
        });

      const initialProps = await Document.getInitialProps(ctx);
      return {
        ...initialProps,
        styles: (
          <>
            {initialProps.styles}
            // 추가된 부분
            {sheet.getStyleElement()}
          </>
        ),
      };
      // 추가된 부분
    } finally {
      sheet.seal();
    }
  }

  render() {
    return (
      <Html>
        <Head>
          <style />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

```

6. theme 적용해보기

```
// _app.tsx
import "@/styles/globals.css";
import theme from "@/styles/theme";
import type { AppProps } from "next/app";
import { createGlobalStyle, ThemeProvider } from "styled-components";

export default function App({ Component, pageProps }: AppProps) {
  const GlobalStyle = createGlobalStyle`
    body {
      margin: 0;
      background-color: tomato;
    }
  `;

  return (
    <ThemeProvider theme={theme}>
      <GlobalStyle />
      <Component {...pageProps} />;
    </ThemeProvider>
  );
}
```

7. pakage.json

```
// pakage.json
{
  "name": "test-nextjs-styled-components",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "@emotion/react": "^11.11.1",
    "@emotion/styled": "^11.11.0",
    "@mui/icons-material": "^5.14.19",
    "@mui/material": "^5.14.19",
    "@mui/styles": "^5.14.19",
    "babel-plugin-styled-components": "^2.1.4",
    "next": "14.0.3",
    "react": "^18",
    "react-dom": "^18",
    "styled-components": "^6.1.1"
  },
  "devDependencies": {
    "@types/node": "^20",
    "@types/react": "^18",
    "@types/react-dom": "^18",
    "eslint": "^8",
    "eslint-config-next": "14.0.3",
    "typescript": "^5"
  }
}
```

8. 참고

- nextjs + ts + mui 셋업
  - https://kyounghwan01.github.io/blog/React/next/mui/#app-tsx
- styled-components 적용
  - https://kyounghwan01.github.io/blog/React/next/mui-styled/#%E1%84%80%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A9%E1%84%87%E1%85%A5%E1%86%AF-%E1%84%89%E1%85%B3%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF-%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5
  - https://lucy0218.com/entry/Nextjs-%EC%97%90%EC%84%9C-styled-components-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0
