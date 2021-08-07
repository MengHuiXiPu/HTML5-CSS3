## 最新的CSS技术

前端技术日新月异，我们需要不断学习来更新自己的前端知识并运用到自己的项目中。这次笔者整理一些未来普及或者现在同学们可能已经用到的CSS特性，包括SVG图标、滚动特性、CSS自定义属性、CSS现代伪类 、JS in CSS、Web Layout、混合模式和滤镜、CSS计数器等等。

## 滚动特性

在**能用CSS实现的就不用麻烦JavaScript**[1]文章提及到滚动捕捉的特性，更多有关于容器滚动方面的CSS新特性其实还有有很多个，比如：

- 自定义滚动条的外观
- `scroll-behavior`指容容器滚动行为，让滚动效果更丝滑
- `overscroll-behavior`优化滚动边界，特别是可以帮助我们滚动的穿透

### 自定义滚动条的外观

默认的window外观和mac外观

windows![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/H8M5QJDxMHoQHAMYb13L4VOSA0picibp0oWRGeakzeIvD8r5VGQ3PaicPPHmLghlODmlP0ThfaeTCOdQrQIHqaKVQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)mac

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/H8M5QJDxMHoQHAMYb13L4VOSA0picibp0ouMaGreticWsa2kCANk0odsSic5mzG6PbyNQh84yfSWAp2WjibsqGKV01g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)image.png

在CSS中，我们可以使用`-webkit-scrollbar`来自定义滚动条的外观。该属性提供了七个伪元素：

- `::-webkit-scrollbar`：整个滚动条
- `::-webkit-scrollbar-button`：滚动条上的按钮（下下箭头）
- `::-webkit-scrollbar-thumb`：滚动条上的滚动滑块
- `::-webkit-scrollbar-track`：滚动条轨道
- `::-webkit-scrollbar-track-piece`：滚动条没有滑块的轨道部分
- `::-webkit-scrollbar-corner`：当同时有垂直和水平滚动条时交汇的部分
- `::-webkit-resizer`：某些元素的交汇部分的部分样式（类似`textarea`的可拖动按钮）

```
html {
    --maxWidth:1284px;
    scrollbar-color: linear-gradient(to bottom,#ff8a00,#da1b60);
    scrollbar-width: 30px;
    background: #100e17;
    color: #fff;
    overflow-x: hidden
}

html::-webkit-scrollbar {
    width: 30px;
    height: 30px
}

html::-webkit-scrollbar-thumb {
    background: -webkit-gradient(linear,left top,left bottom,from(#ff8a00),to(#da1b60));
    background: linear-gradient(to bottom,#ff8a00,#da1b60);
    border-radius: 30px;
    -webkit-box-shadow: inset 2px 2px 2px rgba(255,255,255,.25),inset -2px -2px 2px rgba(0,0,0,.25);
    box-shadow: inset 2px 2px 2px rgba(255,255,255,.25),inset -2px -2px 2px rgba(0,0,0,.25)
}

html::-webkit-scrollbar-track {
    background: linear-gradient(to right,#201c29,#201c29 1px,#100e17 1px,#100e17)
}
复制代码
```

通过这几个伪元素，可以实现你自己喜欢的滚动条外观效果，比如下面这个示例：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/H8M5QJDxMHoQHAMYb13L4VOSA0picibp0ojGyaDkpKSYbGjTz0YicuayLkdBdvLpzK3mtg1jrqv0XS0Jl7ZGn543w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)image.png

**完整演示**[2]

## css自定义属性

你大概已经听说过CSS自定义属性，也被称为 CSS 变量，估计熟悉SCSS、LESS就会很快上手，概念大同小异，都是让我们的CSS变得可维护，目前Edge最新版都已经支持这个特性了，这说明现在 CSS 自定义属性已经能用在实际项目中了，相信不久以后开发者们将大大依赖这个特性。但还请在使用之前请先检查一下本文附录中 Postcss 对于 CSS 自定义属性的支持情况，以便做好兼容。

