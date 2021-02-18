## rem布局方案

## 一些像素概念

- 物理像素：即实际的每一个物理像素，也就是移动设备上每一个物理显示单元（点）
- 设备逻辑像素（`css`中的`px`）：可以理解为一个虚拟的相对的显示块，与物理像素有着一定的比例关系，也就是下面的设备像素比
- 设备像素比（`dpr`）：= 物理像素 / 设备独立像素(`px`)

> 如果`dpr`为`1`的话，那么`1px = 1物理像素`，`x`轴`y`轴加起来就是`1`

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/zHYsKHjf0ngqIZUhlTMkCIJYkRYD0ejwtrQOV3qZXaCia72tNEss3YJg3LmHVoPSAkXFe0bvQibYNjpYymaCq7cg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> 如果`dpr`为`2`的话，那么`1px = 2`物理像素，`x`轴`y`轴加起来就是`4`

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/zHYsKHjf0ngqIZUhlTMkCIJYkRYD0ejwa8hl0fJ9a882R7JKibiaRYpqkQFbA0ic8143gOwsbpmliaia31LkaUtuAVA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**以此类推**

> 在js中可以通过`window.devicePixelRatio`获取当前设备的`dpr`。

这里说明一下，无论`dpr`多大，`1px`的大小通常来说是一致的，这也就意味着，随着`dpr`的增大，物理像素点会越来越小，这样才能容纳更多的物理像素，才能更高清，更`retina`

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

说完基本概念，来说一下几个问题：

## retina屏图片模糊

> 首先普及一下位图像素：一个位图像素是图片的最小数据单元，每一个单元都包含具体的显示信息（色彩，透明度，位置等等）

那为什么在dpr高的retina屏上反而会模糊呢？看图~

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

> 在`1dpr`的屏幕上，位图像素和物理像素一一对应没什么问题，但是在`retina`屏上，由于一个`px`由`4`个甚至更多的物理像素组成，并且单个位图像素不能进一步分割，所以会出现就近取色的情况，如果取色不均，那么就会导致图片模糊。

> 对于这种情况，只能采用`@2x`、`@3x`这样的倍图来适配高清展示，这样侧向说明了为什么照着`iphone6`做的`ui`稿不是`375`，而是`750`的问题。

虽然这样在`dpr`为`1`的屏幕上会导致`1`个物理像素上有`4`个位图像素，但是这种情况的取色算法更优，影响不大，不做讨论。

## 1px的粗细问题

> 由于`1px`的实际大小是一样的，只是里面的物理像素数量不同，所以如果直接写`1px`是没问题的，不会出现粗细不同的情况，但是这样一来`retina`的优势也`rem`的作用也就没了，其实还是`dpr`的问题，`dpr`为`1`，那么`1px`就是一个物理像素，但是在`retina`中。`1px`实际可能有`4`、`9`个物理像素，`ui`想要的其实是`1`个物理像素，而不是1px，不过由于不是素所有的手机都能适配`0.x`，所以曲线救国，采用`scale`缩放或者设置`meta`都可以

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## viewport

### 三个概念

- `layout viewport`
- `visual viewport`
- `ideal viewport`

**layout viewport**

> 最开始，`pc`上的页面是无法再移动端正常显示的，因为屏幕太小，会挤作一团，所以就有了`viewport`的概念，又称布局视口（虚拟视口），这个视口大小接近于`pc`，大部分都是`980px`

**visual viewport**

> 有了布局视口，还缺一个承载它的真是视口，也就是移动设备的可视区域-视觉视口（物理视口）,这个尺寸随着设备的不同也有不同。这样在视觉视口中创建了一个布局视口，类似`overscroll:scroll;`这样，可以通过滚动拖拽、缩放扩大进行较好的访问体验

**ideal viewport**

> 像上面的体验在早些年可能比较多，但是近几年几乎很少了，还是归咎于用户体验，所以，我们还需要一个视口-理想视口（同样是虚拟视口），不过这个理想视口的大小是等于布局视口的，这样用户就能得到更好的浏览体验。

### 一个特性

> `viewport`有六种可以设置的常用属性：

- `width`：定义`layout viewport`的宽度，如果不设置，大部分情况下默认是`980`
- `height`：非常用
- `initial-scale`：可以以某个比例将页面缩放\放大，你也可以用它来设置`ideal viewport`：

```
  <meta name='viewport' content='initial-scale=1' />
```

- `maximum-scale`：限制最大放大比例
- `minimum-scale`：限制最小缩小比例
- `user-scalable`：是否允许用户放大\缩小页面，默认为`yes`

## rem适配方案

> 先说原理，通过`meta`修正`1px`对应的物理像素数量，在根据统一的设计稿来生成`html`上的动态`font-size`，根据`dpr`构造字体等误差较大的样式的`mixin`

```
// 第一版:
function initRem() {
  const meta = document.querySelector('meta[name="viewport"]');;
  const html = document.documentElement;
  const cliW = html.clientWidth;
  const dpr = window.devicePixelRatio || 1;
  meta.setAttribute('name', 'viewport');
  meta.setAttribute(
      'content',
      `width=${cliW * dpr}, initial-scale=${1 /
          dpr} ,maximum-scale=${1 / dpr}, minimum-scale=${1 /
          dpr},user-scalable=no`
  );
  html.setAttribute('data-dpr', dpr);
  // 这样计算的好处是，你可以直接用ui的px/100得到的就是rem大小，方便快捷，无需mixin
  html.style.fontSize = 10 / 75 * cliW * dpr + 'px';
}
initRem();
window.onresize = window.onorientationchange = initRem();
```

> 对于引入的第三方ui组件，需要使用px2rem转换工具去做整体转换，比如`postcss-pxtorem`：https://github.com/cuth/postcss-pxtorem



