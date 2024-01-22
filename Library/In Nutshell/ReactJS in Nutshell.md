# Table of Contents

- [1. Introduction](#1.-introduction)
- [2. The Basic of ReactJS](#2.-the-basic-of-reactjs)


# 1. Introduction
## 1.1 Why ReactJS

- 어떤 Library 또는 Framework를 채택할지 알 수 없기 때문에 처음 배울 언어를 잘 선택하는 것이 좋은데, React JS는 그 점에 있어서 배우기 좋은 언어라고 볼 수 있다.
	1. 많은 웹사이트들이 React JS를 사용한다. (e.g. 에어비앤비, 페이스북, 넷플릭스...)
	2. 해당 웹사이트가 웹사이트를 운영하고 있는 회사에 얼마나 중요한지 생각할 것
	3. React JS는 페이스북으로부터 왔다. 이 말인 즉, 해당 언어를 개발시키기 위해 투자하는 회사가 존재한다는 뜻이다.
	4. React JS는 Javascrift와 많은 공통 분모가 있다. 이 말인 즉, 커뮤니티도 따라 거대하다는 뜻이다.

## 1.2 Requirements

- 바닐라 JS로 크롬앱 만들기를 수강하고 오는 것이 좋다.
- Node JS도 후에 습득 해 놓으면 좋다.
- Requirement는 다음과 같다.
	1. 웹 페이지
	2. 텍스트 편집기

### 1.2.1 Event Listener

- 웹 페이지나 애플리케이션에서 발생하는 이벤트(e.g. 클릭, 키 입력, 마우스 움직임 등)를 감지하고 처리하는 역할을 하는 프로그래밍 구성 요소이다.
- e.g.
```js
<body>
    <button id="myButton">클릭하세요</button>

    <script>
        // 버튼 요소를 선택하고 클릭 이벤트에 대한 이벤트 리스너 등록
        const button = document.getElementById('myButton');

        button.addEventListener('click', function(event) {
            alert('버튼이 클릭되었습니다!');
            console.log(event); // 이벤트 객체 출력
        });
    </script>
</body>
```

### 1.2.2 DOM(Document Object Model)

- HTML을 Javascript에서 수정하기 위해 사용되는 인터페이스다. 이를 통해 동적 웹 페이지를 만들고 사용자 상호작용을 처리할 수 있다.
- 일반적인 단계는 다음과 같다.
	1. 요소 선택하기: JavaScript로 HTML 요소를 선택한다. 선택한 요소는 변수에 저장된다.
	2. 요소 수정하기: 선택한 HTML 요소를 수정한다.
	3. 새 요소 생성 및 추가: 새로운 HTML 요소를 생성하고 웹 페이지에 추가한다.
	4. 요소 제거: 'remove' 매서드를 사용하여 요소를 삭제한다.
	5. 속성 수정: HTML 요소의 속성을 수정한다.
	6. 이벤트 처리: 요소에 이벤트 리스너를 추가하거나 제거 할 수 있다.


# 2. The Basic of ReactJS
## 2.1 Introduction

- ReactJS는 UI를 interactive하게 만들어준다. 다르게 말하면 웹사이트에 interactivity(상호작용)을 만들어준다.
- ReactJS는 상호작용을 위한 아이디어를 모아놓았다.

## 2.2 바닐라JS vs ReactJS

e.g. 클릭할 때마다 클릭 횟수를 화면에 업데이트해주는 웹사이트
- 바닐라JS
	1. HTML에서 class나 id로 버튼 생성
	2. Javascript에서 이 버튼을 찾기 위해 document.getElementByld/querySelector 사용
	3. button.addEventListener(event 감지)를 하고 function을 실행한다.(클릭 수 업데이트)
	4. HTML을 업데이트한다.(span 사용)
	`>>`상호작용의 관점에서 코드가 지나치게 비효율적이다.

- ReactJS
	React를 사용하기 위해선 react와 react-dom을 링크를 이용해서 import 해와야 한다.
```js
<script src="https://www.unpkg.com/react@16.7.0/umd/react.production.min.js"></script>
<script src="https://www.unpkg.com/react-dom@16.7.0/umd/react-dom.production.min.js"></script>

<script>
// User Code
</script>
```

## 2.3 First React Element

- ReactJS : 어플리케이션이 interative하도록 만들어주는 library이다.
- react-dom : React element들을 HTML body에 둘 수 있도록 해주는 library이다.
- render : React element를 HTML로 만들어 배치하여 사용자에게 보여주는 것.
- 바닐라 JS VS ReactJS
	+ 바닐라 JS : HTML을 먼저 만들고, 그걸 Javascript로 가져와서 HTML을 수정
	+ React JS : Javascript로 먼저 시작한 후, 그 다음에 HTML이 됨

## 2.4 Events in React

- `<div>` : ReactDOM이 React element들을 가져다 놓는 곳이다. element중 "root"는 렌더링 되는 가장 최상위 요소이다.
- createElement("div", {property}, {contents})에서 property는 id, class, event listener등이 될 수 있다.
- Data 업데이트

## 2.5 JSX

- JSX : 문법이 HTML과 유사한 Javascript의 확장 버전
- Babel : JSX 문법을 ReactJS 문법으로 바꿔주는 pre-컴파일러
- 사용자가 정의한 Component(Title, Button 등)을 다른 곳에서 사용할 때, 첫 글자는 반드시 대문자여야 한다. 만약 소문자면 React와 JSX는 이 component가 HTML 태그라고 생각할 것이다.
- Component안에 다른 Component들을 포함 시킬 수 있다.
- `<Component />` : 복붙의 의미
- 왜 JSX 문법을 이용하여 코드를 다르게 쓰느냐? 개발자들이 더 이해하기 쉽도록!
- Example code:
```
function Title() { return ( //User Code ); }
const Title = () ==> ( //User Code );
//두 개는 의미가 같다.
```