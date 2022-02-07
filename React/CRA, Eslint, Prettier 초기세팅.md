# CRA, Eslint, Prettier 초기세팅

- Create-React-App으로 프로젝트를 만들 때 eslint, prettier를 적용하는 방법 기록 (간단한데 매번 찾아봐야 해서 귀찮음)

## 1. `npx create-react-app 프로젝트명`

## 2. ESLint와 Prettier 설치

2-1. 패키지 설치 1. npm 패키지로 설치하는 것이 제일 편하다 2. CRA에는 eslint가 내장되어 있기 때문에 eslint 추가 설정을 위한 패키지만 설치

`npm install -D prettier eslint-config-prettier eslint-plugin-prettier`

2-2. eslintrc 파일 생성 및 설정

- root 위치에 .eslintrc 파일을 만들고 아래 내용을 입력
- rules 부분은 협업하는 사람들과 상의 후 결정

```
{
  "extends": ["react-app", "plugin:prettier/recommended"],
  "rules": {
    "no-var": "warn", // var 금지
    "no-multiple-empty-lines": "warn", // 여러 줄 공백 금지
    // "no-console": "warn", // console.log() 금지
    "no-unused-vars": "warn", // 사용하지 않는 변수 금지
    "react/no-direct-mutation-state": "warn", // state 직접 수정 금지
    "react/no-unused-state": "warn", // 사용되지 않는 state
    "react/jsx-key": "warn", // 반복문으로 생성하는 요소에 key 강제
    "react/self-closing-comp": "warn", // 셀프 클로징 태그 가능하면 적용
    "prettier/prettier": [
      "error",
      {
        "endOfLine": "auto"
      }
    ]
  }
}
```

2-3. prettierrc 파일 생성 및 설정

- root 위치에 .prettierrc 파일을 만들고 아래 내용 입력

```
{
	"printWidth": 120,
	"tabWidth": 2,
	"singleQuote": true,
	"trailingComma": "all",
	"bracketSpacing": true,
	"semi": true,
	"useTabs": true,
	"arrowParens": "avoid",
	"endOfLine": "lf"
}
```

2-4. .vscode/setting.json 파일에서 내용 추가

```
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.tabSize": 2,
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": { "source.fixAll.eslint": true },
  "javascript.format.enable": false,
  "eslint.alwaysShowStatus": true,
  "files.autoSave": "onFocusChange",
```

## 3. .gitignore 파일 설정

- git에 올리지 않을 폴더/파일을 기록하는 문서.
- 자동으로 설정되어 있지만 아래 세 폴더/파일은 놓치지 않고 추가해 주자

```
/node_modules
/.vscode
.eslintcache
```

## 4. 실행 및 확인

- 위 작업까지 진행하고 실행해보았을 때, 아래 오류 메세지가 나타날 수 있다.

`Error while loading rule 'prettier/prettier': context.getPhysicalFilename is not a function`

- 이 오류는 eslint의 버전 호환 문제로, 아래의 명령어를 통해 해결

`npm upgrade -R eslint`
