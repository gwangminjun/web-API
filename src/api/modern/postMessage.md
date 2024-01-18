### reference
- https://developer.mozilla.org/ko/docs/Web/API/Window/postMessage
- https://luckygg.tistory.com/174

## 개요
- Window 오브젝트 사이에서 안전하게 cross-origin 통신을 할 수 있게 합니다.

## 원형
```
BOOL PostMessageA(
  HWND   hWnd,
  UINT   Msg,
  WPARAM wParam,
  LPARAM lParam
);
```

## 구조
- 호출된 메시지가 메시지 큐에 들어가며, 윈도우 프로시저에서 이 메시지를 처리
- 시지가 즉각 처리되는 것이 아니라 GetMessage()에 의해 해석된 메시지가 DispatchMessage()에 의해 윈도우 프로시저로 전달되어 처리

## sendMessage 와의 차이
- SendMessage
    - 윈도우 프로시저를 직접 호출하며, 프로시저가 메시지를 처리할 때 까지 반환하지 않는다.
    - 순차적으로 처리(sequentially)
    - 동기 방식(synchronous)
- PostMessage
    - 메시지 큐에 메시지가 삽입되며, 윈도우 프로시저에서 메시지를 처리한다. 해당 메시지가 언제 처리될 지 예측이 어렵다.   
    - 비 순차적으로 처리(not sequentially)
    - 비동기 방식(asynchronous)








 