什么是自定义属性呢？简单来说就是一种开发者可以自主命名和使用的 CSS 属性。浏览器在处理像 color 、position 这样的属性时，需要接收特定的属性值，而自定义属性，在开发者赋予它属性值之前，它是没有意义的。所以要怎么给 CSS 自定义属性赋值呢？这倒和习惯无异

```
.foo {
  color: red;
  --theme-color: gray;
}
复制代码
```

自定义元素的定义由 -- 开头，这样浏览器能够区分自定义属性和原生属性，从而将它俩分开处理。假如只是定义了一个自定义元素和它的属性值，浏览器是不会做出反应的。如上面的代码， `.foo` 的字体颜色由 `color` 决定，但 `--theme-color` 对 `.foo` 没有作用。

你可以用 CSS 自定义元素存储任意有效的 CSS 属性值

```
.foo {
  --theme-color: blue;
  --spacer-width: 8px;
  --favorite-number: 3;
  --greeting: "Hey, what's up?";
  --reusable-shadow: 0 3px 1px -2px rgba(0, 0, 0, 0.85);
}
复制代码
```

### 使用

假如自定义属性只能用于设值，那也太没用了点。至少，浏览器得能获取到它们的属性值。

使用 `var()` 方法就能实现：

```
.button {
  background-color: var(--theme-color);
}
复制代码
```

下面这段代码中，我们将 `.button` 的 `background-color` 属性值赋值为 `--theme-color` 的值。这例子看起来自定义属性也没什么了不起的嘛，但这是一个硬编码的情况。你有没有意识到，`--theme-color` 的属性值是可以用在任意选择器和属性上的呢？这可就厉害了。

```
.button {
  background-color: var(--theme-color);
}
 
.title {
  color: var(--theme-color);
}
 
.image-grid > .image {
  border-color: var(--theme-color);
}
复制代码
```

### 缺省值

如果开发者并没有定义过 --theme-color 这个变量呢？var() 可以接收第二个参数作为缺省值：

```
.button {
  background-color: var(--theme-color, gray);
}
复制代码
```

> 注意：如果你想把另一个自定义属性作为缺省值，语法应该是 background-color: var(--theme-color, var(--fallback-color))

传参数时总是传入一个缺省值是一个好习惯，特别是在构建 web components 的时候。为了让你的页面在不支持自定义属性的浏览器上正常显示，别忘了加上兼容代码：

```
.button {
  background-color: gray;
  background-color: var(--theme-color, gray);
}
复制代码
```

## CSS现代伪类

