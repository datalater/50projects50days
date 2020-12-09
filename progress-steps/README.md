# 🚶🏻‍♂️ Walk through

## HTML instruction

1. 프로젝트명을 `Progress steps`로 하고 head 태그를 수정한다.
2. 전체를 감싸는 `.container` div를 만든다.
3. 프로그레스 아이템들을 감싸는 `.progress-container` div를 만든다.
4. 프로그레스 컨테이너 안에 프로그레스 선으로 사용할 `#progress.progress` div를 만든다.
5. 프로그레스 컨테이너 안에 프로그레스 숫자로 사용할 `.circle` div를 4개 만든다.
6. 위에서 만든 첫 번째 div의 클래스를 `.circle.active`로 수정한다.
7. 프로그레스를 변경할 수 있는 `button.btn#prev{prev}`와 `button.btn#next{next}`를 만든다.
8. prev 버튼은 disabled 속성을 넣는다. 첫 번째 단계에서는 그 이전 단계가 없으므로 prev 버튼을 disabled 한다.

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Progress steps</title>
</head>

<body>
    <div class="container">
        <div class="progress-container">
            <div class="progress" id="progress"></div>
            <div class="circle active">1</div>
            <div class="circle">2</div>
            <div class="circle">3</div>
            <div class="circle">4</div>
        </div>

        <button class="btn" id="prev" disabled>prev</button>
        <button class="btn" id="next">next</button>
    </div>

    <script src="script.js"></script>
</body>

</html>
```

## CSS instruction

### body 세팅

1. 폰트를 `Muli`로 변경한다.
2. body에 다음과 같은 속성을 적용한다.
    1. `background-color: #f6f7fb` 추가
    2. 보일러 플레이트에 있던 `flex-direction: column` 삭제

### 전체 컨테이너 `.container`

1. `text-align: center` 적용

### 프로그레스 컨테이너 `.progress-container`

1. `display: flex`
2. `justify-content: space-between`
3. `position: relative`: 프로그레스 컨테이너에 속한 아이템의 위치를 relatively absolute로 사용하기 위해 설정한다.
4. `margin-bottom: 30px`
5. `max-width: 100%`
6. `width: 350px`

### 프로그레스 라인 `.progress`

1. `background-color: #3498db`
2. `position: absolute`
3. `top: 50%`: 프로그레스 라인을 `.progress-container` 기준으로 세로로 중간에 둔다.
4. `left: 0`
5. `transform: translateY(-50%)`: 선의 두께를 고려하여 선 위치를 위로 50% 올려서 세로로 중간에 둔다.
6. `height: 4px`
7. `width: 50%`
8. `z-index: -1`: 프로그레스 숫자보다 뒤에 나오도록 z-index를 음수값으로 한다.
9. `transition: 0.4s ease`: 프로그레스 라인이 차오르고 줄어드는 애니메이션 효과를 위해 적용한다.

### 프로그레스 empty 라인 `.progress-container::before`

1. `.progress-container::before {}`: `progress-container`의 first child를 만든다.
2. 프로그레스 라인 `.progress`에 있는 코드를 그대로 가져온다.
3. `background-color: #e0e0e0`: 회색으로 한다.
4. `width: 100%`: 전체를 항상 꽉 채워야 한다.
5. `content: ''`: 빈 문자열을 넣어야 사용자 눈에 보인다.

```css
.progress-container::before {
    content: '';
    background-color: #e0e0e0;
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    height: 4px;
    width: 100%;
    z-index: -1;
}
```

### 프로그레스 숫자 `.circle`

```css
.circle {
    background-color: #fff;
    color: #999;
    border-radius: 50%;
    height: 30px;
    width: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 3px solid #e0e0e0;
    transition: 0.4s ease;
}
```

### 활성화된 프로그레스 숫자 `.circle.active`

```css
.circle.active {
    border-color: #3498db;
}
```

### refactoring

색상이 공통적으로 쓰이므로 변수를 사용한다.

```css
:root {
    --line-border-fill: #3498db;
    --line-border-empty: #e0e0e0;
}
```

```css
.progress-container::before {
    content: '';
    background-color: var(--line-border-empty);
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    height: 4px;
    width: 100%;
    z-index: -1;
}

.progress {
    background-color: var(--line-border-fill);
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    height: 4px;
    width: 50%;
    z-index: -1;
    transition: 0.4s
}

.circle {
    background-color: #fff;
    color: #999;
    border-radius: 50%;
    height: 30px;
    width: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 3px solid var(--line-border-empty);;
    transition: .4s, ease;
}

.circle.active {
    border-color: var(--line-border-fill);;
}
```

### 버튼 `.btn`

```css
.btn {
    background-color: var(--line-border-fill);
    color: #fff;
    border: 0;
    border-radius: 6px;
    cursor: pointer;
    font-family: inherit;
    padding: 8px 30px;
    margin: 5px;
    font-size: 14px;
}

.btn:active {
    transform: scale(0.98);
}

.btn:focus {
    outline: 0;
}

.btn:disabled {
    background-color: var(--line-border-empty);
    cursor: not-allowed;
}
```

## JS instruction

