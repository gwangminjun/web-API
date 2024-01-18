### reference 
- https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API
- https://velog.io/@elrion018/%EC%8B%A4%EB%AC%B4%EC%97%90%EC%84%9C-%EB%8A%90%EB%82%80-%EC%A0%90%EC%9D%84-%EA%B3%81%EB%93%A4%EC%9D%B8-Intersection-Observer-API-%EC%A0%95%EB%A6%AC

## 필요성
-  getBoundingClientRect
   - 정의
     - JavaScript 에서 DOM 요소의 크기와 위치 정보를 얻기 위한 메서드
   - 이유
     - 본래 브라우저는 최적화를 위해 필요한 여러 작업을 묶어 큐에 쌓아 대기하다가 한 번의 reflow [@](../modern/draw.md) 로 처리하고자 합니다. 
     - getBoundingClientRect [@](../modern/Element/getBoundingClientRect.md) 호출시 값(top, right 등)을 정확히 읽어들이기 위해 <br> 
       queue 를 flush ( 큐(queue)를 초기화하거나 비워내는 작업) 하고 스타일을 적용함으로써 다수의 reflow 를 발생
- 기존의 scroll event
  - 스크롤시 짧은 시간 내에 수 백, 수 천의 이벤트가 동기적으로 실행

## 교차관찰
- 대상 요소
  - 기기의 뷰포트 또는 특정 요소와 교차합니다.
- 특정 요소
  - Intersection Observer API 의 목적에 따라 루트 요소 또는 루트라고 불립니다.
- Intersection Observer API
  - Intersection Observer API 는 특정 요소와 대상 요소의 교차점을 관찰합니다.
  - 타겟 요소가 루트 요소와 교차하는지 아닌지를 구별하는 기능을 제공
  - scroll 이벤트와 다르게 교차 시 비동기적으로 실행되며 교차성(가시성) 구분 시 reflow 를 발생시키지 않습니다.
- 계산
  - Intersection Observer 는 모든 영역을 사각형(rectangle)로 취급합니다. 
  - 요소가 사각형이 아니거나, 이외 이상하고 불규칙한 모습으로 렌더링되었다고 하더라도, <br> 요소의 모든 부분을 감싸는 가장 작은 사각형으로 가정하고 교차성(가시성)을 계산

## 사용
```javascript
// 옵션 생성
let options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};

//선언
let observer = new IntersectionObserver(callback, options);

// 타겟 요소 관찰 시작
let target = document.querySelector('#listItem');
observer.observe(target);

```

## 옵션
### root
- 대상 가시성을 체크하기 위한 뷰포트로 사용되는 요소. 
- 반드시 타겟의 상위 요소이어야 합니다. 
- 만약 뷰포트를 지정하지 않거나 null 이면 브라우저 뷰포트가 기본으로 설정됩니다.

### rootMargin
- 루트 주위의 여백입니다. 
- margin 을 주어 루트 요소의 범위를 확장할 수 있습니다
- 기본 값은 0입니다.

### threshold
- 관찰자의 콜백이 무조건 실행되어야 하는 대상의 가시성 백분율을 나타내는 숫자 또는 숫자 배열입니다.
- 어느정도 타겟 요소가 보여졌는 지에 따라서 콜백을 호출
- 기본 값은 0 입니다. 

## callback

### Entries
- entries 는 IntersectionObserverEntry [@](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry) 인스턴스를 담은 배열입니다. 
- 일반적으로 callback 에 파라미터로 전달
- 후술할 Intersection Observer.takeRecords() 를 통해 반환받을 수도 있습니다.

#### Methods
- IntersectionObserverEntry.isIntersecting 
  - 해당 entry 에 타겟 요소가 루트 요소와 교차하는 지 여부를 Boolean 값으로 반환합니다.

### 유의사항
- 타겟 요소와 루트 요소가 전혀 교차하지 않았음에도 불구하고 타겟 요소의 관찰이 시작되면 콜백 또한 바로 호출 
- 이는 Intersection Observer 의 기본동작입니다. 
- 예외처리(intersectionRatio 사용)
```javascript
let callback = (entries, observer) => {
  entries.forEach(entry => {
	// 타겟 요소가 루트 요소와 교차하는 점이 없으면 콜백을 호출했으되, 조기에 탈출한다.
	if (entry.intersectionRatio <= 0) return

	// 혹은 isIntersecting을 사용할 수 있습니다.
	if (!entry.isIntersecting) return

	// ... 콜백 로직
  });
};
```

## Methods
- **IntersectionObserver.observe(targetElement)** 
  - 타겟 요소에 대한 관찰을 시작합니다.
- **IntersectionObserver.unobserve(targetElement)**
  - 타겟 요소에 대한 관찰을 중지합니다.
- **IntersectionObserver.disconnect()**
  - 인스턴스의 타겟 요소들에 대한 모든 관찰을 중지합니다. 
- **IntersectionObserver.takerecords(targetElement**)
  - IntersectionObserverEntry 인스턴스들의 배열을 리턴합니다. 

## useless

### 지연 로딩(Lazy Loading)
- 타겟 요소와 루트 요소가 원하는 임계점만큼 교차했을 때 이미지 표츌
- [codepen](https://codepen.io/ooxhapqg-the-reactor/pen/QWopdXY)

### 무한스크롤
- 타겟 요소와 루트 요소가 원하는 임계점만큼 교차했을 때 새로운 item 들을 추가해줍니다.
- - [codepen](https://codepen.io/ooxhapqg-the-reactor/pen/ZEPeezz)

