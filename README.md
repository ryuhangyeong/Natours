# Natours

## 2021.01.15

- 반응 형 이미지의 목표는 작은 화면에 불필요한 큰 이미지를 다운로드하지 않도록 올바른 이미지를 올바른 화면 크기와 장치에 제공하는 것입니다.
- 1MB 이미지가 있다고 가정하자. 데스크톱에서는 해당 이미지를 사용하는건 상관없지만 모바일에서 사용하는 경우 느리거나 빠른 데이터 소모를 초래할 수 있습니다.(그래서 데스크톱 이미지와 모바일용 이미지가 필요하다.)
- 세 가지 사용 사례

  - 해상도 전환
    Large screen -> Small screen (작은 화면에서 이미지 해상도 감소)
  - 밀도 전환
    @2x screen (high-res) -> @1x screen (low-res)
    @1x 화면에서 이미지 해상도의 절반
  - 아트 방향
    Large screen -> Small screen (작은 화면에서 다른 이미지)

  이렇게 보았을 때는 잘 모르겠네.. 실제 예제를 보면서 이해해보자!

- HTML의 반응형 이미지 - 아트 방향 및 밀도 전환

  - 밀도 설명자와 함께 <img> 및 <source> 요소의 srcset 속성에 대한 방법

  ```scss
  <footer class="footer">
    <div class="footer__logo-box">
      <img srcset="img/logo-green-1x.png 1x, img/logo-green-2x.png 2x" alt="Full logo" class="footer__logo">
    </div>
  </footer>
  ```

  https://heropy.blog/2019/06/16/html-img-srcset-and-sizes/

  - 아트 디렉션에 <picture> 요소를 사용하는 방법과 이유

  ```scss
  <footer class="footer">
  		<div class="footer__logo-box">
  			<picture>
  				<source srcset="img/logo-green-small-1x.png 1x, img/logo-green-small-2x.png 2x" media="(max-width: 37.5em)">
  				<img srcset="img/logo-green-1x.png 1x, img/logo-green-2x.png 2x" alt="Full logo" class="footer__logo">
  			</picture>
      </div>
  </footer>
  ```

  - HTML로 미디어 쿼리를 작성하는 방법.

## 2021.01.14

- `tous, story, book` 영역 반응형 지원 완료
- `sizzy`라는 멋진 도구도 알게 되었다. 월 $5이니까 비싼것도 아니다.
  웹 개발을 하다보면 기종간 테스트가 가장 골치아픈데 `sizzy`를 사용하면 100% 호환할순 없지만 많은 부분에서 테스트할 때 편리할 듯하다.

## 2021.01.11

- 앞서 우리가 작성한 `html` 부분의 font-size에 대한 미디어 쿼리에 대해 다시 한번 정리해봅시다.

  ```scss
  html {
    font-size: 62.5%; // 1rem = 10px, 10px/16px = 62.5%

    @include respond(tab-land) {
      // width < 1200?
      font-size: 56.25%; // 1 rem = 9px, 9/16 = 50%
    }

    @include respond(tab-port) {
      // width < 900?
      font-size: 50%; // 1 rem = 8px, 8/16 = 50%
    }
    /*
      0 - 600px: phone
      600 - 900px: tablet portrait
      900 - 1200px: tablet landscape
      [1200 - 1800] is where our normal styles apply
      1800px + : big desktop
  
      이해를 위해 다시 한번 살펴보자.
      700px이라고 가정해보자. 그러면 tab-land와 tab-port 두 쿼리 모두 적용됩니다. 모두 적용되
      므로 가장 마지막에 선언된 tab-port의 스타일이 적용될 것입니다.
  
      만약 tab-land와 tab-port의 순서를 반대로 했다면 tab-land의 font-size가 적용되어 레이아웃이
      엉망이 되었을 것입니다.
    */

    @include respond(big-desktop) {
      font-size: 75%; // 1 rem = 12px, 12/16 = 75%
    }
  }
  ```

## 2021.01.10

