---
theme: qklhk-chocolate
highlight: srcery
---
## 开篇

最近把 《CSS揭秘》这本书又精读了一遍。简单介绍下这本书，这本书是 CSS 工作组的大佬成员写的一本书，某瓣评分也挺高。里面提到了很多有用的 CSS 技巧，第一次读的时候，粗略的翻了一遍，没怎么深究，这次又翻了出来再次研读。

对着书中的代码都敲了一下，又有了一些不一样的感受。结合最近遇到的一些 CSS 问题，读完以后竟然有种茅塞顿开的感觉，所以就挑选了其中一些可能大家会遇到的案例，先给大家展示一下，算是一个导读吧。当然因为本人水平有限，感兴趣的可以自行去读读这本书，强烈推荐啊！

## CSS编码原则

在开始分享书中一些有趣的案例之前，先来学习一下书中所提到的 CSS 的书写原则。什么是书写原则，我觉得就和写 JS 是一样的，就是写出来的代码要具有可维护性、可读性以及可复用性。应用在 CSS 身上，可以归纳为以下几点：

- 减少重复，Don‘t repeat yoursef，DRY原则。
- 结构分离，能使用 CSS 完成的不要用 HTML 来代替。
- 考虑 CSS 代码的可维护性，多用简写属性、善用继承。

首先减少重复很容易理解，应用在任何语言中都是一样的，不要去写重复的 CSS 代码，这样不仅会导致文件体积变大，可维护性也会变差，所以一般如果出现重复两次以上的属性，就可以抽离出来，做成类似 CSS 变量或者预处理器的 mixins。

我们经常会遇到写一个小箭头的需求，一般大家可能会用一个额外的 div 或者是 span来写这个箭头，但实际上这种 HTML 标签是没语义的，完全可以使用其他元素上的伪元素定位来替代，这样就不会占用额外的 HTML 标签了。还有类似什么做装饰作用的分割线什么的，都可以考虑使用伪元素来替代。这就是我们说的，能使用 CSS 完成的工作，尽量交给 CSS 来做，这样会让页面结构更加清晰。 

使用简写也很简单，CSS 中存在非常多的简写属性，它把多个属性合成为一个属性，这样不但减少了代码量而且如果我们要修改相关属性的时候也非常的方便，这同样也涉及到了下面提到的一个原则，可维护性原则。

可维护性原则是我重点想要说的，我觉得也是这本书中一直会体现的一个地方。CSS 的可维护性到底是什么呢，可以来看个简单的例子🌰

```css
.bad {
  padding: 6px 16px;
  border: 1px solid #446d88;
  background: #58a linear-gradient(#77a0bb, #58a);
  border-radius: 4px;
  box-shadow: 0 1px 5px gray;
  color: white;
  text-shadow: 0 1px 1px #335166;
  font-size: 20px;
  line-height: 30px;
}
```

![截屏2021-10-12下午6.57.06的副本.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/02dbfa45a78e4fe88c08c544b5d927c8~tplv-k3u1fbpfcp-watermark.image?)

这就是一个普通的按钮，现在假设需求变了，我们需要一个弹框中的确认和取消按钮，该怎么去修改，才能使修改的地方最少呢。对，修改的地方最少，这就是我们追求的地方。在 CSS 文件日益变得非常庞大的时候，当我们需要修改某些样式的时候，希望只修改其中的一处，或者几处，而不是到处去修改。

回到这个需求上来，如何把一个普通按钮修改为弹框的确认按钮，我们需要修改背景颜色，文字阴影，边框，大小等等内容，这就不符合上面提到的可维护性的原则了，所以得修改一下这段代码：

```css
div {
  font-size: 50px; /* 使用相对单位，只需要改动一处，就可以灵活改变按钮的大小 */
}
.good {
  display: inline-block;
  padding: .3em .8em;
  border: 1px solid rgba(0, 0, 0, .1);
  background: #58a linear-gradient(hsla(0,0%,100%,.2), transparent);
  border-radius: .2em;
  box-shadow: 0 0.05em 0.25em rgba(0,0,0,.5);
  color: white;
  text-shadow: 0 0.05em 0.05em rgba(0,0,0,.5);
  font-size: 125%;
  line-height: 1.5;
}
```

