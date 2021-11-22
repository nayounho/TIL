# CRA, TS, prettier, eslint 초기셋팅

## 설치
```npx create-react-app app이름 --template typescript```
```npm i eslint-plugin-prettier```
```npm i eslint-config-prettier```
```npm i eslint-plugin-react```
```npm i prettier```



## 셋팅

- 이후 root폴더에 .eslintrc와 .prettierrc 파일 생성 후 밑의 내용을 작성

  - .eslintrc.json
    ```
    {
      "extends": ["plugin:prettier/recommended"],
      "plugins": ["react", "prettier"],
      "parserOptions": {
        // ESLint는 es6 이후의 문법을 알지 못하기 때문에 설정
        // https://eslint.org/docs/user-guide/configuring#specifying-parser-options
        "ecmaVersion": 6,
        "ecmaFeatures": {
          "jsx": true // eslint-plugin-react
        }
      },
      "rules": {
        // 설정하고 싶은 규칙 작성
        // 밑은 예시일 뿐, 아무거나 추가 가능
        "prettier/prettier": ["error", { "singleQuote": true }], // eslint-plugin-prettier
        "react/jsx-uses-vars": "error" // eslint-plugin-react
      },
      "ignorePatterns": ["*.config.js"] // 제외하려는 파일
    }
    ```
  - .prettierrc.json
    ```
    {
      "singleQuote": true,
      "semi": true,
      "useTabs": false,
      "tabWidth": 2,
      "trailingComma": "all",
      "printWidth": 80
    }
    ```
- 패키지 및 config 항목의 의미
  - eslint-config-prettier: ESLint와 Prettier의 충돌을 막아줌
  - eslint-plugin-react: React에서 ESLint 명세 규정
  - eslint-plugin-prettier: Prettier를 ESLint 규칙으로 실행하고 문제점을 ESLint로 보고
    ESLint와 Prettier는 이전글에서 확인 가능하다.