# Starting point

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Expanding Cards</title>
</head>

<body>
    <div class="container">
        <div class="panel active"
            style="background-image: url('https://i.picsum.photos/id/667/1200/600.jpg?hmac=-W4R932df-nW8MwisC1G0ZqHTD9uOyOZeAwpGWRytgA')">
            <h3>Explore The World</h3>
        </div>
        <div class="panel"
            style="background-image: url('https://i.picsum.photos/id/331/1200/600.jpg?hmac=v71pUninK6hHC1D5oJeXulGz1YxJtizP05LPA4yJwwg')">
            <h3>Wild Forest</h3>
        </div>
        <div class="panel"
            style="background-image: url('https://i.picsum.photos/id/338/1200/600.jpg?hmac=noBx0ZMHPhhOdPBW4c3KNaRLiAbFuVu0H0TFLKMX6ew')">
            <h3>Sunny Beach</h3>
        </div>
        <div class="panel"
            style="background-image: url('https://i.picsum.photos/id/121/1200/600.jpg?hmac=dmiuAZbsurkULeyl2vx-VlW4wKr5mpoYXNpuSV8x5DY')">
            <h3>City on Winter</h3>
        </div>
        <div class="panel"
            style="background-image: url('https://i.picsum.photos/id/396/1200/600.jpg?hmac=D7mKaNbl9myhayrW_3RYbVaqioEJEQvbbKuELV0aq-E')">
            <h3>Mountains - Clouds</h3>
        </div>
    </div>

    <script src="script.js"></script>
</body>

</html>
```

```css
@import url('https://fonts.googleapis.com/css?family=Muli&display=swap');

* {
    box-sizing: border-box;
}

body {
    font-family: 'Muli', sans-serif;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    overflow: hidden;
    margin: 0;
}
```

# CSS Instructions

다음 항목에 대해 CSS를 적용하라.

## 이미지 컨테이너 `.container`

1. `display: flex`: 이미지 아이템을 가로로 정렬하고, 컨테이너와 아이템을 플렉스 모드로 적용한다.
2. `width: 90vw`: 이미지 컨테이너의 너비를 90% viewport width로 한다.

## 이미지 아이템 `.panel`

1. `background-size: cover`: 백그라운드 이미지가 일부가 아니라 전부가 나오도록 한다.
2. `background-position: center`: 백그라운드 이미지가 가운데에 오도록 한다.
3. `background-repeat: no-repeat`: 백그라운드 이미지가 반복 패턴을 만들지 않도록 한다.
4. `height: 80vh`: 이미지 아이템의 높이를 viewport height의 80%로 한다.
5. `border-radius: 50px`: 테두리를 둥글게 만든다.
6. `color: #fff`: 텍스트 컬러를 화이트로 한다.
7. `cursor: pointer`: hover되면 커서를 포인터로 한다.
8. `flex: 0.5`: flex proportion을 0.5로 even하게 맞춘다. 나중에 `.panel.active`의 flex 비율을 0.5보다 크게 줄 것이다.
9. `margin: 10px`: 이미지 간에 간격을 주기 위해 마진을 추가한다.
10. `position: relative`: 이미지 캡션으로 사용되는 h3의 포지션을 relatively absolute로 사용하기 위해 h3의 container인 `.panel`의 포지션을 relative로 적용한다.
11. `transition: flex 0.7s ease-in`: `.panel.active`에 transition 효과를 줄 것이다. 그리고 효과를 줄 property는 flex이며 0.7초 동안 duration을 가지고, ease-in effect를 적용한다.

## 이미지 아이템 캡션 `.panel h3`

1. `font-size: 24px`: 글자 크기를 24px로 한다.
2. `position: absolute`: 이미지 아이템 안에서 캡션이 absolute position을 가지도록 한다.
3. `bottom: 20px`: 캡션의 absolute 오프셋을 바닥에서 20px 떨어진 곳으로 한다.
4. `left: 20px`: 캡션의 absolute 오프셋을 왼쪽에서 20px 떨어진 곳으로 한다.
5. `margin: 0`: 마진을 없앤다.
6. `opacity: 0`: 이미지가 활성화(`.panel.active`) 되어 있지 않을 때는 보여줄 필요가 없으므로 진하기(opacity)를 0으로 줘서 안 보이도록 한다.

## 활성화된 이미지 아이템 `.panel.active`

1. `flex: 5`: 활성화된 이미지는 크게 보여주기 위해 flex 비율을 5배로 늘린다.

## 활성화된 이미지 아이템의 캡션 `.panel.active h3`

1. `opacity: 1`: 활성화된 이미지의 캡션을 보여주기 위해 진하기를 1(100%)로 한다.


## 미디어 쿼리

1. 너비가 480px 이하일 경우 미디어 쿼리를 적용한다.
2. 이미지 컨테이너의 너비를 `100vw`로 한다.
3. 이미지 아이템의 마지막 두 개를 보여주지 않는다.

