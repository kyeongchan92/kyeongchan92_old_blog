---
title: Javascript 기초
description:
categories:
tags:
---

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="ie=edge">
    <title>Javascript</title>
    <link rel="stylesheet" href="css/style.css">
    <script>
        console.log("Hello world")
    </script>
</head>
<body>
</body>
</html>
```

![0](/assets/images/javascript%20basic/helloworld.png)

개발자도구를 열어보면 Hello World가 출력되어있다. 웹브라우저는 HTML 파일의 위부터 순서대로 읽는다. <body> 영역이 HTML 출력을 담당하는데, <body>보다 먼저 출력되면 아직 생성되지도 않은
HTML요소를 제어할 수 없으므로 문제가 생길 것이다.

실험해보자. 그래서 script 태그에서 #title을 잡아 출력하게 하고, <body>안에는 #title을 넣어보자.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="ie=edge">
    <title>Javascript</title>
    <link rel="stylesheet" href="css/style.css">
    <script>
        const title = document.querySelector("#title");
        console.log(title);
    </script>
</head>
<body>
<h1 id="title">HELLO WORLD</h1>
</body>
</html>
```

![0](/assets/images/javascript%20basic/helloworld_title.png)

콘솔에서는 null이 출력된다. 자바스크립트가 실행되는 시점에서 아직 <body>부분을 읽지 못했기 때문에 #title을 잡지 못한 것이다. 호출위치를 <body>안으로 넣어보자.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="ie=edge">
    <title>Javascript</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
<h1 id="title">HELLO WORLD</h1>
<script>
    const title = document.querySelector("#title");
    console.log(title);
</script>
</body>
</html>
```

![0](/assets/images/javascript%20basic/helloworld_in_body.png)

제대로 #title이 출력된다!!

이제부터는 <head>태그 안에 ```<script defer src="custom.js"></script>``` 처럼 외부에서 js파일을 호출하여 사용하도록 하자.
defer는 body를 해석하면서 동시에 외부 js파일을 가져오도록 한다. 그 다음 body 영역이 모두 출력된 후 외부 스크립트 파일을 실행한다.


## js로 HTML 요소 선택하기

### document.querySelector()

요소를 선택할 때 사용한다.

```html
<section id="wrap">
    <article class="box1">box1</article>
    <article class="box1">box2</article>
    <article class="box1">box3</article>
</section>
```

```javascript
document.querySelector("#wrap");
```

body에서 id가 wrap인 요소를 찾으라는 의미다.

```javascript
const frame = document.querySelector("#wrap");
console.log(frame);
```
frame이라는 변수에 #wrap 요소가 저장된다.

![0](/assets/images/javascript%20basic/selector_wrap.png)


#wrap 요소가 제대로 출력되었다.

이번엔 #wrap 안에 있는 .box1을 찾아보자.

```javascript
const box1 = document.querySelector("#wrap .box1");
console.log(box1);
```

![0](/assets/images/javascript%20basic/selector_box1.png)


article 속의 .box1이 잘 출력되었다.

### document.querySelectorAll()

요소를 모두 선택할 때 사용한다. html은 위 예시를 그대로 쓰자.

```javascript
const item = document.querySelector("#wrap article");
console.log(item);
```

이렇게하면 가장 첫 번째 요소만 선택한다! 즉 box1만 출력된다. 대신 이렇게 쓰자.

```javascript
const items = document.querySelectorAll("#wrap article");
console.log(items);
```

![0](/assets/images/javascript%20basic/selector_selectall.png)

변수엔 NodeList가 담겼다. for of 문을 사용해서 하나씩 꺼내보자.


```javascript
const items = document.querySelectorAll("#wrap article");
for(let item of items) {
    console.log(item);
}
```

변수에는 const, let이 있다. const는 변경되지 않는 것, let은 변경되는 것이다. item은 계속 변경될 것이므로 let 변수에 담자.

![0](/assets/images/javascript%20basic/selector_for_of.png)

하나씩 꺼내어 잘 출력했다. for문으로 사용하는 예시는 다음과 같다.

```javascript
const items = document.querySelectorAll("#wrap article");
for(let i=0; i<items.length; i++) {
    console.log(items[i]);
}
```


### 부모, 자식, 형제 요소 선택

```html
<body>
    <ul class="list">
        <li class="item1">box1</li>
        <li class="item2">box2</li>
        <li class="item3">box3</li>
        <li class="item4">box4</li>
    </ul>
</body>
```
```javascript
const list = document.querySelector(".list");
const items = list.children;

