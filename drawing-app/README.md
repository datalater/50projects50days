# Starting point

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Drawing App</title>
</head>

<body>
    <div class="toolbox" id="toolbox">
        <button id="decrease">-</button>
        <span id="size">10</span>
        <button id="increase">+</button>
        <input type="color" id="color">
        <button id="clear">X</button>
    </div>
    <canvas id="canvas""></canvas>
    <script src=" script.js"></script>
</body>

</html>
```

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

* {
    box-sizing: border-box;
}

body {
    background-color: #f5f5f5;
    font-family: 'Roboto', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
}
```

# CSS Instructions

## 캔바스 영역 `canvas`

1. `border: 2px solid steelblue`: 경계선을 넣어준다.

## 툷박스 영역 `.toolbox`

1. `background-color: steelblue`: 배경색을 지정한다.
2. `border: 1px solid slateblue`: 경계선을 지정한다.
3. `display: flex`: 가로 정렬을 위해 flex 모드를 정렬한다.
4. `width: 604px`: canvas의 너비가 600인데 좌우 border가 각각 2px이므로 툴박스 너비를 604px로 지정한다.
5. `padding: 1rem`: 테두리 안쪽 여백을 넣어준다.

## 툴박스 다이렉트 칠드런 `.toolbox > *`

1. `background-color: #fff`: 배경색을 지정한다.
2. `border: none`: 테두리를 없앤다.
3. `display: inline-flex`: flex 모드가 적용된 inline element로 취급한다.
4. `align-items: center`: 텍스트 같은 인라인 요소(현재 `inline-flex`이므로)를 축을 움직여서 가운데에 둔다.
5. `justify-content: center`: 축에 있는 아이템 간격을 정렬한다.
6. `font-size: 2rem`: 폰트 크기를 지정한다.
7. `width: 50px`: 툴박스 아이템의 너비를 지정한다.
8. `height: 50px`: 툴박스 아이템의 높이를 지정한다.
9. `margin: 0.25rem`: 툴박스 아이템의 테두리 바깥 여백을 지정한다.
10. `padding: 0.25rem`: 툴박스 아이템의 테두리 안쪽 여백을 지정한다.
11. `cursor: pointer`: 마우스 호버가 되면 커서를 포인터로 바꾼다.

## 툴박스 다이렉트 칠드런 중 마지막 요소 `.toolbox > *:last-child`

1. `margin-left: auto`: 마지막 요소의 왼쪽 마진을 auto로 줘서 가장 오른쪽에 위치하도록 한다.

# JS Instructions

## 캔바스 API로 원과 라인 그리기

1. 캔바스 노드를 가져온다.

```js
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
```

2. 원을 그리는 함수를 구현한다. 펜촉의 모양을 원으로 할 것이기 때문이다.

```js
let size = 20;
let color = 'black';
let x;
let y;

function drawCircle(x, y) {
    ctx.beginPath();
    ctx.arc(x, y, size, 0, Math.PI * 2);
    ctx.fillStyle = color;
    ctx.fill();
}
```

3. 선을 그리는 함수를 구현한다. 마우스가 너무 빨리 움직이면 좌표가 연속되지 않을 수 있기 때문에 처음 지점에서 다음 지점까지 연속으로 그리는 기능이 필요하다.

```js
function drawLine(x1, y1, x2, y2) {
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.strokeStyle = color;
    ctx.lineWidth = size * 2; // 원은 반지름을 사용하기 때문에 라인의 두께를 원과 동일하게 하려면 선 너비를 2배로 곱해야 한다.
    ctx.stroke();
}
```

코드 중간 점검:

```js
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

let size = 20;
let color = 'black';
let x;
let y;

function drawCircle(x, y) {
    ctx.beginPath();
    ctx.arc(x, y, size, 0, Math.PI * 2);
    ctx.fillStyle = color;
    ctx.fill();
}

function drawLine(x1, y1, x2, y2) {
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.strokeStyle = color;
    ctx.lineWidth = size * 2; // 원은 반지름을 사용하기 때문에 라인의 두께를 원과 동일하게 하려면 선 너비를 2배로 곱해야 한다.
    ctx.stroke();
}
```

## 마우스 움직임에 따라 그림 그리는 효과 구현하기

마우스를 클릭하고 드래그하면서 그림이 그려지는 효과를 구현할 것이다. 필요한 변수를 추가하고, 캔바스에 이벤트 리스너를 추가한다.

