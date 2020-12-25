# Natours

## 2020.12.25

- 대규모 CSS 시스템을 위한 SCSS 도입 및 아키텍처

  - abstracts
    - mixins
    - functions
    - variables
  - base
    - animation
    - base
    - typography
    - utilities
  - components
  - layout
  - pages
  - index

  각 scss 역할에 따라 분리합시다. 분리한 다음 `@import`를 통해 조합하여 사용합니다. 이를 통해서 재사용성과 유지보수성을 올릴 수 있습니다.

- 레이아웃
  레이아웃은 CSS에서 매우 중요합니다. 결국 우리는 디자이너가 제공한 시안을 적절히 배치하는게 대부분이기 때문이죠. 레이아웃을 만드는 다양한 방법이 존재합니다.

  1. float
  2. flexbox
  3. grid

  1번 방법은 가장 전통적인 방법입니다. 이 방법에는 한계점이 있지만 오랜 기간 연구된 끝에 많은 어려움을 해결하는 방법이 알려져 있습니다. 그리고 1번 방법은 대부분 브라우저에서 작동합니다. 2, 3번 방법은 레이아웃을 위한 전용 CSS 속성입니다. 1번 방법에서 구현하기 힘든 것들을 손쉽게 개발할 수 있습니다. 그렇다면 2, 3번 방법을 사용하면 될까요? 안타깝게도 2, 3번 방법은 현대적인 브라우저에서만 작동합니다. 그래서 범용적인 웹을 만들기 위해 아직까지 1번 방법을 많이 사용합니다. 하지만 웹은 빠르게 발전하고 있고 마이크로소프트에서 IE에 대한 지원 중단을 선언하면서 빠른 시간내에 우리는 2, 3번 방법으로도 많은 사용자를 충족시킬 수 있을 것입니다. 2, 3번 방법은 개발자에게도 매우 유용합니다.

## 2020.12.24

- SCSS 연습해보기(변수, 중첩, 믹스, 확장 및 기능)

  ```html
  <nav>
    <ul class="navigation">
      <li><a href="#">About us</a></li>
      <li><a href="#">Pricing</a></li>
      <li><a href="#">Contact</a></li>
    </ul>
    <div class="button">
      <a class="btn-main" href="#">Sign up</a>
      <a class="btn-hot" href="#">Get a quote</a>
    </div>
  </nav>
  ```

  ```scss
  * {
    margin: 0;
    padding: 0;
  }

  $color-primary: #f9ed69; // yellow color
  $color-secondary: #f08a5d; // orange
  $color-tertiary: #b83b5e; // pink
  $color-text-dark: #333;
  $color-text-light: #eee;

  $width-button: 150px;

  @mixin clearfix {
    &::after {
      content: "";
      display: none;
      clear: both;
    }
  }

  @mixin style-link-text($color) {
    text-decoration: none;
    text-transform: uppercase;
    color: $color;
  }

  @function divide($a, $b) {
    @return $a / $b;
  }

  /* comment */

  nav {
    margin: divide(60, 2) * 1px;
    background-color: $color-primary;
    @include clearfix;
  }

  .navigation {
    list-style: none;
    float: left;

    li {
      display: inline-block;
      margin-left: 30px;

      &:first-child {
        margin: 0;
      }

      a:link {
        @include style-link-text($color-text-dark);
      }
    }
  }

  .buttons {
    float: right;
  }

  %btn-placeholder {
    padding: 10px;
    display: inline-block;
    text-align: center;
    border-radius: 100px;
    width: $width-button;
    @include style-link-text($color-text-light);
  }

  .btn-main {
    &:link {
      @extend %btn-placeholder;
      background-color: $color-secondary;
    }
    &:hover {
      background-color: darken($color-secondary, 15%);
    }
  }

  .btn-hot {
    &:link {
      @extend %btn-placeholder;
      background-color: $color-tertiary;
    }
    &:hover {
      background-color: lighten($color-tertiary, 10%);
    }
  }
  ```

- Mixin과 Extend 차이점에 대해서 알아보기

## 2020.12.23

- `rem`과 `em`의 차이에 대해서 알기
- 왜? html 스타일에 62.5%의 `font-size`를 지정하는걸까?
  대부분의 브라우저 기본 폰트 사이즈는 16px 이고 16px의 62.5%는 10px이다. 10px로 설정하면 rem을 설정할 때 계산하기 편하다. 그렇다면 명시적으로 10px을 주면 되는데 왜 굳이 62.5%를 주는걸까? 이유는 사용자 환경에 따라 폰트 사이즈가 제각각이기 때문이다. 어떤 사용자는 브라우저를 확대해서 보거나 축소해서 본다. 그런데 우리가 폰트 사이즈를 명시적으로 10px로 설정하면 사용자는 확대/축소 기능의 이점을 가질 수 없다. 사용자를 위한 배려라고 생각하자.

  > 하지만 사용자의 설정을 존중해야 한다는 입장의 개발자, 그러니까 62.5%를 쓰는게 더 좋다고 말하는 개발자들은 이 설정이 보기엔 큰 차이가 없지만 사용자의 접근성 옵션(accessibility options)을 해친다고 말한다. 글씨가 작아 기본 폰트를 키워 쓰던 사람이 10px로 사용자 브라우저의 기본 폰트를 바꾸면 그 설정에 맞게 동작하지 않고 강제로 10px 설정된 상태에서 사용해야 하기 때문에 불편하다는 것이다.

- 구조화되고 재사용 가능한 CSS를 작성하기 위해 패턴 혹은 아키텍처 도입을 고려해보자. 대표적인 아키텍처는 `BEM`이 있다.
  독립적인 영역을 `BLOCK`이라고 하며 `BLOCK`은 `ELEMENT`로 구성되어있다. `ELEMENT`는 `BLOCK`내에서 종속적이다. `MODIFIER`는 `BLOCK`과 `ELEMENT`의 성질을 의미한다. BEM은 CSS를 구조화할 수 있고 각 영역을 독립적으로 관리할 수 있지만 불필요하게 네이밍이 길어지는 단점이 있다.

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
