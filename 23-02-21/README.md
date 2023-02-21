# express로 구성하는 간단한 서버앱 환경설정

패키지 셋팅
```shell
npm init -y
```

typescript 설치 및 셋팅
```shell
npm i -D typescript
npx tsc --init
```

ESLint
```
npm i -D eslint
npx eslint --init
```

ts-node 설치
```shell
npm i -D ts-node
```

Express 설치
```shell
npm i express
npm i -D @types/express
```

간단한 예제
```ts
import express from 'express';

const port = 3000;

const app = express();

app.get('/', (req, res) => {
	res.send('Hello, world!');
});

app.listen(port, () => {
	console.log(`Server running at http://localhost:${port}`);
});
```

ts-node로 실행
```shell
npx ts-node app.ts
```

코드를 수정되는 것을 인식해 자동으로 재실행 시켜주는 nodemon 설치
```shell
npm i -D nodemon
npx nodemon app.ts
```