console.log(items);
console.log(items[0]);
console.log(items[1]);
console.log(items[2]);
console.log(items[3]);
```

![0](/assets/images/javascript%20basic/parents_brother.png)

.list를 선택하면 <ul>태그를 선택한 것이다. 콘솔을 보면 자식 4개가 묶여서 출력되었다. 그리고 .children으로 자식요소를 선택해 출력하면 각각의 <li>가 선택된 것을 알 수 있다.


### 부모 요소 선택하기

```javascript
const item2 = document.querySelector(".item2");
console.log(item2.parentElement);
```

.parentElement를 사용하면 부모인 ul이 출력된다.


### 제일 가까운 상위 부모 요소 선택

```html
<body>
    <main>
        <section>
            <article>
                <ul>
                    <li>list</li>
                </ul>
            </article>
        </section>
    </main>
</body>
```

여기서 li로부터 main을 선택하고싶다면? 다음과같이 .closest를 쓰면 된다.

```javascript
const li = document.querySelector(".list");
console.log(li.closest("main"));
```

### 형제 요소 선택

이전 형제 : item3.previousElementSibling

다음 형제 : item3.nextElementSibling


## 자바스크립트로 스타일 제어

```html
<body>
    <article id="box"></article>
</body>
```

```css
#box {
    width: 200px;
    height: 200px;
    background-color: aqua;
    border: 5px solid #000;
    transform: rotate(0deg);
}
```

![0](/assets/images/javascript%20basic/css_control.png)

그림과 같이 가로세로 200px의 사각형을 하나 만들었다. 그리고 다음처럼 js파일을 연결하자.

```javascript
const box = document.querySelector("#box");

box.style.width = "10%";
box.style.height = "300px";
box.style.backgroundColor = "hotpink";
box.style.border = "none";
box.style.transform = "rotate(10deg)"
```

![0](/assets/images/javascript%20basic/css_control_box.png)


<article> 태그를 box 변수로 잡고, .style을 붙여 속성명을 입력하면 된다. background-color같ㅣ 하이픈으로 연결된 속성은 camel case로 변경해야한다. js에 예약어가 존재하기 때문이다.


!! document.querySelector로 선택한 요소는 DOM 객체이다. DOM에는 HTML + 스타일 정보(style속성)도 있어서 style 속성값을 변경할 수 있다.


## 이벤트 연결하기

마우스 동작과 관련된 이벤트를 HTML과 연결하고 제어해보자.

### 클릭 이벤트 연결하기

```html
<body>
    <a href="https://www.naver.com"></a>
</body>
```
```css
a {
    font-size: 100px;
    color: #555;
}
```
```javascript
const link = document.querySelector("a")

link.addEventListener("click", ()=>{
    console.log("링크를 클릭했습니다.");
})
```

변수 link에 <a>태그를 선택했다. .addEventListner()문은 (이벤트명, 전달될값=>실행할구문) 처럼 인자를 전달해 쓴다. 여기서는 첫 번째 인자(이벤트명) "click"을 사용했다.
두 번째 인자는 리스너인데, =>같은 화살표를 쓰며 이벤트가 발생할 때 응답해서 실행할 동작을 의미한다. 근데 위처럼 작성하면 클릭하자마자 네이버로 넘어가므로 링크이동을 막고 콘솔창에 출력해보도록 하자.


```javascript
const link = document.querySelector("a")

link.addEventListener("click", (e)=>{
    e.preventDefault();
    console.log("링크를 클릭했습니다.");
})
```

![0](/assets/images/javascript%20basic/click.png)

preventDefault는 이벤트의 기본 기능을 수행하지 말라는 것이다. 

여기서 e는 이벤트 객체이다.

### 호버 이벤트 연결하기

```html
<body>
    <div id="box"></div>
</body>
```
```css
#box {
    width: 200px;
    height: 200px;
    background: aqua;
    margin: 100px auto;
}
```
```javascript
const box = document.querySelector("#box");

box.addEventListener("mouseenter", ()=>{
    box.style.backgroundColor = "hotpink";
});

box.addEventListener("mouseleave", ()=>{
    box.style.backgroundColor = "aqua";
});
```

![0](/assets/images/javascript%20basic/hover_enter.png)

![0](/assets/images/javascript%20basic/hover_leave.png)

이벤트 mouseenter와 mouseleave를 사용했다. div 태그의 background-color 속성이 hotpink로 바뀌는 것을 볼 수 있다.














































































































































