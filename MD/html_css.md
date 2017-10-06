#HTML & CSS
1. 盒模型的宽高值计算方式？
	
		![](https://github.com/lj614418910/blog/blob/master/images/20170321180119900.png)
	
		![](https://github.com/lj614418910/blog/blob/master/images/20170321180159166.png)
		
2. 边界合并，边界塌陷？

	- margin合并，上下两个div的margin会取两个数值之中的最大值作为二者上下之间的距离。
	- margin塌陷，就是父子div之间的上下margin，会取最大值作为二者与外界的上下margin。
	- 解法：塌陷可以通过边框或者利用overflow属性来触发bfc的效果。合并可以给每一个div分别加上一个父级包裹层，然后给父级包裹层都加上overflow:hidden;

3. 标准模式与怪异模式区别？

	- 在标准模式页面按照HTML,CSS的定义渲染，而在怪异模式就是浏览器为了兼容很早之前针对旧版本浏览器设计，并未严格遵循W3C标准而产生的一种页面渲染模式。浏览器基于页面中文件类型描述的存在以决定采用哪种渲染模式，如果存在一个完整的DOCTYPE则浏览器将会采用标准模式，如果缺失就会采用怪异模式。下面介绍标准模式和怪异模式之间的区别。
	- 二者最主要区别是盒模型的不同。
	
		- ![](https://github.com/lj614418910/blog/blob/master/images/20170321180119900.png)
	
		- ![](https://github.com/lj614418910/blog/blob/master/images/20170321180159166.png)	

	- 图片元素的垂直对齐方式：对于inline元素和table-cell元素，标准模式下vertical-align属性默认取值为baseline，在怪异模式下，table单元格中的图片的vertical-align属性默认取值为bottom，因此在图片底部会有及像素的空间。
	- `<table>`元素中的字体：CSS中，对于font的属性都是可以继承的，怪异模式下，对于table元素，字体的某些元素将不会从body等其他封装元素中继承得到，特别是font-size属性。
	- 内联元素的尺寸：标准模式下，non-replaced inline元素无法自定义大小，怪异模式下，定义这些元素的width，height属性可以影响这些元素显示的尺寸。
	- 元素的百分比高度：
		- CSS中对于元素的百分比高度规定如下：百分比为元素包含块的高度，不可为负值，如果包含块的高度没有显示给出，该值等同于auto，所以百分比的高度必须在父元素有高度声明的情况下使用。
		- 当一个元素使用百分比高度时，标准模式下，高度取决于内容变化，怪异模式下，百分比高度被正确应用。
	- 元素溢出的处理：标准模式下，overflow取默认值visible，在怪异模式下，该溢出会被当做扩展box来对待，即元素的大小由其内容决定，溢出不会裁减，元素框自动调整，包含溢出内容。
	- box-sizing的概念？
		- box-sizing 属性允许您以特定的方式定义匹配某个区域的特定元素，默认值是content-box。还可以设置为box-sizing。
		- content-box：
			- padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 ( Element width = width + border + padding)
			- 此属性表现为标准模式下的盒模型。
		- border-box：
			- padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 ( Element width = width )
			- 此属性表现为怪异模式下的盒模型。

4. 简要说下BFC(Block Formatting Context)是什么？有哪些应用？

	- bfc全称是block format context——块级格式化上下文
	- 浮动元素、绝对定位元素，不是块级盒的块级包含块(比如inline-block、table-cell、table-capation)和overflow值不为visible的块级盒子为它们的内容建立了一个新的块级排版上下文。
	- 在一个块级排版上下文中，盒子是从包含块顶部开始，垂直的一个接一个的排列的，相邻两个盒子之间的垂直的间距是被margin属性所决定的，在一个块级排版上下文中相邻的两个块级盒之间的垂直margin是折叠的。参与BFC的布局的只有普通流normal flow中的块级盒，而float、position值不为relative\static的元素是脱离BFC这一布局环境的，不参与BFC的布局
	- 在一个块级排版上下文中，每个盒子的左外边是触碰到包含块的左边的（对于从右向左的排版，则相反），即使在有浮动元素参与的情况下也是如此(即使一个盒子的行盒是因为浮动而收缩了的)，除非这个盒子新建了一个块级排版上下文(在某些情况下这个盒子自身会因为floats而变窄)。
	- 应用：
		- 边距合并
		- 浮动的高度塌陷问题
		- 让文字环绕块级元素的方法
		- margin的折叠 

5. 要求大容器宽度自由伸缩的情况下，A/B/C三个元素的宽度比始终是1:1:1，如何实现？
	- 弹性布局:

		```
		.wrapper{
		    display: flex;
		}
		.A,.B,.C{
		    flex: 1;
		    height: 100px;
		    border: 1px solid red;
		}
		```
		```
		.wrapper{
			font-size: 0;
			width:100%;		
      	}
		.A,.B,.C{
		   display: inline-block;
			width:33.3%;
			box-sizing: border-box;
			height: 100px;
			border: 1px solid red;
		}//inline-block记得要把元素之间的空格清掉font-size: 0;
		```
		```
		 .wrapper{
            width:100%;
            overflow: hidden;
        }
        .A,.B,.C{
            float: left;
            width:33.3%;
            box-sizing: border-box;
            height: 100px;
            border: 1px solid red;
        }//这种方法记得清除浮动
		```

6. 居中
	- 已知子元素宽高:100px;
	- div宽高已知，可利用`position:absolute`绝对定位，用`top:50%;left:50%`将div的左上角置于容器的中点，再利用`margin-top:(负的1/2的div高度);margin-left: (负的1/2的div宽度)`;将div的中点和容器中点重合。
	
	```
	.wrapper{
		position: relative;
	}
	.A{
		position: absolute;
		left: 50%;
		top: 50%;
		margin-left: -50px;
		margin-top: -50px;
		height: 100px;
		width: 100px;
	}
	```
	
	- div宽高已知，可利用`position:absolute`绝对定位，然后用`top:0;left:0;bottom:0;right:0;`和`margin:auto`结合起来使div位于容器中间。其原理是`margin:auto`可分配剩余空间，平均分配所占位的剩余空间。
	
	```
	.wrapper{
	    position: relative;
	}
	.A{
	    position: absolute;
	    left: 0;
	    top: 0;
	    bottom: 0;
	    right: 0;
	    margin: auto;
	    height: 100px;
	    width: 100px;
	}
	```
	
	- div宽高未知，可利用`position:absolute`绝对定位，用`top:50%;left:50%`将div的左上角置于容器的中点，再利用`transfrom:translate(-50%,-50%)`,translate的50%是相对于div本身所占的宽高的。

	```
	.wrapper{
		position: relative;
	}
	.A{
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate3d(-50%, -50%, 0);
	}
	```
	
	- flex布局的居中，在父元素上设置`display:flex;justify-content:center;align-items:center`,flex布局中justify-content和align-items并不是上下左右对齐的意思，而分别是在主轴上对齐方式和在交叉轴上的对齐。<br/>而flex-direction是设置flex布局的主轴的方向（即项目的排列方向）。`row（默认值）`：主轴为水平方向，起点在左端。`row-reverse`：主轴为水平方向，起点在右端。`column`：主轴为垂直方向，起点在上沿。`column-reverse`：主轴为垂直方向，起点在下沿。

	```
	.wrapper{
		display: flex;
		justify-content: center;
		align-items: center;
	}
	```
	```
	.wrapper{
        text-align: center;
    }
    .A{
        display: inline-block;
        vertical-align: middle;
        background: red;
    }
    .null{
        display: inline-block;
        vertical-align: middle;
        width: 0;
        height: 100%;
    }//null为添加一个行级块元素给.A用于垂直对齐的。
	```

7. 清除浮动
	- 添加新的元素，应用 clear：both；添加一个新的子元素在后面，给它设置`clear:both`。
	- 父级div定义 `overflow: auto`,触发bfc。我们可以使用hiddent和auto值来清除浮动，但切记不能使用visible值。
	- 主流清除浮动方法：原理和第一种方法一样，但是使用伪元素可以不用在html添加，结构样式分离。而且用`zoom:1`兼容低版本ie
	
		```
		.box {
			zoom:1;
		} /*==for IE6/7 Maxthon2==*/
		.box:after {
			clear:both;
			content:'';
			display:block;
			width: 0;
			height: 0;
			visibility:hidden;
		}
		```
		
8. bfc
	- bfc全称是block format context —— 块级格式化上下文
	- 在BFC中,盒子都是从它的包含块的顶部一个一个的垂直放置的。两个相邻盒子的垂直间距决定于margin属性。在BFC中,两个相邻块级盒子之间垂直方向上的外边距是会塌陷的。
	- 在BFC中,每个盒子的左外边界挨着包含块的左边界(对于从右到左的格式化,则为右边界互相挨着)。即使是存在浮动元素也是如此(即使一个盒子的行盒是因为浮动而收缩了的),除非这个盒子建立了一个新的BFC(在某些情况下这个盒子自身会因为浮动而变窄)。
	
9. 单行打点

	``` 
	overflow: hidden; //溢出部分隐藏
	white-space: nowrap; //不换行
	text-overflow: ellipsis; //末尾打点
	```

10. 多行打点

	```
	overflow : hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp: 2;
	-webkit-box-orient: vertical;		
	```
	
11. 双飞翼布局(三栏布局)

	```
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
		<style>
			.wrapper,.left,.right{
				position: relative;
				height: 100px;
				float: left;
			}
			.wrapper{
				width: 100%;
			}
			.left{
				margin-left: -100%;
				background: red;
				width: 200px;
			}
			.right{
				background: blue;
				margin-left: -200px;
				width: 200px;
			}
			.main{
				margin-left: 200px;
	        	margin-right: 200px;
	        	height: 100px;
				background: yellow;
			}
		</style>
	</head>
	<body>
		<div class="wrapper">
			<div class="main"></div>
		</div>
		<div class="left"></div>
		<div class="right"></div>
	</body>
	</html>
	```