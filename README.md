# Natours

## 2020.12.22

- 버튼에 잔상이 남는 효과 만들기, `css/style.css` `.btn-white` 참고하기
- `animation-fill-mode: backwards;`
- 브라우저가 HTML가 CSS를 읽는 단계

  1. html를 로드합니다.
  2. html을 파싱합니다.
  3. 파싱하다가 중간에 <link>를 만나면 브라우저를 웹 서버에 css를 요청합니다.
  4. 응답 받은 css를 읽습니다. CSS 파싱을 합니다.
  5. 파싱 하면서 명시도 계산, 값 계산을 합니다. 이 결과 CSSOM이 만들어집니다.
  6. CSS를 읽은 후 HTML을 마저 파싱합니다. HTML을 읽은 후 DOM Tree를 구성합니다.
  7. 앞서 만든 DOM Tree와 CSSOM을 이용하여 Render Tree를 만듭니다.
  8. 화면 배치 및 그립니다.

- 명시도
  다양한 선택자(selector)를 이용하여 CSS 속성을 설정하다보면 예상치 못하게 스타일이 적용되지 않을 때가 있습니다.
  그때는 명시도가 낮음을 의심해봅시다.
  CSS 선택자를 분석하여 명시도를 알아낼 수 있습니다. 명시도에 따라 선택되는 CSS 스타일이 다릅니다.
  아래 설명에서 숫자가 낮을수록 명시도가 높습니다.
  1. `!important`가 명시도가 가장 높습니다.
  2. `inline style` 태그에 style을 직접 주는 경우 명시도가 높습니다.
  3. id
  4. class, pseudo-classes, attribute
  5. elements, pseudo-elements

## 2020.12.21

- `a tag`의 가상 선택자 `:link`와 `:visited`가 있다.

- 가상 선택자로 `:hover`, `:active`도 있다.

- 자식 요소가 `inline-block`이라면 부모 요소의 `text-align`에 영향을 받는다. 이를 활용하여 수평 중앙 정렬을 할 수 있다.

- 요소 가상 선택자에 animation이나 transform을 설정한 후 요소에 transition을 설정해두어야 스무스하게 효과가 나타난다.

- animation

  ```css
  animation-name: moveInLeft;
  animation-duration: 1s;
  animation-timing-function: ease-out;
  animation-iteration-count: 3;
  animation-delay: 3s;
  @keyframes moveInLeft {
    0% {
      opacity: 0;
      transform: translateX(-100px);
    }

    80% {
      transform: translateX(10px);
    }

    100% {
      opacity: 1;
      transform: translateX(0);
    }
  }
  ```

  애니메이션을 적용하고 요소를 자세히보면 끝나는 시점에 조금 흔들리는 것을 볼수 있습니다. 이를 해결하려면 감싸는 부모 요소에 `backface-visibility: hidden;`를 설정합시다. 강좌가 2년전 강좌라 현재 시점에서는 필요 없을 수도 있습니다.

- 부모를 기준으로 자식 요소를 이동하고 싶다면 부모 요소에 `position: relative`, 자식에 `position: absolute`를 지정하면 된다.

- `text-transform` 요소내 텍스트를 변형시킨다. 예를 들어 소문자인 내용을 대문자로 바꾼다거나.

- `letter-spacing` 문자 사이의 수평 간격을 지정할 수 있다. 값이 클수록 벌어진다.

- `transform: translate(-50%, -50%);`를 사용하여 요소를 중간에 위치하게 만들 수 있다.

## 2020.12.20

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
