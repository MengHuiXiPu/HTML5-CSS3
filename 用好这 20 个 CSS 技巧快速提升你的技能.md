# 用好这 20 个 CSS 技巧快速提升你的技能

- flexbox 内容换行
- 间距
- 长文本内容
- 防止图像被拉伸或压缩
- 锁定滚动链接
- CSS 变量回退
- 使用固定宽度或高度
- 被忽略的 background-repeat
- 垂直方向的媒体查询
- 使用 justify-content: space-between
- 图片上的文字
- 使用 CSS 网格中的固定值需注意
- 仅在需要时显示滚动条
- 滚动条装订线
- CSS flexbox 中的最小内容大小
- CSS 网格中的最小内容大小
- 自动适合与自动填充
- 图片最大宽度
- 位置：粘性css网格
- 分组选择器

## 内容

### flexbox 内容换行

当我们使用 flexbox 布局的时候，默认情况下，在容器宽度不够时，可能就会出现这样的情况。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个主要是因为 `flex-wrap` 的默认值是 `nowrap`，所以我们需要这样改变一下值。

```
.options-list {
  display: flex;
  flex-wrap: wrap;
}
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 间距

设计师在提供的设计稿中，很容易忽略文本在极限情况下与其他元素之间的间距，最终可能会出现这样的情况，文字与 icon 紧挨。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

要处理这个情况，就是需要考虑是在文本还是 icon 部分，根据实际情况分析后选择添加 `margin` 值来增加间距。

```
.section__title {
  margin-right: 1rem;
}
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

> 不过这个情况，我一般会考虑在 icon 上加 `margin-left`，这样的话，在没有 icon 的时候，文字还是可以顶到容器边缘。

### 长文本内容

这个就简单，就是文本过长的处理方式，一般情况下就是换行或者截断并已 ... 的形式出现。至于选择什么方式就要看当前页面模块的设计风格来决定了。

文中的处理方式是采用了截断出 ... 这种。