- 강력한 sass mixing을 사용하여 모든 미디어 쿼리를 작성하는 방법
- @content 및 @if sass 지시문을 사용하는 방법
- 반응 형 디자인을 위해 chrome devtools를 활용합니다.
- 종단점

  ```
  0 - 600px: phone
  600 - 900px: tablet portrait(세로)
  900 - 1200px: tablet landscape(가로)
  [1200 - 1800] is where our normal styles apply
  1800px + : big desktop
  ```

  ```scss
  /*
  $breakpoint argument choices:
  - phone
  - tab-port
  - tab-land
  - big-desktop
  */
  @mixin respond($breakpoint) {
    @if $breakpoint == phone {
      @media (max-width: 600px) {
        @content;
      }
    }

    @if $breakpoint == tab-port {
      @media (max-width: 900px) {
        @content;
      }
    }

    @if $breakpoint == tab-land {
      @media (max-width: 1200px) {
        @content;
      }
    }

    @if $breakpoint == big-desktop {
      @media (min-width: 1800px) {
        @content;
      }
    }
  }
  ```

  해당 부분을 들으면서 기본적인 미디어 쿼리 사용법이나 `mixin` 만드는 것을 이해하는데는 큰 어려움이 없었지만 근본적으로 **치밀한 전략**이 필요한 부분이구나라는 생각이 들었다. 지금까지는 그냥 `px`로 각 각 종단점에 `px`을 설정해주었는데. 강좌에서는 그렇지 않더라. 실제 현업에서는 도대체 어떻게 하는지가 참 궁금하다. 나도 현업에 있지만 개발 환경이 그렇게 좋지 못하고 주변 개발자가 없기 때문에 주먹구구식으로 했다.이번 기회에서라도 **제대로** 이해 해봐야겠다.

## 2021.01.08

- 반응형 전략은 현대 웹에서 매우 중요하다. 모바일 우선과 데스크톱 우선 전략이 있다.
- 미디어쿼리로 반응형 웹을 작성할 수 있으며 `media screen and (max-width: 100px)`와 같은 syntax를 가진다. 여기에서 주의해야할 것은 `max-width`는 이하를 의미하고 `min-width`는 이상을 의미한다는 것이다.
- 모바일 우선 전략의 장점

  - 모바일 경험에 100 % 최적화
  - 웹 사이트와 앱을 절대적인 필수 요소로 줄입니다.
  - 더 작고 빠르며 효율적인 제품을 만듭니다.
  - 바람직 할 수 있는 미적 디자인보다 콘텐츠를 우선시합니다.

- 모바일 우선 전략의 단점

  - 데스크톱 버전이 지나치게 비어 있고 단순하게 느껴질 수 있습니다.
  - 개발하기 더 어렵고 반 직관
  - 창작의 자유가 줄어들어 독특한 제품을 만들기가 더 어려워 짐
  - 클라이언트는 사이트의 데스크톱 버전을 프로토 타입으로 보는 데 사용됩니다.
  - 사용자는 모바일 인터넷을 사용합니까? 웹 사이트의 목적은 무엇입니까?