这里的区别就是使用了相对单位 em 代替了原先的像素的单位，目的注释中也提到了，就是为了修改方便，像上面的例子，只需要改动 div 的字体大小就可以随意改变按钮的大小。

除此之外，如果我们需要改变按钮的颜色，像上面提到的对话框中的确认和取消按钮，可以这么去修改：

```css
.ok {
  background-color: #6b0;
}
.cancel {
  background-color: #c00;
}
```

![截屏2021-10-14下午9.57.46.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cca4bd40e14848d9a9f709abdcb5ff2a~tplv-k3u1fbpfcp-watermark.image?)

如上，我们只需要加个背景颜色就可以了，这个平时大家应该都会用到。这里不一样的地方在于 background 属性写了两个颜色，其中一个还是渐变色，为什么要这么写呢？原因就在于按钮不止有背景色，还有边框、文字阴影、盒子阴影，这些都需要随着背景色的变化而变化，所以在可维护的写法中可以看到，这些属性都变成了半透明。这样背景色覆盖在上面，就不需要单独修改这些属性了。而 background 中使用了两个背景色，其实是按钮的主色 #58a 这么一个颜色值作为主色调，另外通过 hsla 对比度饱和度调整形成一个主色调的明暗效果。关于多重背景的应用，后续还会说到。

另外可维护性还体现在简写属性上，这个也是显而易见的，简写属性为几个属性的复合写法，不但减少了代码量，同时修改起来也会非常的方便，

说完了这些，来看看在实际项目中如何写出一个好的 CSS 呢。

## CSS复杂背景的应用

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a83e98fcaa1745f6947c035b58a58245~tplv-k3u1fbpfcp-watermark.image?)

### 条纹背景

如何写出上图所示的复杂的背景呢，条纹背景，网格背景、棋盘背景等等。除了用图片来替代这些背景，我们也可以使用纯 CSS 来实现，这样就不需要额外的图片结构，不需要多余的带宽，也符合上面提到的能用 CSS 来解决的就不要使用一些影响页面结构的 DOM 元素来解决。

办法就是一个平时可能很少用到的一个属性：线性渐变 linear-gradient。这里就是利用了渐变属性 linear-gradient 的一个特性，**后一个渐变颜色的范围如果是0的话，那么这个渐变颜色将不会有过渡，而是直接从前一个颜色到后一个颜色**。什么意思呢，来看一个条纹背景的实现思路就明白了。

```css
background: linear-gradient(#fb3 50%, #58a 0);
```

就这么简单，一行足以，来看看效果

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dcf4ec4de5f44c99a724d1ce84bc13ed~tplv-k3u1fbpfcp-watermark.image?)

好像不是我们要的条纹，修改一下

```css
background: linear-gradient(#fb3 50%, #58a 0);
/* 利用了背景平铺的效果 */
background-size: 100% 30px;
```

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bd4b1247ab1d4825bdbac440fdd86c16~tplv-k3u1fbpfcp-watermark.image?)

是不是有点意思了，想要个垂直的条纹呢，改变下渐变的方向就可以了

```css
background: linear-gradient(to right, #fb3 50%, #58a 0);
background-size: 30px 100%;
```

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b9802ea071034cc8abc23153f3849b75~tplv-k3u1fbpfcp-watermark.image?)

也成了，那如果想要一个斜向的条纹呢，是不是像下面这么写就可以了：