```
.username {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 防止图像被拉伸或压缩

这种情况一般是在是图片由用户上传，或者图片未按规定比例使用导致的。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

处理方式就是直接使用 `object-fit` 属性：

```
.card__thumb {
  object-fit: cover;
}
```

### 锁定滚动链接

主要出现的情况是在当页面中点击弹出 modal 弹层时，页面内容本身很长的时候。

```
.modal__content {
  overscroll-behavior-y: contain;
  overflow-y: auto;
}
```

`overscroll-behavior` 虽好，但目前存在很大的兼容性问题。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### CSS 变量回退

这个 CSS 变量回退，说白了，其实就是当 CSS 变量值无效时而使用一个“安全值”来保证某个属性值还是可用的。

```
.message__bubble {
  max-width: calc(100% - var(--actions-width, 70px));
}
```

不过结合小志在做猎头之前写前端页面时的处理方式，可能还会去考虑一下浏览器对 `var()` 的兼容性问题，那么就会再加上一个属性。不过现在的浏览器来看，好像普遍性不存在这个问题。

```
.message__bubble {
  max-width: 70px;
  max-width: var(--actions-width, 70px);
}
```

### 使用固定宽度或高度

这个没啥说的，主要就是对内容过长时，溢出容器时的一个保护措施。比如当我们把高度固定为 `350px` 之后，内容过长就会溢出。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而如果我们把 `height` 换成 `min-height` 的话，那情况就有所不同了。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同理，在宽度的处理上也是一样。

### 被忽略的 background-repeat

在使用背景图的时候，如果没加上 `no-repeat` 的话，默认就会把背景图平铺开。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

所以，随手加上 `no-repeat` 还是一个好习惯，除非你已知这张背景图是否要平铺。

```
.hero {
  background-image: url('..');
  background-repeat: no-repeat;
}
```

### 垂直方向的媒体查询

这个场景目前小志在页面中还未见过，一般在中后台页面出现的概率比较多，比如作者提供的页面效果。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

左下角是通过 `position: sticky;` 定位的，可能通过 `fixed` 定位也是一样吧，然后当浏览器的高度变小的时候，就会叠在左侧的导航上面。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

显然，这样的页面效果就不对了。而如果这个时候，通过 `@media` 方式判断页面高度，在某个安全区外我们才让左下角这部分通过 `sticky` 来定位。

```
@media (min-height: 600px) {
  .aside__secondary {
    position: sticky;
    bottom: 0;
  }
}
```

### 使用 justify-content: space-between

原文中这部分标题是 **Using Justify-Content: Space-Between**，但看起来更应该是 **Using gap**。使用什么标题不是关键，这部分主要是提到当使用 `justify-content: space-between;` 时，如果元素不够，元素与元素之间的间距就会拉长，因为要平均分布元素之间的间距。

比如原本是想要这样的一个效果，间距的值是固定的：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在元素数量足够的情况下，元素与元素之间的间距还是比较理想的，通过下面这个 CSS 处理方式：

```
.wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
```

但，元素的数量总是会不足的时候，那么就会这样了。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可见，其实并不是想要平均分布，而只是想要在最大的极限情况下和内容不足的时候，元素之间的间距是可控的。一般来说，这个处理方式挺多的，用 `margin` 之类的也是可以处理，不过目前有一个兼容性并不是十分好的 `gap` 能完美解决这个问题。

```
.wrapper {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 图片上的文字

用户体验上的一个细节处理问题，如果在图片上有文案，但图片没加载出来，或者加载失败的时候，添加一个背景色，以保证图片加载失败的情况下，文字与背景能区分开。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 使用 CSS 网格中的固定值需注意

`grid` 网格布局现在开始慢慢被关注了，`grid` 与 `flex` 一样都可以做自适应的效果，也可以使用固定值的情况。在使用固定值的时候，最好是通过 `@media` 判断一下宽度，以便于满足最小宽度时的处理。

```
@media (min-width: 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 250px 1fr;
    gap: 1rem;
  }
}
```

> 不过一般这种情况下，可能就直接改变页面布局了，感觉作者在这个方面上担忧是多余的。或者主要是自己未遇到过他所遇到的场景吧。

### 仅在需要时显示滚动条

这个没啥说的，一般大家都是把 `scroll` 设置为 `auto`。不过在个别情况下，可能这个 `auto` 会让元素的宽度变小，然后导致内容或者布局出现一点小瑕疵。

```
.element {
  overflow-y: auto;
}
```

### 滚动条装订线

这个所谓的装订线，主要就是 `scrollbar-gutter` 属性，保留滚动条的空间。在上面那个 `overflow` 例子中提到，如果设置为 `auto` 的时候，可能会导致页面布局出现非意料的情况，那么 `scrollbar-gutter` 就可以处理这个问题了。

> 兼容性还是一个不可避免的话题。

假设不考虑兼容性的问题，那么我们就可以使用这个方法保留滚动条的空间。

```
.element {
  scrollbar-gutter: stable;
}
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### CSS flexbox 中的最小内容大小

在使用 flex 布局的时候，很有可能其中某个 item 的文本内容很长，最终导致没有换行而溢出容器之外。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这种情况也并不是说没有可能，就算是使用 `overflow-wrap: break-word;` 也不会有效果，那么这个时候需要加上 `min-width: 0;` 来处理。

```
.card__title {
  overflow-wrap: break-word;
  min-width: 0;
}
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

> 对于这点，小志心中有一个疑惑，为什么要考虑使用 `overflow-wrap` 的方式，而不使用 `word-wrap` 或者 `word-break` 呢？
>
> 通过 demo 对比之后发现，效果上 `word-break: break-word;` 与上述的例子是相同的结果，仅仅只是从文字换行的 demo 效果来看。但从 MDN 中对这两个属性的介绍来看，还是有区别的。

- **word-break**

  > The word-break CSS property sets whether line breaks appear wherever the text would otherwise overflow its content box.
  > ![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- **overflow-wrap**

  > The overflow-wrap CSS property applies to inline elements, setting whether the browser should insert line breaks within an otherwise unbreakable string to prevent text from overflowing its line box.
  > ![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### CSS 网格中的最小内容大小

与 flexbox 类似，CSS 网格的子项有一个默认的最小内容大小，即 `auto`，这意味着，如果存在大于网格项的元素，它将溢出。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
@media (min-width: 1020px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 248px;
    grid-gap: 40px;
  }
}

.carousel {
  display: flex;
  overflow-x: auto;
}
```

> 对于网格这块，了解的不够深入，直接延用作者的意思就是将 `grid-template-columns` 的值改变一下，改为：`minmax(0, 1fr) 248px` 就可以得到下面这个效果。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 自动适合与自动填充

> 当时看到效果图的时候，我还以为是 flex 布局中的情况，结果是 grid 网格布局中使用的情况。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在使用 grid 布局时，如果是这样写，利用 `auto-fit` 的话，就会是上面这个效果，item 不够时会被拉长。

```
.wrapper {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  grid-gap: 1rem;
}
```

而如果改为 `auto-fill` 的话就是下面这个效果。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

对于 `auto-fit` 和 `auto-fill` 的区别可以看这张图。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 图片最大宽度

作者建议可以在 reset 部分中对 `img` 加上最大宽度的设置，但没说具体是为什么。个人猜想是考虑图片在容器中的展示吧，同时还有一个 `object-fit` 属性。😆

```
img {
  max-width: 100%;
  object-fit: cover;
}
```

### 位置：粘性css网格

在使用 CSS 网格布局时，如果子元素使用了 `position: sticky` 的话，由于网格子元素的默认对齐方式是 `stretch`，所以会被拉伸。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而其实我们想要的是，左侧边栏并非拉伸效果的，所以，需要通过 `align-self: start;` 改变一下子元素自身的对齐方式。

```
aside {
  align-self: start;
  position: sticky;
  top: 1rem;
}
```

这样的话，效果就不一样了。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

> 简单测试了一下，在 flex 布局中也是同样的情况，主要就是因为子元素的特性是拉伸的。

### 分组选择器

```
/* Don't do this, please */
input::-webkit-input-placeholder,
input:-moz-placeholder {
  color: #222;
}
```

现在写 CSS 的时候基本上都不会去写带前缀的属性，而是通过构建工具来自动完成。所以平时也不会在意这个。按照作者的说法是，如果把这两个写在一起，会导致整个规则失效，建议分开写。

```
input::-webkit-input-placeholder {
  color: #222;
}

input:-moz-placeholder {
  color: #222;
}
```