- 우리 프로젝트에 대한 종단점을 설정하는 것이 중요합니다. 그렇다면 어떤걸 기준으로 해야할까요?
  - ![종단점 기준](https://user-images.githubusercontent.com/27342882/103908070-6b511000-5145-11eb-9b7a-4a66a80749b2.PNG)
    가장 완벽한 방법은 **컨텐츠**를 기준으로 하는것입니다.
- 반응형 전략은 매우 복잡하고 고려해야할 사항이 많다. 프로젝트 규모나 성격에 따라 전략이 완전이 다를 것이다. 그러므로 추후에 반응형 웹을 도입할 때 [55. Mobile-First vs Desktop-First and Breakpoints]를 다시 보자.

## 2021.01.07

- `cubic-bezier`를 이용하여 속성 전환 방식을 변경할 수 있다.
- `table, tabel-cell`를 이용하면 수평, 수직 정렬을 손쉽게 제어할 수 있다.
  그 다음에 해당 속성이 유용한 경우는 부모 > 자식 관계에서 한쪽 자식의 높이에 영향을 받아 모든 자식 요소 높이를 동일하게 만들고 싶을 때 사용할 수 있다. 이 작업은 float로 표현할 수 없더라.(삽질로 이미 경험하였음)
- `column-count`를 이용하여 작성된 텍스트를 신문(?)처럼 표현할 수 있다.
- CSS hyphens 속성은 여러 줄에 걸치는 텍스트에서 단어에 붙임표를 추가하는 방식을 설정합니다.
- 새로운 속성을 많이 사용하는데 이전 브라우저 버전에서는 작동하지 않을수 있으므로 브라우저 접두사를 붙여야할 필요성이 있다. 이때 이 작업을 하나하나 수동으로 해주는 것보단 **autoprefixer**라는 도구를 쓰면 편하다.
- 토글링(숨겨지고 나타나는) 요소에 애니메이션 효과를 넣는 경우 아래 속성으로 숨기자.
  ```css
  opacity: 0;
  visibility: hidden;
  ```
- 와 이 강의는 정말 어메이징하다. 우리는 `#`를 통해서 문서내에서 이동할 수 있다. 예를 들어서 `<div id="move"></div>` 요소를 설정하고 `<a href="#move">이동</a>`을 누르면 `div#move`로 이동하게 된다. 그리고 `<div id="move"></div>`는 `:target`(의사 클래스)를 통해서 호출 여부를 판단할 수 있다. 이것을 통해서 js없이 css로만 팝업을 만들 수 있다. 팝업을 닫는 경우 `<a href="다른ID">`를 해주면 된다.

## 2021.01.06

- 체크박스(`:checked`)와 라벨을 활용하여 자바스크립트없이 네비게이션 영역을 토글하는 방법에 대해 배우게 되었다. 이 트릭은 빈번히 사용된다.
- 추가적으로 네비게이션이 펼쳐질때 작은 버튼에서 시작하여 전체 화면을 모두 꽉 차게하는 애니메이션에 대해서 배우게 되었다. 솔찍히 이 방법을 보고 감탄했다. 이렇게 하는거구나 하고 말이다.
- 그리고 그라디레이션을 활용한 hover에 대해서 배우게 되었다. 기울게 그라디레이션을 안보이는 영역에서 준비해두었다가 hover되면 크기를 늘려서 사용자에게 보여주는 것이다. 이것은 애니메이션 응용에 해당된다.
- 도대체 누가 CSS가 쉽다는거지? 배울 때마다 새롭고 이런 방법이 있다는 것에 항상 놀란다.
- 이 방법은 다양한 곳에서 응용될 수 있을 듯 하다.

## 2021.01.03

- 상속(inherit)를 적극 사용하라. 부모의 색상, 글꼴에 영향을 받지 못하는 태그들이 있는데. 하나하나 값을 지정해주기보단 `inherit`를 사용하여 부모로부터 상속을 받자. 생각해보면 지금까지 이런 부분들을 많이 놓치고 있었다.
- `&::-webkit-input-placeholder`를 사용하여 `placeholder`에 스타일을 지정할 수 있다. 단, 크롬과 사파리에서만 적용.
- `&:focus:invalid`를 사용하여 유효성 검사가 잘못된 경우 스타일을 지정할 수 있다.
- `&:placeholder-shown` placeholder가 보일 때 스타일 지정
- `radio` 버튼은 css로 커스텀할 수 없다. 그래서 주로 라디오를 대체할 이미지를 사용하거나 기존 input을 지우고 대체 요소로 라디오를 대체한다. 이때 label과 radio를 응용한다. for과 id가 같으면 label을 누르는 경우 radio가 작동한다. radio가 check되어있으면 `:checked`가 실행되고 이를 활용하여 보이지 않던 커스텀 라디오 버튼을 보여주거나 하면 된다. 이는 매우 자주 활용된다.
- helper css는 `!important`로 명시도를 가장 높게 올려주자. 그래야 설정의 효과가 있다.

## 2021.01.02

> 거의 다 끝나간다! 힘을 내자!

- 영역 전체에 비디오 화면을 넣고 싶으면 `<video>` 내부에 영역을 넣으면 된다.
- **coverr**에서 웹사이트에 넣을만한 비디오 화면을 찾을 수 있습니다.
- `autoplay muted loop`과 같은 속성으로 비디오를 제어할 수 있습니다.
- `object-fit` 속성은 대체되는 요소의 내용(<img>, <video>, <object>, <svg> 등과 같은)이 지정된 너비와 높이에 맞게 장착되는 방식을 지정한다.
- `book`에서 `linear-gradient`을 응용해서 다각형을 만들었다. 이 트릭을 사용하지 않았다면 `clip-path`로 직접 만들었을 듯하다.

## 2021.01.01

> 2021년! 파이팅하여 공부하자!

- 셰이프 외부 및 부동을 사용하여 셰이프 주위에 텍스트 흐름을 만드는 방법
  특정 모양(동그라미, 세모등) 주위에 텍스트를 흐르게 만드는 방법 `shape-outside: circle(50% at 50% 50%);` 특정 요소를 원하는 모양으로 표현 `clip-path: circle(50% at 50% 50%);`
- 이미지에 필터를 적용하는 방법
- 전체 섹션을 다루는 배경 비디오를 만드는 방법
- `<video>` html 요소 사용 방법
- object-fit 속성을 사용하는 방법과 시기
- `transform: skewX(12deg);`로 요소를 기울인 경우 안에 있는 **요소들을** `transform: skewX(-12deg);`로 해줘야 원래대로 돌아온다.
- 드디어 **Tous Section** 부분이 끝났다. 이번 섹션에서는 카드뒤집기를 css로만 구현하는 방법에 대해서 알아보았다. 새로운 속성이 너무 많이 나와서 추후에 다시 한번 따라해보면서 정리해보아야겠다.

## 2020.12.29

- `background-blend-mode`
- `clip-path`와 같은 현대적인 속성은 브라우저 접두사를 반드시 사용해야한다.
- `box-decoration-break`
  한줄의 글자에 `padding`을 주고 크기를 줄여 두줄이 되면 줄바꿈된 곳에 `padding`이 들어가있지 않을 때 사용

CSS에 정말 유용한 속성이 많이 나왔다. 공부할게 너무 많네.. 이번 기회에 이런 속성이 있구나.. 정도로 알아두고 나중에 사용할 일이 생기면 다시 찾아보는 방식으로 공부하자. 아무래도 새로운 속성은 브라우저 호환성 때문에 실무에서 사용하지 못할 가능성이 높으니까.

## 2020.12.27

- 카드 뒤집기 효과 만들기

  - `transform: rotateY()`
  - `perspective`
    원근감을 준다.
  - `backface-visibility`
    뒤집어진 뒷면이 보여질지 안보여질지를 결정한다.

  카드 뒤집기 효과가 많은 속성의 응용이라 쉽게 이해가 되지 않는다. 대략적인 사용 용도는 파악했다.

- BEM 활용하기
  - Block
  - Element
  - Modifier
- 반응형 이미지
  반응형 웹에서 이미지 처리는 매우 어렵습니다. 고정된 크기의 경우 스크롤이 생겨버리거든요. `%` 단위를 사용합시다.
- outline, border를 테두리라고 한다면 outline은 border 바깥 외곽선을 말합니다.
  - outline-offset, border와 outline 사이의 여백을 의미합니다.
- linea 아이콘 사이트
  - \_basic/\_ICONFONT/fonts, styles 복사해서 사용
- `transform: skewY()`로 틀어진 내부 아이템을 `transform: skewY()` 반대 값을 주면 원래대로 돌아온다. 모든 값에 지정해주면 되지만 모든 아이템 하나하나마다 이를 지정하면 매우 비효율적일것이다. 그래서 `& > *` 선택자를 이용하자.

## 2020.12.26

- 그리드 시스템 만들어보기

  ```html
  <section class="grid-test">
    <div class="row">
      <div class="col-1-of-2">Col 1 of 2</div>
      <div class="col-1-of-2">Col 1 of 2</div>
    </div>

    <div class="row">
      <div class="col-1-of-3">Col 1 of 3</div>
      <div class="col-1-of-3">Col 1 of 3</div>
      <div class="col-1-of-3">Col 1 of 3</div>
    </div>

    <div class="row">
      <div class="col-1-of-3">Col 1 of 3</div>
      <div class="col-2-of-3">Col 2 of 3</div>
    </div>

    <div class="row">
      <div class="col-1-of-4">Col 1 of 4</div>
      <div class="col-1-of-4">Col 1 of 4</div>
      <div class="col-1-of-4">Col 1 of 4</div>
      <div class="col-1-of-4">Col 1 of 4</div>
    </div>

    <div class="row">
      <div class="col-1-of-4">Col 1 of 4</div>
      <div class="col-1-of-4">Col 1 of 4</div>
      <div class="col-2-of-4">Col 2 of 4</div>
    </div>

    <div class="row">
      <div class="col-1-of-4">Col 1 of 4</div>
      <div class="col-3-of-4">Col 3 of 4</div>
    </div>
  </section>
  ```

  ```scss
  .row {
    max-width: $grid-width; // 1140px 보다 작은 경우 100%를 차지
    background-color: #eee;
    margin: 0 auto;

    &:not(:last-child) {
      // 마지막 요소를 제외한 모든 row
      margin-bottom: $gutter-vertical;
    }

    @include clearfix;

    [class^="col-"] {
      // 클래스 이름이 col-로 시작하는 경우
      background-color: orangered;
      float: left;
      &:not(:last-child) {
        margin-right: $gutter-horizontal;
      }
    }
    .col-1-of-2 {
      width: calc((100% - #{$gutter-horizontal}) / 2);
    }

    .col-1-of-3 {
      width: calc(
        (100% - 2 * #{$gutter-horizontal}) / 3
      ); // 사이 여백이 2개이므로 곱하기 2
    }

    .col-2-of-3 {
      width: calc(
        2 * ((100% - 2 * #{$gutter-horizontal}) / 3) + #{$gutter-horizontal}
      ); // col-1-of-3 을 두개 곱한 후 horizontal을 하나 더하면 된다.
    }

    .col-1-of-4 {
      width: calc(
        (100% - 3 * #{$gutter-horizontal}) / 4
      ); // 사이 여백이 3개이므로 곱하기 3
    }

    .col-2-of-4 {
      width: calc(
        2 * ((100% - 3 * #{$gutter-horizontal}) / 4) + #{$gutter-horizontal}
      ); // col-1-of-4 을 두개 곱한 후 horizontal을 하나 더하면 된다.
    }

    .col-3-of-4 {
      width: calc(
        3 * ((100% - 3 * #{$gutter-horizontal}) / 4) + 2 * #{$gutter-horizontal}
      ); // col-1-of-3 을 세개 곱한 후 horizontal를 두배한 값을 더하면 된다.
    }
  }
  ```

- css `calc`에서 sass 변수를 사용하려면 `#{$gutter-horizontal}` 형식으로 해야 사용 간으하다.
- 그리드 시스템 구성 방법을 공부했고 이해했다. 현재는 하나하나 계산하여 그리드 시스템을 구축하지만 scss 함수를 이용하면 이를 자동화할 수 있다.
- 요소 크기나 색상을 변수로 관리하는 경우 변화에 기민하게 대처할 수 있다.
- 시맨틱 요소를 적절히 사용하기
  - `main`
  - `header`
- Enmet
  HTML을 편리하게 사용하기 위한 편리 기능
- `-webkit-background-clip`
  background-clip 속성은 요소의 배경이 테두리, 안쪽 여백, 콘텐츠 상자 중 어디까지 차지할 지 지정합니다.
  현재 프로젝트에서 글자에 그라디에이션 효과를 주어야하는데. 글자에 `background-image: linear-gradient()` 주고 난 후에 그라디에이션(배경)이 텍스트 영역에만 보이게 하면 글자에 효과가 적용됩니다.
- `transform: skewY()`는 기울기를 의미합니다.
- `text-shadow` 텍스트에 그림자를 추가합니다.
- 프로젝트에서 재사용할 유틸성 클래스를 많이 만들어서 재사용하라.

  ```scss
  .u-center-text {
    // 텍스트 중앙 정렬
    text-align: center;
  }

  .u-margin-bottom-8 {
    margin-bottom: 8rem;
  }
  ```

- `&rarr;` HTML에서 오른쪽 화살표 의미

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