1. 버튼에 따라 프로그레스 라인과 프로그레스 숫자에 변화를 주기 위해서 필요한 변수를 가져온다.

```js
const progress = document.getElementById('progress');
const prev = document.getElementById('prev');
const next = document.getElementById('next');
const circles = document.querySelectorAll('.circle');
```

2. 현재 active한 프로그레스 숫자를 기억하기 위해 변수 currentActive를 선언한다. 처음 초기화 값은 1단계이므로 1로 사용한다.

```js
let currentActive = 1;
```

3. `next` 버튼을 누르면 currentActive가 1씩 증가하도록 이벤트리스너를 추가한다.

```js
next.addEventListener('click', () => {
    currentActive++;

    if (currentActive > circles.length) {
        currentActive = circles.length;
    }
});
```

4. `prev` 버튼을 누르면 currentActive가 1씩 감소하도록 이벤트리스너를 추가한다.

```js
prev.addEventListener('click', () => {
    currentActive--;

    if (currentActive < 1) {
        currentActive = circles.length;
    }
});
```

5. `prev`, `next` 버튼에 update 함수를 호출한다.

```js
next.addEventListener('click', () => {
    currentActive++;

    if (currentActive > circles.length) {
        currentActive = circles.length;
    }

    update();
});

prev.addEventListener('click', () => {
    currentActive--;

    if (currentActive < 1) {
        currentActive = circles.length;
    }

    update();
});
```

6. `update` 함수 기능으로 next와 prev 버튼을 눌렀을 때 프로그레스 숫자가 변화하도록 구현한다.

```js
function update() {
    circles.forEach((circle, idx) => {
        if (idx < currentActive) {
            circle.classList.add('active');
        } else {
            circle.classList.remove('active');
        }
    })
}
```

7. css에서 .progress의 width를 0으로 바꾼다.

```css
.progress {
    background-color: var(--line-border-fill);
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    height: 4px;
    width: 0%;      /* Change this */
    z-index: -1;
    transition: 0.4s
}
```

8. `update` 함수 기능으로 next와 prev 버튼을 눌렀을 때 프로그레스 라인이 변화하도록 구현한다. 한 선에 4개의 점이 찍혀 있으면 간격은 3칸이 된다. 1칸 채울 때 마다 프로그레스를 33%씩 채워야 한다. 따라서 프로그레스가 2일 때 1/3, 3일 때 2/3, 4일 때 3/3이 되도록 한다.

```js
function update() {
    circles.forEach((circle, idx) => {
        if (idx < currentActive) {
            circle.classList.add('active');
        } else {
            circle.classList.remove('active');
        }
    })

    const actives = document.querySelectorAll('.active');

    progress.style.width = (actives.length - 1) / (circles.length - 1) * 100 + '%';
}
```

9. `update` 함수에서 currentActive 값에 따라 `prev` 및 `next` 버튼을 언제 비활성화하고 언제 활성화할지 구현한다.

```js
function update() {
    circles.forEach((circle, idx) => {
        if (idx < currentActive) {
            circle.classList.add('active');
        } else {
            circle.classList.remove('active');
        }
    })

    const actives = document.querySelectorAll('.active');
    progress.style.width = (actives.length - 1) / (circles.length - 1) * 100 + '%';

    if (currentActive === 1) {
        prev.disabled = true;
    } else if (currentActive === circles.length) {
        next.disabled = true;
    } else {
        prev.disabled = false;
        next.disabled = false;
    }
}
```

# ✅ 2W1H

## Q1. 프로그레스 라인이 회색에서 파란색으로 채워지는 효과를 나타내기 위해 회색 선과 파란 선을 동일한 위치에 두고 싶을 때 어떻게 해야 하는가?

1. 파란색 프로그레스 라인을 그린다.
2. 파란색 프로그레스 라인을 포함하는 컨테이너 클래스에 pseudo-class `::before` 붙여서 컨테이너의 first-child 셀럭터를 추가하고 그 셀렉터에 파란색 프로그레스 라인과 동일한 CSS를 넣는다.

## Q2-1. 총 네 단계인 프로그레스 바를 단계가 올라갈 때마다 채우려면 한 칸에 몇 퍼센트씩 채워야 하는가?

33% (단, 마지막 칸에 도달할 때는 100%)

## Q2-2. 다음 코드의 뜻을 프로그레스 단계별로 경우의 수를 나눠서 설명하라.

```js
const actives = document.querySelectorAll('.active');
progress.style.width = (actives.length - 1) / (circles.length - 1) * 100 + '%';
```

총 4단계 중 현재 1단계라면: 

* actives.length = 1
* circles.length = 4
* `progress.style.width = 0 / 3 * 100 + '%'` => 0%


총 4단계 중 현재 2단계라면: 

* actives.length = 2
* circles.length = 4
* `progress.style.width = 1 / 3 * 100 + '%'` => 33%

총 4단계 중 현재 3단계라면: 

* actives.length = 3
* circles.length = 4
* `progress.style.width = 2 / 3 * 100 + '%'` => 66%

총 4단계 중 현재 4단계라면: 

* actives.length = 4
* circles.length = 4
* `progress.style.width = 3 / 3 * 100 + '%'` => 100%