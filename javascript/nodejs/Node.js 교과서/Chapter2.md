# 2. 알아둬야 할 자바스크립트

### Promise

Promise.all 대신 Promise.allSettled 사용해야 결과 상태로 어떤것이 reject인지 알 수 있음.

Node16부터는 Promise에 catch를 필수로 사용해야함.

### async/await

Promise를 순차적으로 실행하는 방법

```jsx
const promise1 = Promise.resolve('success1');
const promise2 = Promise.resolve('success2');
(async() => {
	for await (promise of [promise1, promise2]) {
		console.log(promise);
	}
})();
```

### Map/Set

Map은 속성들 간의 순서를 보장하고 반복문을 사용할 수 있음. size 메서드를 통해 숫자 알 수 있음.

Set은 중복을 허용하지 않는다.