这些最新的伪类特性，我们也需要知道。![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/H8M5QJDxMHoQHAMYb13L4VOSA0picibp0orPd3JJTbmz7E172wCcxcyl20bZaOujAPQwKdzz1v9iaf4DVo203ThTQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 使用 :is() 减少重复

你可以使用 `:is()` 伪类来删除选择器列表中的重复项。

```
/* BEFORE */
.embed .save-button:hover,
.attachment .save-button:hover {
  opacity: 1;
}

/* AFTER */
:is(.embed, .attachment) .save-button:hover {
  opacity: 1;
}
复制代码
```

此功能主要在未处理的标准CSS代码中有用。如果使用Sass或类似的CSS预处理程序，则可能更喜欢嵌套。

**注意**：浏览器还支持非标准的 `:-webkit-any()` 和 `:-moz-any()` 伪类，它们与 `:is()` 相似，但限制更多。WebKit在2015年弃用了 `:-webkit-any()` ，Mozilla已将Firefox的用户代理样式表更新为使用 `:is()` 而不是 `:-moz-any()`。

### 使用 :where() 来保持低特殊性

`:where()` 伪类与 `:is()` 具有相同的语法和功能。它们之间的唯一区别是 `:where()` 不会增加整体选择器的特殊性（即某条CSS规则特殊性越高，它的样式越优先被采用）。

> `:where()` 伪类及其任何参数都不对选择器的特殊性有所帮助，它的特殊性始终为零

此功能对于应易于覆盖的样式很有用。例如，基本样式表 sanitize.css 包含以下样式规则，如果缺少 `<svg fill>` 属性，该规则将设置默认的填充颜色：

```
svg:not([fill]) {
  fill: currentColor;
}
复制代码
```

由于其较高的特殊性（B = 1，C = 1），网站无法使用单个类选择器（B = 1）覆盖此声明，并且被迫添加 `!important` 或人为地提高选择器的特殊性（例如 `.share- icon.share-icon`）。

```
.share-icon {
  fill: blue; /* 由于特殊性较低，因此不适用 */
}
复制代码
```

CSS库和基础样式表可以通过用 `:where()` 包装它们的属性选择器来避免这个问题，以保持整个选择器的低特殊性(C=1)。

```
/* sanitize.css */
svg:where(:not([fill])) {
  fill: currentColor;
}

/* author stylesheet */
.share-icon {
  fill: blue; /* 由于特殊性较高，适用 */
}
复制代码
```

其它新伪类特性有情趣同学可以按照导图查阅一下相关文档资料。

**完整演示**[3]

## JS in CSS

前面提到过，使用CSS自定义属性的时候，可以通过JavaScript来操作自定义属性的值。其实还可以更强大一点，如果你对CSS Houdini熟悉的话，可以借助其特性，直接在CSS的代码中来操作CSS自定义属性

```
:root {
  --property: document.write('hello world!');
}
复制代码
window.onload = () => {
  const doc = window.getComputedStyle(document.documentElement);
  const cssProp = doc.getPropertyValue('--property');
  new Function((cssProp))();
}
复制代码
```

**完整演示**[4]

## Web layout

对于Web布局而言，前端就一直在探讨这方面的最优方式。早期的`table`布局，接着的`float`和`position`相关的布局，多列布局，Flexbox布局和Grid布局等。Flexbox和Grid的出现，Web布局的灵活性越来越高。

如图不依赖媒体查询实现自动计算

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/H8M5QJDxMHoQHAMYb13L4VOSA0picibp0oUL5MkIMbBibOfKhxAnEPgGjSlSYNiac74qqL5FcRoCSWDX53rkbhnictg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)屏幕录制2021-07-27 下午3.17.46.gif

CSS Grid中提供了很多强大的特性，比如：

- `fr`单位，可以很好的帮助我们来计算容器可用空间
- `repeat()`函数，允许我们给网格多个列指定相同的值。它也接受两个值：重复的次娄和重复的值
- `minmax()`函数，能够让我们用最简单的CSS控制网格轨道的大小，其包括一个最小值和一个最大值
- `auto-fill`和`auto-fit`，配合`repeat()`函数使用，可以用来替代重复次数，可以根据每列的宽度灵活的改变网格的列数
- `max-content`和`min-content`，可以根据单元格的内容来确定列的宽度
- `grid-suto-flow`，可以更好的让CSS Grid布局时能自动排列

结合这些功能点，布局会变得更轻松。比如我们要实现一个响应式的布局，很多时候都会依赖于媒体查询（`@media`）来处理，事实上，有了CSS Grid Layout之后，这一切变得更为简单，不需要依赖任何媒体查询就可以很好的实现响应式的布局。特别是当今这个时代，要面对的终端设备只会增加不会减少，那么希望布局更容易的适配这些终端的布局，那么CSS Grid Layout将会起到很大的作用。

**完整示例**[5]

Grid和flex都是面向未来的最佳布局方案。我们不应该探讨谁优谁劣，而是应该取长补短结合使用。

## 混合模式和滤镜

**能用CSS实现的就不用麻烦JavaScript — Part2**[6]一文提到混合模式。CSS混合模式和滤镜主要是用来处理图片的。熟悉PS之类软件的同学很容易理解里面的属性。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/H8M5QJDxMHoQHAMYb13L4VOSA0picibp0ocqzQBu42tpm68vdMub7wlwKV35ZLtELjibwVovUO0h7gl7I2FVaRgOw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)屏幕录制2021-07-19 上午11.12.39.gif

