# Nextjs + TS + Mui + Emotion 초기 세팅

## 설치하기

1. create-next-app

```
npx create-next-app --ts appName
```

- nextjs 프레임 워크 설치 (typeScript 사용시 명령어 + --ts)

2. Emotion 설치

```
npm i @mui/material @emotion/react @emotion/styled @emotion/server
```

- MUI 설치를 진행합니다. MUI는 styling 엔진을 같이 사용할 수 있는데, 보통 styled-components 또는 emotion를 같이 사용합니다. 저는 emotion를 사용해보겠습니다.
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

3. emotionCache factory 생성

- 먼저, 클라이언트와 서버 모두에서 작동하는 동일 구조의 이모션 캐시를 만들어야 합니다.
- utils/createEmotionCache.ts 생성

```
// utils/createEmotionCache.ts

import createCache from "@emotion/cache";

const isBrowser = typeof document !== "undefined";

// 클라이언트 측에서 <head> 상단에 메타 태그를 생성하고 삽입 지점으로 설정합니다.
// 이렇게 하면 MUI 스타일이 먼저 로드됩니다.
// 이를 통해 개발자는 CSS 모듈과 같은 다른 스타일링 솔루션으로 MUI 스타일을 쉽게 재정의할 수 있습니다.
const createEmotionCache = () => {
  let insertionPoint;

  if (isBrowser) {
    const emotionInsertionPoint = document.querySelector<HTMLMetaElement>(
      'meta[name="emotion-insertion-point"]'
    );
    insertionPoint = emotionInsertionPoint ?? undefined;
  }

  return createCache({ key: "mui-style", insertionPoint });
};

export default createEmotionCache;

```

4. `pages/_app.tsx` 수정

- `<ThemeProvider/>` 및 `<CSSBaseline/>` 컴포넌트를 렌더링할 수 있도록 `_app.tsx`를 수정한다.

```
// theme/defaultTheme.tsx
import { createTheme } from "@mui/material";
import { red } from "@mui/material/colors";
import { Inter } from "next/font/google";

export const inter = Inter({
  weight: ["300", "400", "500", "600"],
  subsets: ["latin"],
  display: "swap",
  fallback: ["Helvertica", "Arial", "sans-serif"],
});

const defaultTheme = createTheme({
  palette: {
    primary: {
      main: "#556cd6",
    },
    secondary: {
      main: "#19857b",
    },
    error: {
      main: red.A400,
    },
  },
  typography: {
    fontFamily: inter.style.fontFamily,
  },
});

// pages/_app.tsx
export default defaultTheme;
import defaultTheme from "@/theme/defaultTheme";
import createEmotionCache from "@/utils/createEmotionCache";
import { CacheProvider, EmotionCache } from "@emotion/react";
import { CssBaseline, ThemeProvider } from "@mui/material";
import type { AppProps as NextAppProps } from "next/app";
import Head from "next/head";

export interface AppProps extends NextAppProps {
  emotionCache?: EmotionCache;
}

const clientSideEmotionCache = createEmotionCache();

const App = ({
  Component,
  emotionCache = clientSideEmotionCache,
  pageProps,
}: AppProps) => {
  return (
    <CacheProvider value={emotionCache}>
      <Head>
        <meta name="viewport" content="initial-scale=1, width=device-width" />
      </Head>
      <ThemeProvider theme={defaultTheme}>
        <CssBaseline />
        <Component {...pageProps} />
      </ThemeProvider>
    </CacheProvider>
  );
};

export default App;
```

5. `_document.tsx` 수정

- SSR이 동작하도록 `_document.tsx`파일을 수정합니다.

