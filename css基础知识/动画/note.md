# 浏览器私有前缀

css3完全向后（向前就是向未来；向后就是向过去）兼容：不必改变现有的设计，浏览器永远支持css2

因为制定HTML和CSS标准的组织W3C动作是很慢的，一个属性已经足够成熟了之后（但还未发布为w3c标准），浏览器商不愿意等太久就会在自己的浏览器中加入此属性的支持，但是呢，此时毕竟是某个浏览器商自己决定的支持，并不是w3c的规范，所以呢，浏览器商自行支持某个属性时，不能直接支持这个属性的原名属性，而是在属性前面加上一个前缀，然后这家浏览器商对这个加了自家前缀的属性进行支持：

~~~css
div {
  text-stroke:1px #f00; // 这个属性（文字描边）谷歌是支持的，但是属于一个测试阶段，也就是我们上面所说的谷歌提前于w3c进行了此属性的支持；所以这样写是无效的
  -webkit-text-stroke: 1px #f00; // 有效代码（谷歌从功能上支持text-stroke属性，但实际支持的属性是-webkit-text-stroke）
}
~~~

不同的浏览器商，也就是对应着不同的浏览器内核，对应着一个或者多个浏览器：

* Gecko内核：前缀为`-moz-`，火狐浏览器
* Webkit内核：也叫谷歌内核，前缀为`-webkit-`，chrome浏览器最先开发使用，safari，以及国内的360极速、猎豹等
* Trident内核：也称为IE内核，前缀为`-ms-`
* Presto内核：前缀为`-o-`，只有opera采用

平时开发时对于一个测试阶段的css属性，正确的书写方式是：

把各浏览器内核支持的带前缀的属性依次全写完，然后最后书写没有前缀的标准属性（我猜应该是方便属性正式加入标准之后进行css样式覆盖吧）

# 过渡与动画的理解

过渡是由一个状态转变成另一个状态的过程，而动画就是过渡这种状态转变的集合。

transition属性应用于某个元素之后，这时并不会触发效果，需要等待这个元素的样式改变（不管是js操作还是css操作），所以从这种层面上讲过渡是一个待触发的效果。

动画的定义过程中已经包含了各个状态，即状态的变化过程已经定义好了，动画属性应用于某个元素之后，就会直接按照我们设计的效果去执行动画，只是说动画执行时状态变化的过程中会使用过渡效果。所以动画是一种设置及生效的效果。

# 过渡属性

## 概述：

![image-20230130155311907](./images/:Users:jinrongda:Library:Application Support:typora-user-images:image-20230130155311907.png)

## 过渡属性

![image-20230130155441662](./images/:Users:jinrongda:Library:Application Support:typora-user-images:image-20230130155441662.png)

## 过渡时间

![image-20230130155534699](./images/:Users:jinrongda:Library:Application Support:typora-user-images:image-20230130155534699.png)

## 过渡函数

![image-20230130155611636](./images/:Users:jinrongda:Library:Application Support:typora-user-images:image-20230130155611636.png)

## 过渡延迟

![image-20230130155816144](./images/:Users:jinrongda:Library:Application Support:typora-user-images:image-20230130155816144.png)

## 简写属性transition

![image-20230130155954863](./images/:Users:jinrongda:Library:Application Support:typora-user-images:image-20230130155954863.png)

* 可以同时设置这四个值，并且**没有次序要求**。

* 只有一个要求：

  * 如果写两个时间 前边的默认是持续时间 后边的是默认时间。

  * 如果写一个时间，这个时间指的是持续时间。

# animation动画

其实动画属性使用起来与transition很相似

简写属性：`animation`，若干个子属性，其中一个是使用的动画，类似于transition中指定的过渡属性，然后还有动画持续时间，动画延迟时间，动画重复次数以及动画函数

子属性也没有次序要求，只是第一个时间一定是动画持续时间

**使用动画之前需要先用@keyframes定义：**

~~~css
/*
    定义关键帧动画：
*/
@keyframes myAnimation {
    50% {
        margin-left: -300px;
    }
}
/*
    给swiper-item-container使用动画（让它水平移动）
*/
#swiper-item-container {
    animation: 3s infinite 1s myAnimation;
}
~~~

具体案例见：./关键帧动画.html

# Vue动画

## 基本使用

![:Users:jinrongda:Desktop:0f08a69f233d4c1f8ac32da1d7510a66](./images/:Users:jinrongda:Desktop:css学习笔记:css基础知识:关键帧动画相关图片:Users:jinrongda:Desktop:0f08a69f233d4c1f8ac32da1d7510a66.png)

~~~html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
	<title></title>
	<style type="text/css">
		/* 设置持续时间和动画函数 */
		.v-enter-active,.v-leave-active {
		  transition: all .8s ease;
          }
		.v-enter, .v-leave-to{
		  transform: translateX(100px);
		  opacity: 0;
		}
	</style>
</head>
<body>
	<div id="app">
		  <button @click="show = !show">切换</button>
		  <transition>
		    <p v-show="show">hello</p>
		  </transition>

	</div>

	<script type="text/javascript">
		let vue  = new Vue({
			el:'#app',
			data:{
				show:true,
			},
		})

	</script>
</body>
</html>

~~~

![7716084cef81488d85b099e69a013160](./images/:Users:jinrongda:Desktop:7716084cef81488d85b099e69a013160.png)

enter阶段对应元素的生成，leave阶段对应元素的消失；

也就是说v-enter、v-enter-to、v-leave、v-leave-to是定义动画过程中结点状态（样式）的，然后v-enter-active会在整个enter阶段生效