**完整代码演示**[7]

## CSS计数器

CSS计数器其实涉及到三个属性：`counter-increment`、`counter-reset`和`counter()`。一般情况都是配合CSS的伪元素`::before`和`::after`的`content`一起使用。可以用来计数

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/H8M5QJDxMHoQHAMYb13L4VOSA0picibp0oMkIIAicTDAVjDEibL0jskcZje19LwM18aFUiboCFbx8R6dibyPdKGQ8K0w/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)屏幕录制2021-07-27 下午3.15.06.gif

**完整演示**[8]

## SVG图标

对于SVG而言，它是一套独立而又成熟的体系，也有自己的相关规范（Scalable Vecgtor Graphics 2），即 SVG2。虽然该规范已经存在很久了，但很多有关于SVG相关的特性在不同的浏览器中得到的支持度也是有所不一致的。特别是SVG中的渐变和滤镜相关的特性。不过，随着技术的革新，在Web的应用当中SVG的使用越来越多，特别是SVG 图标相关的方面的运用。

- 最早通过`<img>`标签来引用图标（每个图标一个文件）

- 为了节省请求，提出了Sprites的概念，即**将多个图标合并在一起，使用一个图片文件**，借助`background`相关的属性来实现图标

- 图片毕竟是位图，面对多种设备终端，或者说更易于控制图标颜色和大小，开始在使用**Icon Font**来制作Web图标

- 当然，字体图标是解决了不少问题，但每次针对不同的图标的使用，需要自定义字体，也要加载相应的字体文件，相应的也带了一定的问题，比如说跨域问题，字体加载问题

- 随着SVG的支持力度越来越强，大家开始在思考SVG，使用SVG来制作图标。该技术能解决我们前面碰到的大部分问题，特别是在而对众多终端设备的时候，它的优势越发明显

- - SVG和`img`有点类似，我们也可以借助`<symbol>`标签和`<use>`标签，将所有的SVG图标拼接在一起，有点类似于Sprites的技术，只不过在此称为SVG Sprites

```
<!-- HTML -->
<svg width="0" height="0" display="none" xmlns="http://www.w3.org/2000/svg">
    <symbol id="half-circle" viewBox="0 0 106 57">...</symbol>
    <!-- .... -->
    <symbol id="icon-burger" viewBox="0 0 24 24">...</symbol>
</svg>
复制代码
```

SVG Sprites和img Sprites有所不同，SVG Sprites就是一些代码（类似于HTML一样），估计没有接触过的同学会问，SVG Sprites对应的代码怎么来获取呢？其实很简单，可以借助一些设计软件来完成，比如Sketch。当然也可以使用一些构建工具，比如说`svg-sprite`。有了这个之后，在该使用的地方，使用`<use>`标签，指定`<symbol>`中相应的`id`值即可，比如：

```
<svg class="icon-nav-articles" width="26px" height="26px">
    <use xlink:href="#icon-nav-articles"></use>
</svg>
复制代码
```

使用SVG的图标还有一优势，我们可以在CSS中直接通过代码来控制图标的颜色：

```
.site-header .main-nav .main-sections>li>a>svg {
    // ...
    fill: none;
    stroke-width: 2;
    stroke: #c2c2c2;
}
.site-header .main-nav:hover>ul>li:nth-child(1) svg {
    stroke: #ff8a00;
}
复制代码
```

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/H8M5QJDxMHoQHAMYb13L4VOSA0picibp0oefEAmh5w33O8IzysfHfwYNZw3jNJujf8zDicbwc96mQXEDD7fmBYhkQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)image.png

**完整演示**[9]

## 写在最后

以上列举都是CSS一些优秀的特性。还有很多，有时间再收集更多分享给大家。这些新特性在不同的浏览器中差异性是有所不同的。但这并不是阻碍我们去学习和探索的原因所在。我们应该及时去了解并运用到，才可以做到对项目精益求精。

 

The End

 