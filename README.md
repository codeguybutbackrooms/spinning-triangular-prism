# Spinning Triangular Prism
This code will render a spinning triangular prism in ASCII character hash (#). It's all only JavaScript, basic linear algebra, and simple trigonometry.

## Triangular Prism
Triangular Prism is not known to many people. It is like Pyramid, but the back part will be longer, like a rectangle. <br>

A triangular prism consists of:

- **2 triangular faces** (front and back), each with 3 vertices
- **3 rectangular side faces** connecting corresponding vertices
- **6 total vertices** and **9 edges**

## Vertex Coordinates
We place the front triangle at _z = −1_ and the back at _z = +1_:

```js
const points = [
  [ 1,  1, -1],  // 0: front right
  [-1,  1, -1],  // 1: front left
  [ 0, -1, -1],  // 2: front bottom
  [ 1,  1,  1],  // 3: back right
  [-1,  1,  1],  // 4: back left
  [ 0, -1,  1],  // 5: back bottom
];
```
## Edges
- Connect each front vertex to its corresponding back vertex, forming the rectangular faces:
 - Edge between front right (0) and back right (3)
 - Edge between front left (1) and back left (4)
 - Edge between front bottom (2) and back bottom (5)
```js
const edges = [
  // front triangle
  [0,1], [1,2], [2,0],
  // back triangle
  [3,4], [4,5], [5,3],
  // side edges connecting front → back
  [0,3], [1,4], [2,5],
];
```
Each pair [i,j] represents a line segment between the projected positions of points[i] and points[j]. These segments outline all faces of the prism, ensuring that both triangular bases and the three side faces are drawn.

## 3D Spinning
We rotate each point $`(x, y, z)`$ by angle $`\theta`$ around the vertical axis using:

$`
\begin{pmatrix}
\cos\theta & 0 & -\sin\theta \\
0 & 1 & 0 \\
\sin\theta & 0 & \cos\theta
\end{pmatrix}
`$

```js
function rotateY([x,y,z], θ) {
  const c = Math.cos(θ), s = Math.sin(θ);
  return [
    c*x - s*z,
    y,
    s*x + c*z
  ];
}
```

## Perspective Projection
To simulate depth, we project each rotated point $`(x′, y′, z′)`$ onto 2D:

$`x_{\text{proj}} = \frac{x'}{z' + d}, \quad y_{\text{proj}} = \frac{y'}{z' + d}`$

Where <kbd>d</kbd> is the camera distance (e.g. 3). In code:

```js
function project([x,y,z]) {
  const dist = 3;
  const f = dist / (dist + z);
  return [
    Math.round(x * f * scale),
    Math.round(y * f * scale)
  ];
}
```

## Bresenham's Line Algorithm
Each edge between two projected points $`(x_{\text{0}}, y_{\text{0}})`$ and $`(x_{\text{1}}, y_{\text{1}})`$ is drawn into a 2D character buffer:
```js
function drawLine(buf, x0,y0, x1,y1) {
  // standard Bresenham implementation:
  // step from (x0,y0) to (x1,y1), setting buf[y][x] = '#'
}
```