```css
@media (max-width: 480px) {
    .container {
        width: 100vw;
    }

    .panel:nth-of-type(4),
    .panel:nth-of-type(5) {
        display: none;
    }
}
```

# JS Instructions

클래스 이름을 추가하고 지우는 방식으로 이미지 활성화 기능을 만들 것이다. 

1. 모든 panel 노드 리스트 가져온다.

```js
const panels = document.querySelectorAll('.panel');
```

2. 모든 panel 노드에 클릭 이벤트 리스너를 추가한다. 클릭하면 클래스 이름에 `active`를 추가하는 이벤트 핸들러를 만든다.

```js
panels.forEach((panel) => {
    panel.addEventListener('click', () => {
        panel.classList.add('active');
    })
})
```

3. 단, 현재 클릭된 panel을 제외한 나머지 panel은 비활성화해야 하므로 모든 panel의 클래스 이름에서 `active`를 제거하는 함수 `removeActiveClasses()`를 실행한다.

```js
panels.forEach((panel) => {
    panel.addEventListener('click', () => {
        removeActiveClasses();
        panel.classList.add('active');
    })
})
```

4. `removeActiveClasses()`를 구현한다.

```js
function removeActiveClasses() {
    panels.forEach((panel) => {
        panel.classList.remove('active');
    })
}
```

5. 지금까지 구현한 기능을 실행하면 활성화된 패널의 flex가 증가할 때 너비가 좁았을 때는 캡션 텍스트가 두 줄이었다가 너비가 넓어지면서 한 줄로 바뀌면서, 캡션 텍스트가 아래로 떨어지는 현상이 발생한다. 이는 기대하지 않은 사이드 이펙트이므로 수정한다. 기존에 작성한 CSS 파일에서 이미지 패널의 캡션에 적용된 opacity에 transition 효과를 추가하여 해결한다.

```css
.panel.active h3 {
    opacity: 1;
    transition: opacity 0.3s ease-in 0.4s; /* Add this. We put the delay of 0.4s */
}
```

# Solution

## HTML

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Expanding Cards</title>
</head>

<body>
    <div class="container">
        <div class="panel active"
            style="background-image: url('https://i.picsum.photos/id/667/1200/600.jpg?hmac=-W4R932df-nW8MwisC1G0ZqHTD9uOyOZeAwpGWRytgA')">
            <h3>Explore The World</h3>
        </div>
        <div class="panel"
            style="background-image: url('https://i.picsum.photos/id/331/1200/600.jpg?hmac=v71pUninK6hHC1D5oJeXulGz1YxJtizP05LPA4yJwwg')">
            <h3>Wild Forest</h3>
        </div>
        <div class="panel"
            style="background-image: url('https://i.picsum.photos/id/338/1200/600.jpg?hmac=noBx0ZMHPhhOdPBW4c3KNaRLiAbFuVu0H0TFLKMX6ew')">
            <h3>Sunny Beach</h3>
        </div>
        <div class="panel"
            style="background-image: url('https://i.picsum.photos/id/121/1200/600.jpg?hmac=dmiuAZbsurkULeyl2vx-VlW4wKr5mpoYXNpuSV8x5DY')">
            <h3>City on Winter</h3>
        </div>
        <div class="panel"
            style="background-image: url('https://i.picsum.photos/id/396/1200/600.jpg?hmac=D7mKaNbl9myhayrW_3RYbVaqioEJEQvbbKuELV0aq-E')">
            <h3>Mountains - Clouds</h3>
        </div>
    </div>

    <script src="script.js"></script>
</body>

</html>
```

## CSS

```css
@import url('https://fonts.googleapis.com/css?family=Muli&display=swap');

* {
    box-sizing: border-box;
}

body {
    font-family: 'Muli', sans-serif;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    overflow: hidden;
    margin: 0;
}

.container {
    display: flex;
    width: 90vw;
}

.panel {
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    height: 80vh;
    border-radius: 50px;
    color: #fff;
    cursor: pointer;
    flex: 0.5;
    margin: 10px;
    position: relative;
    transition: flex 0.7s ease-in;
}

.panel h3 {
    font-size: 24px;
    position: absolute;
    bottom: 20px;
    left: 20px;
    margin: 0;
    opacity: 0;
}

.panel.active {
    flex: 5;
}

.panel.active h3 {
    opacity: 1;
    transition: opacity 0.3s ease-in 0.6s;
}

@media (max-width: 480px) {
    .container {
        width: 100vw;
    }

    .panel:nth-of-type(4),
    .panel:nth-of-type(5) {
        display: none;
    }
}
```

## JS

```js
const panels = document.querySelectorAll('.panel');

panels.forEach(panel => {
    panel.addEventListener('click', () => {
        removeActiveClasses();
        panel.classList.add('active');
    })
})

function removeActiveClasses() {
    panels.forEach(panel => {
        panel.classList.remove('active');
    })
}
```


