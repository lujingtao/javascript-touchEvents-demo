@[toc](javascript移动设备触屏事件及练习)

# 一、前言
对于写原生js代码或者理解触摸事件原理的人员来说，记录移动端触摸事件的核心内容及写下有实用意义的demo是十分有必要的，本文仅做简单介绍和demo演示，用于日后需要的时候使用。

# 二、触摸事件简单介绍
核心是如下几个事件：
touchstart - 触摸开始
touchend - 触摸结束
touchmove - 触摸移动

想了解更详尽信息可以自行百度一下“javascript 触屏事件”
使用方法：

```csharp
//添加“触摸开始”事件监听 
document.getElementById("#obj").addEventListener("touchstart", function (e) { 
    var point = e.touches[0]; //e：事件对象，e.touches[0]相当于一个手指对象 
    startX =  point.pageX; // 手指具有pageX、pageY属性，相当于手指在页面中的x、y坐标 
    startY =  point.pageY; 
    alert("手指触碰坐标为x轴："+startX+"| y轴："+startY); 
},false); 
```

# 三、在线演示和GitHub源码
效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015113956987.gif)

[-->在线演示](https://lujingtao.github.io/javascript-touchEvents-demo/)
[-->GitHub源码](https://github.com/lujingtao/javascript-touchEvents-demo)

下一篇文章[《javascript移动设备触屏画板（附演示地址和源码）》](https://blog.csdn.net/iamlujingtao/article/details/102564181)<font color="red">我们以此为基础结合Canvas来做一个画板！</font>

# 四、示例核心代码

```js
		(function(){ 
			var isTouchPad = (/hp-tablet/gi).test(navigator.appVersion);
			var hasTouch = 'ontouchstart' in window && !isTouchPad;
			var touchStart = hasTouch ? 'touchstart' : 'mousedown';
			var touchMove = hasTouch ? 'touchmove' : 'mousemove';
			var touchEnd = hasTouch ? 'touchend' : 'mouseup';
			var startX = 0;
			var startY = 0;
			var distX = 0;
			var distY = 0;
			var touchArea = document.getElementById("touchArea");
			var p =document.getElementById("point");
			var log =document.getElementById("log");
			var logStr ="";


			var start = function(e){
				var point = hasTouch ? e.touches[0] : e;
				distX = distY = 0;
				startX =  point.pageX; 
				startY =  point.pageY;
				p.style.left=point.pageX+"px";
				p.style.top=point.pageY+"px";
				logStr="";
				log.innerText=logStr="触摸开始( x:"+startX+",y: "+startY+")";

				//添加“触摸移动”事件监听
				touchArea.	addEventListener(touchMove, move,false);
				//添加“触摸结束”事件监听
				touchArea.addEventListener(touchEnd, end ,false);
			}

			var move = function(e){
					var point = hasTouch ? e.touches[0] : e;
					distX = distY = 0;
					e.preventDefault();
					p.style.left=point.pageX+"px";
					p.style.top=point.pageY+"px";
					distX = point.pageX-startX;
					distY = point.pageY-startY;
					var tempStr = "触摸开始( x:"+startX+",y: "+startY+")";
					logStr=tempStr+" | 触摸移动( x:"+point.pageX+",y: "+point.pageY+")";
					log.innerText=logStr;
			}

			var end = function(e){
					logStr=logStr+"\n触摸结束( x轴移动距离:"+distX+",y轴移动距离: "+distY+")";
					log.innerText=logStr;
					touchArea.removeEventListener(touchStart, end, false);
					touchArea.removeEventListener(touchMove, move, false);
					touchArea.removeEventListener(touchEnd, end, false);
			}

			//添加“触摸开始”事件监听
			touchArea.addEventListener(touchStart,start,false);


		})();
```