```
// pages/_document.tsx
import Document, {
  Html,
  Head,
  Main,
  NextScript,
  DocumentProps,
  DocumentContext,
} from "next/document";
import { AppType } from "next/app";
import { AppProps } from "./_app";
import createEmotionCache from "../utils/createEmotionCache";
import createEmotionServer from "@emotion/server/create-instance";
import defaultTheme from "@/theme/defaultTheme";

interface MyDocumentProps extends DocumentProps {
  emotionStyleTags: JSX.Element[];
}

export default function MyDocument({ emotionStyleTags }: MyDocumentProps) {
  return (
    <Html lang="ko">
      <Head>
        <link rel="shortcut icon" href="/favicon.ico" />
        <meta name="theme-color" content={defaultTheme.palette.primary.main} />
        <meta name="emotion-insertion-point" content="" />
        {emotionStyleTags}
      </Head>
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}

MyDocument.getInitialProps = async (ctx: DocumentContext) => {
  const originalRenderPage = ctx.renderPage;

  const cache = createEmotionCache();
  const { extractCriticalToChunks } = createEmotionServer(cache);

  ctx.renderPage = () =>
    originalRenderPage({
      enhanceApp: (
        App: React.ComponentType<React.ComponentProps<AppType> & AppProps>
      ) =>
        function EnhanceApp(props) {
          return <App emotionCache={cache} {...props} />;
        },
    });

  const initialProps = await Document.getInitialProps(ctx);
  const emotionStyles = extractCriticalToChunks(initialProps.html);
  const emotionStyleTags = emotionStyles.styles.map((style) => (
    <style
      data-emotion={`${style.key} ${style.ids.join(" ")}`}
      key={style.key}
      dangerouslySetInnerHTML={{ __html: style.css }}
    />
  ));

  return {
    ...initialProps,
    emotionStyleTags,
  };
};

```

6. Modularized Imports 사용

- MUI를 사용할 때 개발 시간과 Nextjs 컴파일 시간을 단축하려면 Nextjs 구성에서
  modularizeImports 옵션을 사용해야 합니다.
- mui/icons-material과 같은 큰 패키지를 사용하는 경우 모듈화된 임포트를 사용하지 않으면 Nextjs 컴파일 시간이 크게 느려질 수 있습니다.
- `next.config.js` 수정

```
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  modularizeImports: {
    "@mui/material": {
      transform: "@mui/material/{{member}}",
    },
    "@mui/icons-materal": {
      transform: "@mui/icons-materal/{{member}}",
    },
    "@mui/styles": {
      transform: "@mui/styles/{{member}}",
    },
    "@mui/lab": {
      transform: "@mui/lab/{{member}}",
    },
  },
};

module.exports = nextConfig;
```

7. pakage.json

```
{
  "name": "advent-calendar-nextjs",
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
    "@emotion/server": "^11.11.0",
    "@emotion/styled": "^11.11.0",
    "@mui/material": "^5.14.18",
    "next": "14.0.3",
    "react": "^18",
    "react-dom": "^18"
  },
  "devDependencies": {
    "@types/node": "^20",
    "@types/react": "^18",
    "@types/react-dom": "^18",
    "eslint": "^8.54.0",
    "eslint-config-airbnb": "^19.0.4",
    "eslint-config-airbnb-typescript": "^17.1.0",
    "eslint-config-next": "14.0.3",
    "eslint-config-prettier": "^9.0.0",
    "eslint-plugin-import": "^2.29.0",
    "eslint-plugin-jsx-a11y": "^6.8.0",
    "eslint-plugin-prettier": "^5.0.1",
    "eslint-plugin-react": "^7.33.2",
    "eslint-plugin-react-hooks": "^4.6.0",
    "prettier": "^3.1.0",
    "typescript": "^5"
  }
}
```

8. 참고

- nextjs + ts + mui + emotion 셋업
  - https://reacthustle.com/blog/how-to-setup-mui-with-nextjs-emotion-and-typescript?expand_article=1
- google fonts 사용 이유 및 방법
  - https://velog.io/@dusunax/next.js-google-font-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0feat.-tailwind
- MUI 공식 홈페이지에서 제공하는 nextjs 적용 방법
  - https://mui.com/material-ui/guides/next-js-app-router/
