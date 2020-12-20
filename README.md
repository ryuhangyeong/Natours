# Natours

## 20200.12.20

새롭게 알게된 것

- linear-gradient()
  그라디데이션을 만들기 위한 속성

  ```css
  background-image: linear-gradient(
    to right bottom,
    rgba(126, 213, 111, 0.8),
    rgba(40, 180, 131, 0.8)
  );
  ```

- clip-path
  배경 화면내에서 다각형 및 모형을 만들기 위한 속성
  왼쪽/위를 기준으로 시계방향으로 4개의 점을 움직여서 모양을 만든다.

  ```css
  clip-path: polygon(0 0, 100% 0, 100% 75vh, 0 100%);
  ```

- background-position
  이미지의 고정 영역 설정한다. top이면 화면 크기가 작아지면 위의 영역 이미지는 고정되고 top 이외의 영역이 잘려나간다.

  ```css
  background-position: top;
  ```
