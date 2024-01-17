### reference
https://beomy.github.io/tech/browser/reflow-repaint/

## Repaint(Redraw)
- 정의
    - 화면에 변화가 있을 때 화면을 그리는 과정 
- 구조
    - 화면의 구조가 변경되었을 때에는 Reflow 과정을 거쳐 화면 구조를 다시 계산한 후 <br>
    Repaint 과정을 통해 화면을 다시 그립니다. <br>
- 화면의 구조가 변경 시 
  - Reflow 와 Repaint 모두 발생합니다.
- 화면의 구조가 변경되지 않는 화면 변화
  - Repaint 만 동작
  - opacity, background-color, visibility, outline 등의 스타일 변경

## Reflow(Layout)
- 정의
  - 화면 구조(Layout)이 변경되었을 때, 뷰포트 내에서 렌더 트리의 노드의 정확한 위치와 크기를 계산하는 과정


## 발생하는 경우
  - DOM 노드의 추가, 제거
  - DOM 노드의 위치 변경
  - DOM 노드의 크기 변경(margin, padding, border, width, height 등..)
  - CSS3 애니메이션과 트랜지션
  - 폰트 변경, 텍스트 내용 변경
  - 이미지 크기 변경
  - offset, scrollTop, scrollLeft과 같은 계산된 스타일 정보 요청
  - 페이지 초기 렌더링
  - 윈도우 리사이징