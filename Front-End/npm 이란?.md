# npm 이란?

## npm의 의미

- npm은 Node Packaged Manager의 약자
  - Node는 Node.js를 의미
  - Packaged라는 것은 package로 만들어진 것들을 의미 (Package는 모듈이라고도 불리는데 패키지나 모듈은 프로그램보다는 조금 작은 단위의 기능들을 의미)
  - Manager는 관리자를 의미
- npm이라는 것은 Node.js로 만들어진 pakage(module)을 관리해주는 툴
- npm은 Node.js로 만들어진 모듈을 웹에서 받아서 설치하고 관리해주는 프로그램
  개발자는 단 몇 줄의 명령어로 기존에 공개된 모듈들을 설치하고 활용할 수 있다
- npm은 거기서 그치지 않고 모듈들을 활용했다면 이후에 그 모듈을 만든 개발자가 업데이트를 하거나 할 경우 체크를 해서 알려준다
  버전관리 또한 쉬워진다는 것을 의미한다

## npm 사용 방법

- npm 설치
  - 예전에는 npm를 따로 설치하였지만 지금은 npde.js를 설치하면 내장(built in)되어 있다
- node.js는 npm를 사용하기 위해서는 꼭 필요하다. (node.js의 설치는 node.js 공식 홈페이지에서 다운이 가능하다)
- npm은 node.js로 만들어진 모듈을 관리하는 툴이니 npm을 사용한다는 것은 곧 모듈을 활용한다는 것
  - 활용하는 방법은 간단히 "npm install 모듈이름" 의 명령어로 가능
  - 예를 들어 webpack를 설치해본다면
    `npm install --g webpack`
    으로 설치를 하게된다.
  - 이렇게 일일히 설치할 수도 있겠지만 모듈의 의존성을 한꺼번에 관리하는 것이 더 효율적이므로
    json 파일을 만들어 그 안에 기록을 통해서 관리를 한다.
    - 먼저 json 파일을 생성
      `npm init`
    - package.json파일이 생성
      ```
      {
        "name": "npm study",
        "version": "0.1.0",
        "private": true,
        "dependencies": {
          "@testing-library/jest-dom": "^5.14.1",
          "@testing-library/react": "^11.2.7",
          "@testing-library/user-event": "^12.8.3",
          "web-vitals": "^1.1.2"
        },
        "scripts": {
          "start": "react-scripts start",
          "build": "react-scripts build",
        },
        "browserslist": {
          "production": [
            ">0.2%",
            "not dead",
            "not op_mini all"
          ],
          "development": [
            "last 1 chrome version",
            "last 1 firefox version",
            "last 1 safari version"
          ]
        }
      }
      ```
    - 여기서 중요한 부분은 "scripts" 와 "dependencies"
      - script는 우리가 run 명령어를 통해서 실행할 것들을 적어두는 것
      - dependencies의 경우는 설치할 모듈들을 의미 (npm install -g webpack 같이 설치를 하면 자동으로 기록)
    - 이렇게 package.json 파일이 정리가 되면 배포를 해야 할 때 파일만 같이 배포를 한다면
      해당 프로그램 개발에 사용되었던 모듈을 그대로 인스톨할 수 있게 된다
      `npm install` 배포된 파일을 다운받고 이 명령어를 입력하게 되면 모듈을 한번에 다운받을 수 있게 된다
    - scipts 안의 내용을 실행하기 위해서는 'npm run 이름' 같은 형식으로 실행할 수 있다
      - 예를 들어서 webpack으로 해당 파일을 빌드하기 위해서는 다음과 같이 입력한다
        ``npm run build`
      - 그리고 http-server를 동작시키기 위해서 아래와 같이 입력한다
        `npm run http-server`
