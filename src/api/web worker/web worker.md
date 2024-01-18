### reference
- https://developer.mozilla.org/ko/docs/Web/API/Web_Workers_API
- https://tech.kakao.com/2021/09/02/web-worker/
- https://blog.rhostem.com/posts/2021-01-03-image-load-by-web-worker
- https://rhostem.github.io/image-load-worker/
## 필요성
- 스크립트 연산을 웹 어플리케이션의 주 실행 스레드와 분리된 별도의 백그라운드 스레드에서 실행할 수 있는 기술. 
- 무거운 작업을 분리된 스레드에서 처리하면 주 스레드(보통 UI 스레드)가 멈추거나 느려지지 않고 동작할 수 있습니다.
## 선언
```javascript
const myWorker = new Worker("worker.js");
```

## 메시지 시스템
- 워커와 메인 스레드 간의 데이터 교환은 메시지 시스템을 사용
```javascript
first.onchange = () => {
  myWorker.postMessage([first.value, second.value]);
  console.log("Message posted to worker");
};

second.onchange = () => {
  myWorker.postMessage([first.value, second.value]);
  console.log("Message posted to worker");
};
```

## self 
- self 를 통해 Worker 객체 스스로에게 들어오는 message를 Listen할 수 있게 만들 수 있다.
```javascript
self.addEventListener('message', e => {
  const { repeatCount } = e.data;

  const startTime = new Date().getTime()
  
  for (let i = 0; i < repeatCount; i++) {
    somethingComplexJob();
  }
  
  const runTime = new Date().getTime() - startTime

  postMessage({
    runTime
  });
});
```

    