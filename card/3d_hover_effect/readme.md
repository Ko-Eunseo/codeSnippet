## 3d Card hover effect

### [출처](https://codepen.io/rudtjd2548/details/GRyRwoj)

### 원리

1. getBoundingClientRect();

- 마우스 좌표값을 구하는 함수.
- 왼쪽여백(x좌표), 위쪽여백(y좌표), element의 height, width

```js
let { x, y, width, height } = element.getBoundingClientRect();
```

2. mousemove(e) {}

- mousemove 함수를 통해 실시간으로 마우스 좌표를 구한다.
- e.clientX, e.clientY(브라우저 창 기준에서 커서의 위치)
- 브라우저 창 기준에서 커서 x좌표(y좌표)에서 element의 x좌표(y좌표)를 빼서 element 기준에서의 좌표를 구한다.
- element의 left, top 좌표 구하기
  2-1. left = clientX - x좌표(마우스 기준 왼쪽여백)
  2-2. top = clientY - y좌표(마우스 기준 위쪽여백)
  ```js
  const left = e.clientX - x;
  const top = e.clientY - y;
  ```

````

3. center(0,0) 을 기준으로, 클릭한 위치의 좌표

- 중심(0,0)에서 얼마나 떨어져있는지를 구한다.
- element의 left(top) 위치에서 width(height)의 중간까지 길이를 뺀다.
  3-1. centerX = left - (width / 2);
  3-2. centerY = top - (height / 2);
    ```js
  const centerX = left - (width / 2);
  const centerY = top - (height / 2);
````

4. rotate3d(x, y, z, deg)

- element를 기울인다.(css)
- 가정

```
width: 300px;
height: 450px;
rotate3d(-2, 2, 0, 25deg)

centerX = 150px; // (width/2)
centerY = 225px; // (height/2)
```

- rotate3d의 x에는 위아래로 기울어지게 하기 위해 y값을 준다.
  이때, 1자리 수가 나오도록 100으로 나눈다. (-centerY/100)
- rotate3d의 y에는 좌우로 기울어지게 하기 위해 x값을 준다. (centerX/100)
- 중심으로부터의 거리를 피타고라스 정의를 활용해 구한다. (a\*\*2 + b\*\*2 = c\*\*2)

```js
  const 거리 = Math.sqrt(centerX**2 + centerY\*\*2)
```

- deg가 20정도의 값을 가지도록 10으로 나눈다. (거리/10)

5. background-image: radial-gradient(circle at center, red 0, blue, green 100%)

- 3d 효과를 주기 위해 카드에 그라데이션을 준다.
- 클릭한 위치는 어둡게, 반대쪽 끝은 밝게 한다. radial-gradient(circle at center, black, transparent, white)

```js
light.style.backgroundImage = `
  radial-gradient(
    circle at ${left}px ${top}px, #00000010, #ffffff00, #ffffff70
  )
`;
```

6. box-shadow

- 3d 효과를 주기 위해 그림자를 넣어준다.
- x값이 -면 그림자가 왼쪽, y값이 -이면 그림자가 위쪽으로 생성된다.
- (-centerX)px (-centerY)px 10px #00000010(10은 투명도)

```js
card.style.boxShadow = `
  ${-centerX / 10}px ${-centerY / 10}px 10px rgba(0,0,0,0.1)
`;
```

7. resize

- 브라우저 창 크기가 바뀔때마다 x, y값을 다시 구한다.

```js
window.addEventListener("resize", () => {
  rect = element.getBoundingClientRect();
  x = rect.x;
  y = rect.y;
});
```