```css
background: linear-gradient(45deg, #fb3 50%, #58a 0);
background-size: 30px 30px;
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/04c8ff5567f247d388b00c084b1cbad7~tplv-k3u1fbpfcp-watermark.image?)

不对啊，为什么会是这样的呢？这和我们想象的不一样啊。实际渐变中旋转的45度，只是旋转了当前的一小块背景的45度

![image-20211113134217014](/Users/qiugu/Library/Application Support/typora-user-images/image-20211113134217014.png)

就是上面红色框选出来的一小段的45度，我们需要的效果应该是这样的

![image-20211113134519254](/Users/qiugu/Library/Application Support/typora-user-images/image-20211113134519254.png)

可以看到红色框中的颜色变化了4次，并不是两次，找到了问题所在，改下上面的代码

```css
background: linear-gradient(45deg,#fb3 25%, #58a 0, #58a 50%, #fb3 0, #fb3 75%, #58a 0);
background-size: 30px 30px;
```

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c1bdf7bbf83843fab41387587151f9c8~tplv-k3u1fbpfcp-watermark.image?)

好了，大功告成，所以有时候写 CSS 还得有一颗细心观察的心。

然后再说到网格背景、棋盘背景，波点背景什么的，心里是不是有底了。这里就不在去深入分析，不过注释上面会提到一些值得注意的地方，可以照着下面的代码，自己敲一敲感受一下。

### 网格背景

  ```css
  /* 网格图案，写法参照DRY，改动颜色只需要改动background-image这里就可以 */
  background: white;
  /* 利用了background-image多重背景，一个是横向的渐变，一个是纵向的渐变 */
  /* 另外这里的长度都是百分比单位，也可以写成像素单位，这样就变成了最开始的第二个图案 */
  background-image: linear-gradient(90deg, rgba(200,0,0,.5) 50%, transparent 0),
  linear-gradient(rgba(200,0,0,.5) 50%, transparent 0);
  background-size: 30px 30px;
  ```

### 波点图案

  ```css
  /* 波点图案则是使用了径向渐变来实现 */
  background: #655;
  background-image: radial-gradient(tan 30%, transparent 0),
  radial-gradient(tan 30%, transparent 0);
  background-size: 30px 30px;
  /* 这里做了偏移，否则就是第一个波点图案了 */
  /* 这里的偏移位置需要为background-size设置的一半，也就是每个贴片偏移到中间位置 */
  background-position: 0 0, 45px 45px;
  ```

### 棋盘图案

  ```css
  /* 棋盘图案，这里使用了透明度来表示棋盘的深浅色，替换background主色即可改变其他色系的棋盘 */
  background: #eee;
  background-image: linear-gradient(45deg, rgba(0,0,0,.25) 25%, transparent 0),
  linear-gradient(45deg, transparent 75%, rgba(0,0,0,.25) 0),
  linear-gradient(45deg, rgba(0,0,0,.25) 25%, transparent 0),
  linear-gradient(45deg, transparent 75%, rgba(0,0,0,.25) 0);
  background-position: 0 0, 15px 15px, 15px 15px, 30px 30px;
  background-size: 30px 30px;
  ```

### 角向渐变创建棋盘图案

  ```css
  /* 角向渐变轻松创建棋盘图案，注意兼容性 */
  background: #58a repeating-conic-gradient(rgba(0,0,0,.25) 0 25%, rgba(0,0,0,.5) 0 50%);
  background-size: 30px 30px;
  ```

## CSS复杂图形

所谓复杂图形，就是一些不规则的图形，例如平行四边形、梯形、菱形、椭圆以及切角图形等。平时我们遇到的话可能都是用 div 来拼凑出来一个图形，那用纯 CSS 如何去实现呢？

### 平行四边形

平行四边形就是利用了变形当中的属性 skew 来实现的，但是变形会导致内容也会跟着变形，这在大部分情况下不是我们想要的。于是可以用伪元素来实现变形，元素本身抵消变形来达到效果。看看代码是如何实现的：

```css
div {
  position: relative;
  background-color: transparent;
}
div::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: #58a;
  transform: skewX(-45deg);
  z-index: -1;
}
```

来看看成果：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a6ca3a164f5f423185e5c8f4ba36c139~tplv-k3u1fbpfcp-watermark.image?)

### 菱形

菱形的实现和平行四边形的原理是差不多的，用的 rotate 来旋转45°。也可以拓展另外一种写法，利用非常强大的 clip-path 来做：

```css
clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);
```
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c69ca6d3d0eb4d0eb16719431144ef8d~tplv-k3u1fbpfcp-watermark.image?)

兼容性方面，clip-path IE10 以下都不支持，其他的基本都没有问题。

那上面的平行四边形是不是也可以用 clip-path 来实现呢：

```css
width: 270px;
height: 70px;
clip-path: polygon(70px 0, 270px 0, 200px 70px, 0 70px);
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1cdbfe4545524c3b89f601e5992b7a19~tplv-k3u1fbpfcp-watermark.image?)

