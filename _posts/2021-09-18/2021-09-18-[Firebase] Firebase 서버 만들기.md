## 파이어베이스 서버 만들기

파이어베이스 서버를 만들기로 결심했다. 나만의 앱을 만들어서 풀스택으로 뭔가 하나 만드는게 목표가 되었다. 완성은 2022년이 끝나기 전까지로 기간을 잡았다.
- 벌써 연간목표를 잡아야 할 시기가 슬슬 다가오는것 같다.

이제 시작해보자.
- 참조: 에디트툴은 VisualStudioCode를 사용하였다.

## 1. npm 설치하기
[node.js 사이트](https://nodejs.org/ko/)
Node.js®는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다.

위 사이트에서 Node.js를 다운 받는다.

## 2. firebase-tools 설치
아래와 같은 명령어로 firebase-tools을 설치한다.
```
iOS_Server % npm install -g firebase-tools
```

npm install 에러시 아래 링크 내용을 참고해보면 도움이 될것 같다.
[2021-09-18-[npm] npm cli 명령어 오류 발생시](https://magpiebros.github.io/npm-npm-cli-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%98%A4%EB%A5%98-%EB%B0%9C%EC%83%9D%EC%8B%9C/)

## 3. firebase 명령어 동작 안함

아래 명령어들 실행해서 조치. 걸리는 부분이 꽤 많네…
```
iOS_Server % firebase
zsh: command not found: firebase

Firebase tolls upgrade
iOS_Server % curl -sL https://firebase.tools | bash

```

### 3-1. firebase 명령어 동작안할때 처리 방법

npm을 사용해 Firebase 설치하면 정상 동작한다.
[npm을 사용해아 Firebase 설치] (https://firebase.google.com/docs/web/setup?authuser=0)

firebase 명령어가 정상 동작을 하지 않을 경우 아래 명령어로 firebase를 다시 설치해준다.

```
iOS_Server % npm install firebase  
```

### 4. 설정한다.
다음과 같은 명령으로 firebase 설정을 마무리한다.
생각보다 오래 걸렸다..

```
firebase login
firebase init
```

## 마치며
이제 서버 개발을 위한 초석은 닦아놓았다고 생각한다. 이제 더 깊게 들어갈 준비를 하자.


