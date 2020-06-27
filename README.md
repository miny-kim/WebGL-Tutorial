# WebGL Tutorial - Change Cube Attribute
<br/>

## 1. 프로젝트 이름 
* Change Cube Attribute
<br/><br/>

## 2. 이름
* 소프트웨어학과 201720580 김민영
<br/><br/>

## 3. 프로젝트 개요
    수업에서 배운 cube를 사용자가 마음대로 속성을 바꿔서 바로 적용해 볼 수 있게 하였다.
    1. 큐브의 크기 조절
    2. 큐브의 색/불투명도 조절
    3. 마우스 이벤트를 이용한 큐브의 회전
    4. 큐브 그릴때 depth test 적용/미적용
    5. gl.draw 함수 사용시 적용할 타입 설정 (gl.TRIANGLES / gl.LINES)



<hr />
<br/>


## 4. 프로젝트 기능 설명 <br/>
#### 1. 큐브의 크기 조절 <br/>
기본 화면에 나온 큐브에서 크기를 크게 하고 작게 조절 할 수 있게 하였다. 
이는 버튼을 두개 두어서 view_matrix[14]에 직접 값을 더하고 빼면서 구현할 수 있었다.
~~~javascript
function sizeUp() {
view_matrix[14] = view_matrix[14] + 0.5;
}

function sizeDown() {
view_matrix[14] = view_matrix[14] - 0.5;
}
~~~
#### 2. 큐브의 색/불투명도 조절 <br/>
기본 화면에 나온 큐브에서 큐브에 적용된 색을 조절할 수 있게 하였다.
이는 이전 과제에서 사용하였던 큐브의 좌표를 받는 코드를 응용해서 color에 적용하였다.
함수 setColor(색, 불투명도) 에 대한 결과값이 shader안에서 사용되는 matrix인 colors에 적용된다.
~~~javascript
function colorUp() {
a += 0.2;
colors = setColor(a, b);
gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(colors), gl.STATIC_DRAW);
}

function colorDown() {
a -= 0.2;
colors = setColor(a, b);
gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(colors), gl.STATIC_DRAW);
}
~~~
#### 3. 마우스 이벤트를 이용한 큐브의 회전 <br/>
큐브의 회전을 더 직관적으로 보여주기 위해서 마우스 이벤트를 이용해서 회전을 구현하였다.
드래그한 상태에서 마우스 포인터를 움직이면 그 방향으로 회전한다.
특정 축이 아닌 마음대로 회전해 볼 수 있게 하면서 큐브가 그려지는 모양에 대한 이해를 높일 수 있었다.
~~~javascript
//when mouse is pressed.
var mouseDown = function(e) {
drag = true;
old_x = e.pageX, old_y = e.pageY;
e.preventDefault();
return false;
};
//when mouse is released.
var mouseUp = function(e) {
drag = false;
};
//when mouse is moving while mouse is pressed.
var mouseMove = function(e) {
if (!drag) return false;
else {
dX = (e.pageX - old_x) * 2 * Math.PI / canvas.width,
dY = (e.pageY - old_y) * 2 * Math.PI / canvas.height;
RotX += dX;
RotY += dY;
old_x = e.pageX, old_y = e.pageY;
e.preventDefault();
}
};
~~~
#### 4. depth test <br/>
여러가지 per-fragment operation 중에 가장 중요하다고 생각되는 depth test 설정을 on/off 해 볼 수 있는 버튼을 만들었다.
기본 설정은 depth test를 적용하는 것이고 disable버튼이 set되면 cube를 그릴때 depth test를 적용해지 않고 그린 cube를 볼 수 있다.
~~~javascript
if (zbufferonoff == 0) {
gl.enable(gl.DEPTH_TEST);
} else {
gl.disable(gl.DEPTH_TEST);
}
~~~
#### 5. gl.draw 종류 지정 <br/>
webGL은 primitive type으로 point, line, triangle을 가지고 있다. 모든 그림은 primitive type 3가지 만으로 그려진다.
이중 line과 triangle의 설정을 바꿀수 있게 하여 webgl이 어떻게 다양하게 그림을 그리는지 쉽게 이해할 수 있다.
~~~javascript
function drawType() {
radio_btn = document.getElementsByName("type")
if (radio_btn[0].checked == true)
gl.drawElements(gl.TRIANGLES, indices.length, gl.UNSIGNED_SHORT, 0);
else
gl.drawElements(gl.LINES, indices.length, gl.UNSIGNED_SHORT, 0);

}
~~~

<hr />
<br/>

## 5. 사용되는 Github 오픈소스 SW 목록
* 교과 과목에서 사용한 CUBE 코드 사용 
* gl-matrix.js
* https://www.tutorialspoint.com/webgl/webgl_interactive_cube.htm
* https://git.ajou.ac.kr/hwan/webgl-tutorial/

## Copyright
(CC-NC-BY) 김민영 2020