1. 마우스 클릭 버튼이 눌려졌을 때에 따른 이벤트 리스너 추가하기. 그림을 시작하려면 마우스 클릭 시점에 대한 x, y 좌표를 얻어야 한다.

```js
let size = 20;
let isPressed = false; // add this
let color = 'black';
let x;
let y;

cavnas.addEventListener('mousedown', (e) => {
    isPressed = true;

    x = e.offsetX;
    y = e.offsetY;
});
```

2. 마우스 클릭 버튼이 눌렸다가 떼어지는 것을 감지하는 이벤트 리스너를 추가한다. 클릭이 취소되면 그림 그리기를 멈춰야 한다.

```js
canvas.addEventListener('mouseup', (e) => {
    isPressed = false;

    x = undefined;
    y = undefined;
});
```

3. 마우스가 클릭된 상태에서 움직일 때 그림 그리는 효과를 넣기 위해 이벤트 리스너를 추가한다.

```js
canvas.addEventListener('mousemove', (e) => {
    if (isPressed) {
        const x2 = e.offsetX;
        const y2 = e.offsetY;

        drawCircle(x2, y2);
    }
})
```

4. 3에서 구현한 기능은 완전하지 않다. 마우스가 빨리 움직일 때 원이 연속으로 찍히지 않고 띄엄띄엄 찍히는 사이드 이펙트가 있다. 연속된 구간을 그릴 수 있도록 라인 그리기 함수를 추가로 호출한다.

```js
canvas.addEventListener('mousemove', (e) => {
    if (isPressed) {
        const x2 = e.offsetX;
        const y2 = e.offsetY;

        drawCircle(x2, y2);
        drawLine(x, y, x2, y2);
    }
})
```

5. 4에서 구현한 기능은 완전하지 않다. 마우스 움직임 이벤트가 발생할 때마다 이전 좌표인 (x, y)를 업데이트 해줘야 한다.

```js
canvas.addEventListener('mousemove', (e) => {
    if (isPressed) {
        const x2 = e.offsetX;
        const y2 = e.offsetY;

        drawCircle(x2, y2);
        drawLine(x, y, x2, y2);

        x = x2;
        y = y2;
    }
})
```

## 툴박스 기능 구현하기

1. 클리어 버튼 구현하기

```js
const clearBtn = document.getElementById('clear');

clearBtn.addEventListener('click', () => {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
})
```

2. 인풋 컬러의 값을 그림 색상으로 사용하기

```js
const inputColor = document.getElementById('color');

inputColor.addEventListener('change', (e) => {
    color = e.target.value;
});
```

3. 펜촉의 크기 조절 버튼 구현하기

```js
const sizeSpan = document.getElementById('size');
const increase = document.getElementById('increase');
const decrease = document.getElementById('decrease');

let size = parseInt(sizeSpan.innerText);

increase.addEventListener('click', () => {
    size += 5;
    sizeSpan.innerText = size;
});

decrease.addEventListener('click', () => {
    size -= 5;

    if (size <= 0) {
        size = 0;
    }
    sizeSpan.innerText = size;
});
```

# 커스텀 Instructions

## 캔바스 너비를 창 크기에 비례하기 만들기

```js
const canvas = document.getElementById('canvas');
canvas.width = document.body.clientWidth * 0.9; // Add this
canvas.height = document.body.clientHeight * 0.9; // Add this
const ctx = canvas.getContext('2d');
```

## 툴박스 위치를 상단으로 고정하기

```css
.toolbox {
    background-color: steelblue;
    border: 1px solid slateblue;
    display: flex;
    width: 100%;
    padding: 1rem;

    position: absolute;     /* Add this */
    top: 0;                 /* Add this */
}
```

## export 기능 만들기

1. 익스포트 버튼과 이미지 미리보기를 보여줄 img 태그를 추가한다.

```html
<body>
    <div class="toolbox" id="toolbox">
        <button id="decrease">-</button>
        <span id="size">10</span>
        <button id="increase">+</button>
        <input type="color" id="color">
        <button id="export">export</button>
        <img src="" alt="" id="export-img">
        <button id="clear">clear</button>
    </div>
    <canvas id="canvas""></canvas>
    <script src=" script.js"></script>
</body>
```

2. export 버튼과 clear 버튼의 width를 컨텐트 너비로 하기 위해 css를 수정한다.

```css
#clear, 
#export {
    width: initial;
    padding: 1rem;
}
```

