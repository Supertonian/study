# 3. 노드 기능 알아보기

## REPL 사용하기

**R**ead **E**val **P**rint **L**oop

## CommonJS vs ECMAScript

ECMAScript 모듈이 코드가 간소하고 그 외에는 안되는 기능들이 더 많음.
(Dynamic import 불가능, __filename, __dirname 불가능, index.js 생략 불가능)

ECMAScript에서도 dynamic import가 가능함.

```jsx
const m1 = await import('./func.mjs');
console.log(m1);
const m2 = await import('./var.mjs');
console.log(m2);
```

## 노드 내장 객체 알아보기

### global

전역 객체이며 생략도 가능

console 객체도 원래는 global.console 이다.

전역 객체라는 점을 이용해 파일 간에 간단한 데이터를 공유할 때 사용하기도 함. ← **남용 금지**

### console

- console.time(label)
    - console.timeEnd와 대응되어 같은 레이블 사이의 시간을 측정
- console.table(배열)
    - 배열의 요소로 객체 리터럴을 넣으면 테이블 형식으로 출력

### timer

setImmediate(callback), setTimeout(callback, 0)의 차이?

→ 파일 시스템 접근, 네트워킹 같은 I/O 작업의 콜백 함수 안에서 타이머를 호출하는 경우는 setImmediate가 먼저 실행

→ 하지만 항상 그렇지않고 보장되지않음. setImmediate을 더 권장함.

### process

실행하는 환경마다 다르겠지만 프로세스 정보 출력 가능.

```jsx
process.version
process.arch
process.platform
process.pid
process.uptime()
process.execPath
process.cwd()
process.cpuUsage()
```

- process.env
    - 시스템의 환경 변수
- process.nextTick(callback)
    - 이벤트 루프가 다른 콜백 함수들보다 nextTick의 콜백 함수를 우선으로 처리하도록
    - Promise와 함께 **microtask**라고도 불림
    - microtask를 재귀함수로 부르면 먼저 부르기 때문에 중간에 있는 콜백 함수들이 불리지 않을 수 있음.
- process.exit()
    - exit node

## 노드 내장 모듈 사용하기

### os

→ 운영체제별 다른 서비스를 제공하고 싶을 때 사용

### path

윈도우와 리눅스 경로 차이를 쉽게 해결해줌.

path.join(path, …) vs path.resolve(path, …)

앞에 / 유무에 따라 join은 상대경로로 resolve는 절대경로로 인식.

### url

```jsx
const myURL = new URL('http://www.gilbut.co.kr/?page=1');
```

WHATWG 기준으로 분석을 해줌.

### dns

→ 도메인을 통해 IP나 기타 DNS 정보를 얻고자 할 때

### crypto

PBKDF2: salt라는 문자열을 붙이고 반복을 여러번 해 암호화

bcrypt, scrypt를 많이 사용함.

### util

util.deprecate를 통해 deprecate될 예정이라고 알려줄 수 있음.

### worker_threads: multi-threading

8개를 띄운다고 8배 빨라지지않는다. (매우 잘짜면 가능할수도..? 그것보다 못짜서 느려질 가능성이 매우 높을듯)

postMessage를 통해 부모와 자식과 메시지 주고 받을 수 있다.

### child_process: multi-processing

**spawn**은 새로운 프로세스를 띄우면서 명령어를 실행.

```jsx
const process = spawn('python', ['test.py']);

process.stdout.on('data', function(data) {
	console.log(data.toString());
});
```

### 파일 시스템 접근하기

Promise로 파일 읽고 쓰기 가능

```jsx
const fs = require('fs');

fs.writeFile('./writeme.txt', 'writing something', err => {
	if (err) {
		throw err;
	}
	fs.readFile('./writeme.txt', (err, data) => {
		if (err) {
			throw err;
		}
		console.log(data.toString());
	});
});
```

읽으면 Buffer이기 때문에 toString()으로 변환해줘야한다.

### 동기 메서드와 비동기 메서드

- 동기와 비동기: 백그라운드 작업 완료 확인 여부
- 블로킹과 논블로킹: 함수가 바로 return되는지 여부

### Buffer와 Stream 이해하기

Stream은 동영상같은 용량이 큰것을 chunk로 나눠서 보낼 수 있어 적은 메모리를 통해 큰 파일을 만들수 있다.

### 기타 fs

```jsx
fs.access(...)
fs.mkdir(...)
fs.open(...)
fs.rename(...)
```

### 스레드 풀

fs외에도 내부적으로 스레드 풀을 사용하는 모듈이 있음.

스레드 풀 개수만큼 작업을 동시에 처리함.

### Event

이벤트를 직접 만들수도 있음.

addListener == on

removeListener == off

```jsx
const EventEmitter = require('events');

const myEvent = new EventEmitter();
myEvent.addListener('event', () => {
	console.log('event 1');
});
myEvent.on('event2', () => {
	console.log('event 2');
});
```

### Error handling

에러를 throw하면 노드 프로세스가 멈춤.

throw하는 경우는 반드시 try catch문으로 throw한 에러를 잡아야 함.

```jsx
process.on('uncaughtException', err => {
	console.error('uncaught error', err);
});
```

uncaughtException으로 모든 에러를 처리할 수 있을것 같지만 노드 공식 문서에서는 최후의 수단으로 사용할것을 명시하고 있음.

---

# Todo

- async, await 한번 더 해보기
    - Error handling in async, await
- javascript로 multi-threading, multi-processing 예제 더 찾아보기
- Event-loop, single-thread 한번 더 보기
