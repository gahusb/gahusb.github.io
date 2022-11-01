---
layout: post
title: ReactJS
image:
subtitle: 리액트를 활용하여 영화 웹 서비스 개발
category: devlog
tags: react
accent_image: 
  background: url('/assets/img/web_dev/webdev_sidebar.jpeg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Nomadcoder의 강의를 통해 리액트를 이용해서 영화 앱 서비스를 클론 코딩 해보자.
invert_sidebar: true
sitemap: false
comments: true
---

# 리액트를 활용하여 영화 웹 서비스 개발

* toc
{:toc .large-only}

## What ReactJS ?


## ReacJS를 사용하는 이유
 - interactive(상호작용을 위해)


## VanilliaJS vs ReactJS

VaniliaJS
```html
<!DOCTYPE html>
<html>
<body>
    <span>Total click: 0</span>
    <button id="btn">Click me</button>
</body>
<script>
    let counter = 0;
    const button = document.querySelector("btn");
    const span = document.querySelector("span");
    function handleClick() {
        counter += 1;
        span.innerText = `Total clicks: ${counter}`;
    }
    button.addEventListener("click", handleClick);
</script>
</html>
```

ReactJS
```html
<!DOCTYPE html>
<html>
<body>
    <div id="root"></div>
</body>
<script src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
<script>
    let counter = 0;
    const root = document.getElementById("root");
    const h3 = React.createElement(
        "h3",
        { 
            id: "title",
        },
        `Total click: ${counter}`
    );
    const btn = React.createElement(
        "button",
        {
            onClick: () => counter += 1,
        },
        "Click me"
    );
    const container = React.createElement("div", null, [h3, btn]);
    ReactDOM.render(container, root);
</script>
</html>
```

위에서 간단한 버튼을 삽입하여 카운트를 증가하는 작업을 하는 파일을 만들었다. <br />
간단히 보더라도 body태그 안에 데이터를 넣을 태그를 선언해주고 **querySelector**에서 태그를 찾아서 넣고 Event Listener를 선언해서 연결해주고... <br />

위에처럼 간단하게 만든 버튼 클릭 시, 카운트 증가하는 경우에는 비슷해보일지 모르지만 저런 기능 하나하나가 모인다고 생각해보면 보기만해도 어질어질해진다...

그래서 ReactJS를 사용해서 상호작용해주는 멋진 프레임워크를 사용한다.

## Introducing JSX !
```
Babel compiles JSX down to React.createElement() calls.
```
위 문구에서 볼 수 있듯이 **reactjs.org**에 소개된 JSX의 첫 소개 문구이다. <br />
JSX는 Javascript를 확장한 문법으로, createElement()를 선언하여 태그와 속성을 가져오는 부분을 더 심플하게 컴파일해주는 Babel의 기능이다.

```JSX
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```javascript
const element = React.createElement(
  'hi',
  {className: 'greeting'},
  'Hello, world!'
);
```

첫 번째 코드가 JSX를 사용한 코드이고, 아래가 그냥 JS에 createElement()를 사용해서 속성을 선언한 코드이다.

이를 반영해서 위의 간단한 카운트 코드를 변경해보았다.

```JSX
<!DOCTYPE html>
<html>
<body>
    <div id="root"></div>
</body>
<script src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
    let counter = 0;
    const root = document.getElementById("root");
    const Title = (
        <h3 id="title">
            Total click: `${counter}`
        </h3>
    );
    const Button = (
        <button
            style={{
                backgroundColor: "tomato",
            }}
            onClick={() => counter += 1}
        >
            Click me
        </button>
    );
    const container = React.createElement("div", null, [Title, Button]);
    ReactDOM.render(container, root);
</script>
</html>
```

위 코드에서처럼 JSX를 사용하기 위해서는 **Babel**을 import할 필요가 있다.<br />
왜냐하면 브라우저는 JSX라는 형태를 인식하지 못하기 때문에 JSX를 브라우저가 알 수 있는 코드로 변형해주기 위해서이다.

그리고서 안을 보자면 일반적으로 알고 있는 HTML의 형태와 유사한 코드를 볼 수 있다.<br />
각 태그가 있고, 그 태그 안에는 class, style등의 속성이나 event가 들어갈 수 있고, 모든 정보들을 알아보기 간편해졌다!<br />


## Understanding State
ReactJS에서 State란 무엇인가? <br />
State는 데이터를 저장하는 장소이다.

아래 코드를 살펴보자

```html
...
<script type="text/babel">
    const root = document.getElementById("root");
    let counter = 0;
    function countUp () {
        counter += 1;
        render();
    }
    function render() {
        ReactDOM.render(<Container />, root);
    }
    function Container() {
        return (
            <div>
                <h3>Total click: {counter}</h3>
                <button onClick={countUp}>Click me</button>
            </div>
        );
    }
    render();
</script>
</html>
```

위의 코드를 보면 버튼을 클릭할 때마다 다시 render() 함수를 불러와 호출하는것을 볼 수 있다.<br />
그러나 저 방식은 코드가 많아지거나 컨테이너의 포함 범위가 많아지게되면 rerender를 어느 시점에서 다시 해줘야 하는지 설계하기도 어렵고 나중에 유지보수도 힘들게 된다.

이것을 위해 useState()를 사용하여 데이터와 해당 데이터의 변화를 감지하여 해당 데이터 부분만 리렌더링 해줄 수 있다.

아래의 코드를 살펴보자

```html
...
<script type="text/babel">
    const root = document.getElementById("root");
    function App() {
        const [counter, setCounter] = React.useState(0);
        const onClick = () => {
            setCounter((current) => current + 1);
        }
        return (
            <div>
                <h3>Total click: {counter}</h3>
                <button onClick={onClick}>Click me</button>
            </div>
        );
    }
    ReactDOM.render(<App />, root);
</script>
</html>
```

간단하게 useState()를 통해 초기값 0을 가진 counter와 setCounter라는 데이터의 상태를 변화시켜주는 함수를 같이 선언할 수 있다.<br />
그리고 버튼을 클릭할때마다 counter를 증가시켜주도록 하면, 위에서 rerender를 위해 다시 호출하여 리렌더링해주는 작업을 별도로 선언하지 않아도 해당 데이터 부분에 변화가 생기면 해당 데이터 부분만 리렌더링 된다!!<br />
정말 멋진 기능이 아닐 수 없다!<br />

이 동작은 modifier라고 하는 setCounter라고 명명한 부분이 실행이되면 해당 컴포넌트(여기선 App)를 재실행하게 된다. <br />
그래서 setCounter가 실행되는 부분부터 순차적으로 아래로 실행이되며, return()되는 부분이 재실행되고, 리렌더링이 되는것이다. <br />

바로 이 부분이 ReactJS의 기능의 가장 중점이되는 부분이다. 데이터가 바뀔때마다 컴포넌트를 리렌더링하고 UI를 refresh해준다. <br />

이는 vanillaJS처럼 HTML의 요소를 찾을 필요없고, 이벤트리스너를 더해줄 필요도, UI를 업데이트해줄 필요도 없도록 해주었다! <br />
JSX를 통해 바로 HTML을 넣고, 곧바로 이벤트리스너를 더해주고, state가 변화하면 자동으로 리렌더링 되는 것이다!! So Cool ❗️❗️😆