3. 이미지는 export 버튼이 눌리기 전에는 보여주지 않는다.

```css
#export-img {
    display: none;
}
```

4. 익스포트 기능을 자바스크립트로 구현한다.

```js
const exportBtn = document.getElementById('export');
const exportImg = document.getElementById('export-img');


// 이미지 프리뷰 기능
exportBtn.addEventListener('click', () => {
    exportImg.style.display = 'inline-flex';
    exportImg.src = canvas.toDataURL('image/png');
})


// 이미지 다운로드 기능
exportImg.addEventListener('click', () => {
    var gh = canvas.toDataURL('png');

    var a = document.createElement('a');
    a.href = gh;
    a.download = 'image.png';

    a.click()
})
```

# 커스텀 solution

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Drawing App</title>
</head>

<body>
    <div class="toolbox" id="toolbox">
        <button id="decrease">-</button>
        <span id="size">10</span>
        <button id="increase">+</button>
        <input type="color" id="color">
        <button id="export">export</button>
        <img src="" alt="" id="export-img">
        <button id="clear">clear</button>
    </div>
    <canvas id="canvas""></canvas>
    <script src=" script.js"></script>
</body>

</html>
```

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

* {
    box-sizing: border-box;
}

body {
    background-color: #f5f5f5;
    font-family: 'Roboto', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
    overflow: hidden;
}

canvas {
    background-color: #fff;
    border: 2px solid steelblue;
}

.toolbox {
    background: #7474BF;  /* fallback for old browsers */
    background: -webkit-linear-gradient(to right, #348AC7, #7474BF);  /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to right, #348AC7, #7474BF); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

    display: flex;
    width: 100%;
    padding: 1rem;

    position: absolute;
    top: 0;
}

.toolbox > * {
    background-color: #fff;
    border: none;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-size: 1.2rem;
    width: 50px;
    height: 50px;
    margin: 0.25rem;
    padding: 0.25rem;
    cursor: pointer;
}

.toolbox > *:hover {
    border: 3px solid cornflowerblue;
}

.toolbox > *:last-child {
    margin-left: auto;
}

#clear, 
#export {
    width: initial;
    padding: 1rem;
}

#export-img {
    display: none;
}
```

```js
const canvas = document.getElementById('canvas');
canvas.width = document.body.clientWidth * 0.9;
canvas.height = document.body.clientHeight * 0.9;
const ctx = canvas.getContext('2d');
const clearBtn = document.getElementById('clear');
const inputColor = document.getElementById('color');
const sizeSpan = document.getElementById('size');
const increase = document.getElementById('increase');
const decrease = document.getElementById('decrease');
const exportBtn = document.getElementById('export');
const exportImg = document.getElementById('export-img');

let size = parseInt(sizeSpan.innerText);
let isPressed = false;
let color = 'black';
let x;
let y;

canvas.addEventListener('mousedown', (e) => {
    isPressed = true;

    x = e.offsetX;
    y = e.offsetY;
});

canvas.addEventListener('mouseup', (e) => {
    isPressed = false;

    x = undefined;
    y = undefined;
});

canvas.addEventListener('mousemove', (e) => {
    if (isPressed) {
        const x2 = e.offsetX;
        const y2 = e.offsetY;

        drawCircle(x2, y2);
        drawLine(x, y, x2, y2);

        x = x2;
        y = y2;
    }
});

function drawCircle(x, y) {
    ctx.beginPath();
    ctx.arc(x, y, size, 0, Math.PI * 2);
    ctx.fillStyle = color;
    ctx.fill();
}

function drawLine(x1, y1, x2, y2) {
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.strokeStyle = color;
    ctx.lineWidth = size * 2;
    ctx.stroke();
}

inputColor.addEventListener('change', (e) => {
    console.log(e.target.value);
    color = e.target.value;
});

clearBtn.addEventListener('click', () => {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
});

increase.addEventListener('click', () => {
    size += 5;
    sizeSpan.innerText = size;
});

decrease.addEventListener('click', () => {
    size -= 5;

    if (size <= 0) {
        size = 0;
    }
    sizeSpan.innerText = size;
});

exportBtn.addEventListener('click', () => {
    exportImg.style.display = 'inline-flex';
    exportImg.src = canvas.toDataURL('image/png');
})

exportImg.addEventListener('click', () => {
    var gh = canvas.toDataURL('png');

    var a = document.createElement('a');
    a.href = gh;
    a.download = 'image.png';

    a.click()
})
```