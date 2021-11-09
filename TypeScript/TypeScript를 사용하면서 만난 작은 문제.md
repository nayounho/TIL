# TypeScript를 사용하면서 만난 작은 문제

## React' must be in scope when using JSX
- 최상단에 ```import React from 'react'``` 이렇게 import로 react를 불러와야 하는데 안불러오거나 
  React의 첫번째 알파벳이 대문자여야 하는데 소문자로 쓴 경우 발생하는 에러

## '--isolatedModules' 에러 
- 파일내에서 태그를 같이 사용하는 JSX 파일이라면 확장자명을 JSX(typeScript를 사용하면 TSX)로 해주어야 한다.