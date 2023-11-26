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

실험해보자. 그래서 <script>에서 #title을 잡아 출력하게 하고, <body>안에는 #title을 넣어보자.

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

![0](/assets/images/javascript%20basic/selector_for_of.png.png)

하나씩 꺼내어 잘 출력했다. for문으로 사용하는 예시는 다음과 같다.

```javascript
const items = document.querySelectorAll("#wrap article");
for(let i=0; i<items.length; i++) {
    console.log(items[i]);
}
```


### 부모, 자식, 형제 요소 선택
























































































































































