实现是实现了，而且也不需要再次变形抵消，只是我们设置的宽度是270，实际展示出来的宽度是200，因为 clip-path 没办法超出给定容器的宽高。

### 梯形

先在脑海中想象一下梯形的样子，把矩形放在三维空间，然后底部旋转一定角度，从二维空间观看。下面的动态图演示了这个原理：

![2.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0d54e43cd8f8459787cccf3e5a8774be~tplv-k3u1fbpfcp-watermark.image?)

其实这里的动图已经展示了 CSS 该如何去完成这个工作。首先就是绕着底部做旋转，还要添加一个 3d 的效果：

```css
div {
  position: relative;
  padding: .5em 1em .35em;
  color: #fff;
}
div::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  /* 利用3d变化后的二维视角形成一个梯形，但同时存在内部文本位置不正确的问题 */
  /* 使用scaleY来放大高度消除文本位置不正确的问题 */
  transform: scaleY(1.3) perspective(.3em) rotateX(5deg);
  background: #58a;
  z-index: -1;
  /* 固定3d旋转的底部，绕底部进行旋转 */
  transform-origin: bottom; /* 如果是直角梯形的话，则这里改为left或者right */
}
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1cdbfe4545524c3b89f601e5992b7a19~tplv-k3u1fbpfcp-watermark.image?)

### 椭圆

椭圆的实现相比上面就显得有些平平无奇了，主要就是利用了 border-radius 来实现的，了解这个属性的同学应该知道，这个属性是个复合属性，总共可以有8个值，用斜线分开，斜线前面是水平方向的四个圆角，后面则是垂直方向的四个圆角。顺序都是左上->右上->右下->左下。

```css
div {
  background-color: red;
  width: 200px;
  height: 130px;
  /* border-radius的百分比是基于宽度和高度来计算的，而不是只根据宽度，所以可以生成一个自适应的椭圆 */
  border-radius: 50%;
}
```

沿 X 轴的半椭圆

```css
border-radius: 50% / 100% 100% 0 0;
```

沿 Y 轴的半椭圆

```css
border-radius: 100% 0 0 100% / 50%;
```

四分之一椭圆

```css
border-radius: 100% 0 0 0;
```

下面就是效果图了，椭圆实现还是很简单的，重要的一点就是自己要多写写，感受一下每个值分别代表了哪个圆角，这点很重要。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/100cd4ce4e864f919c8a1f548373cf59~tplv-k3u1fbpfcp-watermark.image?)

### 饼图

饼图应该是比上面的图形都要常用的，平时开发的时候，一般直接都是用图表库来实现的，但是如果就是简单展示一个百分比的饼图，引入一个图表库又没有必要，这个时候就可以选择用 CSS 来实现

```css
div {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  /* 这里的效果就是左边一半是黄绿色，右边一半是渐变实现的#655的颜色 */
  background-color: yellowgreen;
  background-image: linear-gradient(to right, transparent 50%, #655 0);
}
/* 利用伪元素覆盖在上面，形成一个百分比的效果 */
div::before {
  content: '';
  display: block;
  /* 必须要有高度，不然显示不出来 */
  height: 100%;
  background-color: inherit;
  margin-left: 50%;
  /* 就是上面的椭圆的实现原理实现的一个半圆遮罩 */
  border-radius: 0 100% 100% 0 / 50%;
  /* 超过50%的比例会有问题，绿色部分占据了超过50%的部分，因此反过来处理即可 */
  transform: rotate(.2turn);
  transform-origin: left;
}
```

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b63a878a04d441849146bd61b9bfa675~tplv-k3u1fbpfcp-watermark.image?)

这个是我们的最终效果图，先来一步一步看看这个效果是如何实现的。首先去掉伪元素 before 的样式：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38ebbc0908da4876aa2ba030713b4398~tplv-k3u1fbpfcp-watermark.image?)

就如上面注释所说，是 yellowgreen 和 #655 各占 50% 的分布，接下来写上 before 的基本配置，display、height、background-color，为了方便理解，把 background-color 改成红色，再来看看是什么样子的：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7af08d4c2868483ea3113e58ddb6644c~tplv-k3u1fbpfcp-watermark.image?)

就是一个红色的矩形覆盖在原本的 div 上面，然后加上 margin、border-radius再来看看

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b6f42894e72d4e58af21bca47221c590~tplv-k3u1fbpfcp-watermark.image?)

上图左边就是加上了 margin-left 的效果，右边则是又加上了 border-radius 的效果，关于这个半圆，是不是就用到了上面的椭圆实现呢。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/794077e785134791863bfc8574576b6a~tplv-k3u1fbpfcp-watermark.image?)

最后加上了 rotate 旋转一下，改变一下旋转的中心点，改到左边，就完成了，最后背景颜色改成 yellowgreen 其实就是一开始的效果了。不过这种实现也存在问题，就是对于超过 50% 比例的饼图这么写就无能为力了，考虑下面的方法来修复一下：

```css
/* 设置一个和饼图相反的颜色，0.1就对应50%+0.1，也就是60% */
background-color: #655;
transform: rotate(.1turn);
transform-origin: left;
```

这看起来还是比较繁琐的，有没有简单一点的呢？有，用 svg 也可以实现，看起来也非常容易理解：

```html
<svg viewBox="0 0 32 32">
  <!-- 这里取16的半径，是希望周长为100，这样计算比例的时候，直接就可以得到，不需要进行计算 -->
  <circle r="16" cx="16" cy="16"></circle>
</svg>
```

```css
svg {
  /* 旋转一下位置，让顶部成为饼图的起点位置 */
  transform: rotate(-90deg);
  background: yellowgreen;
  border-radius: 50%;
}
circle {
  fill: yellowgreen;
  stroke: #655;
  /* 32就是圆的直径，也就是形成一个半圆 */
  stroke-width: 32;
  /* 第一个参数表示虚线边框的长度，第二个参数则表示每个虚线段之间的距离，这里就是圆的周长 2 * Π * 16约等于100 */
  stroke-dasharray: 38 100;
}
```

这里简单解释下实现原理，利用 circle 的虚线边框，设置 stroke 的宽度为圆的直径，这样边框就占据了整个圆，然后调整虚线段的长度，以及保持虚线段的间隔为一整个圆，这样只会有一条虚线段，通过调整虚线段的宽度，就是饼图中灰色的比例了。看起来还是挺复杂的，看个动图来理解一下就明白了：

![2.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/54d518ba61854c25b921bc45a5ba54a7~tplv-k3u1fbpfcp-watermark.image?)

这样的话，当需要改变饼图比例的时候，只需要调整 stroke-dasharray 的第一个参数就可以了，要比上面的写法更容易维护了，但是用到了 svg 的结构，还是不太好。还有更简单的方法了吗？

有，就是角向渐变，非常简单，就是存在兼容性问题。

```css
div {
  line-height: 100px;
  text-align: center;
  /* 利用角向渐变，实现更加简单，注意兼容性问题，IE全部不支持 */
  background: conic-gradient(#655 80%, yellowgreen 0);
}
```

### 切角折角

先看下什么是折角

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b2139cc54229419a818695ca23aab5bb~tplv-k3u1fbpfcp-watermark.image?)

可以看到就是把纸张一角给折起来了，而切角则是把这个角给切掉了，类似下图这种：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b7c158077f304d619c9eac38018f6345~tplv-k3u1fbpfcp-watermark.image?)

这还不简单嘛，写个三角形定位到边角的地方盖住不就可以了。确实可以这么做，但还是记住我们上面提到的 CSS 编码规范，能用纯 CSS 实现的就不要利用额外的 HTML 元素来做。

这里折角和切角的实现说起来，非常有意思，又是使用了上面提到过的 linear-gradient 的线性渐变来实现的。这次先来看看成果图：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/81f4be4fdf0144bfae6725a4d703a0a7~tplv-k3u1fbpfcp-watermark.image?)

还记得上面提到的斜向条纹吗，切角的实现原理和它是类似的

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b8f514dbbd3240c0b754b97bc19bb2fe~tplv-k3u1fbpfcp-watermark.image?)

只要把右下角那一块变成 transparent 就能实现切角了：

```css
/* 作为一种回退机制，如果渐变效果无用，则使用普通的背景 */
background: #58a;
/* 一个切角的效果 */
background: linear-gradient(-45deg, transparent 15px, #58a 0);
```

还有多个切角的效果，则是利用了多重背景来实现

```css
background: #58a;
background: linear-gradient(45deg, transparent 15px, #58a 0) bottom left,
linear-gradient(-45deg, transparent 15px, #58a 0) bottom right,
linear-gradient(135deg, transparent 15px, #58a 0) top left,
linear-gradient(-135deg, transparent 15px, #58a 0) top right;
/* 四个切角则每个背景占据1/4 */
background-size: 50% 50%;
/* 还要关闭平铺效果，不然重复平铺还是会导致覆盖 */
background-repeat: no-repeat;
```

而折角和切角原理是一样的，只是多了一层背景：

```css
background: #58a;
/* 折角就是利用上面的切角原理，再添加一层渐变背景覆盖在对应位置，形成折叠的效果 */
background: linear-gradient(to left bottom, transparent 50%, rgba(0,0,0,.4) 0) no-repeat 100% 0 / 2em 2em,
linear-gradient(-135deg, transparent 1.5em, #58a 0);
```

上面实现的切角折角都是45°的角，对于非45°的角则需要进行计算：

```css
background: #58a;
/* 非45°角的话，这里就是30-60-90的直角三角形，需要利用三角函数计算两个直角边的大小 */
/* 计算公式，长直角边 = 半径1.5 * sin30° = 1.5 / 0.5 = 3 短直角边 = 半径1.5 * 2 / 根号3 = 根号3 约等于1.73  */
background: linear-gradient(to left bottom, transparent 50%, rgba(0,0,0,.4) 0) no-repeat 100% 0 / 3em 1.73em,
linear-gradient(-150deg, transparent 1.5em, #58a 0);
```

## 结尾

写着写着，发现内容有点太多了，就删除了很多内容，像动画和文字效果等，不过整个学习的笔记还是记录下来了，放在了下面的仓库中，大家可以参考一下。还有很多书中提及的一些有趣的例子，比如有之前看到过的图片对比工具，像下面这样的：

![2.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3781e9376af94364822103bc5b6c4adf~tplv-k3u1fbpfcp-watermark.image?)

还有经常可以在终端中看到的毛玻璃的效果：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f4bbf02908cf4c179c6a639ae0eace12~tplv-k3u1fbpfcp-watermark.image?)

最后想了一下，这篇文章算什么呢，读后感，导读，还是其他什么都不重要了，重要的是，我在其中有了自己的理解，CSS 永远比我们想象的能做的多的多，有些属性以为它只有一个作用，实际能做的事非常多。另外 CSS 的`原则`也很重要，也是自己从这本书中总结出来的，它确实打破了我以前写 CSS 随意任性的想法，写出来的 CSS 代码繁琐冗余不说，维护起来更是痛不欲生。如果真的把这些原则融会贯通，那这本书的价值就体现出来了吧。

<small>[完整的demo地址](https://github.com/qiugu/Front-end-learning-route/tree/master/src/css)</small>
