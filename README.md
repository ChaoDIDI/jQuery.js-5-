# 动画图
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>动画图</title>
</head>
<body>
<style>
    html,body{
        width: 100%;
        height: 100%;
        margin: 0 0;
        padding:0 0;
        overflow:hidden;
    }
</style>

<a href="javascript:void(0)">全屏</a>
<script src="node_modules/jquery/dist/jquery.js"></script>
<script src="js/ball.js"></script>
<script src="js/jquery.fullscreen.js"></script>
</body>
</html>
```
```javascript
function Ball(r) {
    ////半径
    this.r = r;
}
//展示方法 在页面中绘制一个ball
Ball.prototype.show = function () {
    this.el = $("<div></div>");//创建一个jquery对象标签

    ///最大移动范围
    this.maxWidth = $(window).width();
    this.maxHeight = $(window).height();

    ////移动速度
    this.stepLength = 2;
    this.speedY = Math.random() * this.stepLength;
    this.speedX = Math.random() * this.stepLength;

    //////初始化位置
    this.position = {};
    this.position.top = (this.maxHeight - this.r * 2) * Math.random();
    this.position.left = (this.maxWidth - this.r * 2) * Math.random();

    ////移动方向
    this.directionY = Math.random()<0.5?1:1;
    this.directionX =  Math.random()<0.5?1:-1;
    //为元素定义多个样式
    this.el.css({
        "width": (this.r * 2) + "px",
        "height": (this.r * 2) + "px",
        "position": "absolute",
        "background-color": "hsl(" + (Math.random() * 360) + ",50%,50%)",
        "border-radius": "50%"
    });

    this.el.offset(this.position);

    $("body").append(this.el);

    var that = this;

    ////重置窗口大小
    $(window).resize(function () {
        that.maxWidth = $(window).width();
        that.maxHeight = $(window).height();
    });

    //this.move();

    //requestAnimationFrame(this.move())
};

Ball.prototype.move = function () {
    if (this.position.top >= this.maxHeight - (this.r * 2) - 2) {
        this.directionY = -1;
    }
    if (this.position.top <= 0) {
        this.directionY = 1;
    }

    if (this.position.left >= this.maxWidth - (this.r * 2) - 2) {
        this.directionX = -1;
    }
    if (this.position.left <= 0) {
        this.directionX = 1;
    }

    this.position.top += this.speedY * this.directionY;
    this.position.left += this.speedX * this.directionX;
    console.log(this.position);
    this.el.offset(this.position);

};
///接收另外的一个球 检查当前球和传递进来球的碰撞方式,改变结果
/// 改变 方向 速度
Ball.prototype.hits= function (ball) {

};
$(function () {
    //循环生成固定个数的小球
    for (var i = 0; i < 20; i++) {
        var ball = new Ball(20 * Math.random()+10);
        ball.show();


        // requestAnimationFrame(ball.move());
        doMove(ball);
        function doMove(ball) {
            var ball = ball;
            moveBall();
            function moveBall() {
                ball.move();
                requestAnimationFrame(moveBall);
            }
        }

    }
    // $(ducument).fullScreen(true);
    $("a").click(function () {
        $(document).fullScreen(true);
    })


});
/**
 * 小球之间发生碰撞效果
 * 1.两个小球正向碰撞,两个小球向相仿方向运行(速度改变)
 * 2.追尾1:速度快的撞上速度慢的.速度慢(速度慢的会加速运行,速度快的会减速运行)
 * 3.侧向碰撞*/
```
