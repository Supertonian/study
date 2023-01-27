# 4장. http 모듈로 서버 만들기

## 4.1. 요청과 응답 이해하기

### fs

fs.readFile을 통해 따로 html을 만들고 그걸 읽고 리턴 해주는 방식

```jsx
http.createServer(async (req, res) => {
	try {
		const data = await fs.readFile('./server2.html');
		res.writeHead();
		res.end(data);
	} catch (err) {
		console.error(err);
		res.writeHead();
		res.end(err.message);
	}
})
.listen(8080, () => {
	console.log('listening to port 8080');
});
```

## 4.2. REST와 라우팅 사용하기

REST API

GET, POST, PATCH, PUT, DELETE

req.method로 GET만 받을 수 있음.

req.url로 주소 routing 나눠주기.

```jsx
http.createServer(async (req, res) => {
	try {
		if (req.method === 'GET') {
			if (req.url === '/') {
				const data = await fs.readFile('./server2.html');
				res.writeHead();
				res.end(data);
			} else if (req.url === '/about') {

			}
		}
	} catch (err) {
		console.error(err);
		res.writeHead();
		res.end(err.message);
	}
})
.listen(8080, () => {
	console.log('listening to port 8080');
});
```

POST로 결과를 받는것은 streaming으로 받기 때문에

받은 문자를 한 곳에 더해주고 JSON.parse를 해야 정확하게 클라이언트에서 요청한 값을 저장할수 있음.

### 4.3. 쿠키와 세션

쿠키: 키 = 값

set-cookie로 서버에서 클라이언트로 보내면

클라이언트에서 이후 새로고침으로 서버에 보내면 자동으로 쿠키를 더해서 요청한다.

쿠기를 서버에서 받으면 키=값에 ;로 구분되기 때문에 ; 기준으로 분리해서 객체로 저장하는 과정이 필요하다.

Set-cookie에 httponly를 적어주면 자바스크립트에서 이 쿠키에 접근이 불가능하다.

### 4.4. https, http2

https는 다른 모듈로 필수로 cert, key, ca도 넣어줘야함.

http2는 https처럼 보안 정보 그대로 넣어줘야함. 왔다갔다 횟수가 다르기 때문에 http1.1에 비해 매우 빠름. 특히 작은 이미지 파일을 페이지에서 많이 불러오는 경우

### 4.5. 클러스터

프로세스를 여러개 만드는것

클러스터를 통해서 하면 하나의 포트에 여러개의 워커로 서버를 띄울수 있음.

Round robin 알고리즘을 통해 분배해줌.
