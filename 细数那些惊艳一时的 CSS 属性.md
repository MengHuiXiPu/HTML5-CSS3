# 细数那些惊艳一时的 CSS 属性

新的 CSS 属性不断加入提案，本文列举 7 个不常用但很有用且你见过也可能没见过的 CSS 属性，带大家领略 CSS 之美。

## 1. position: sticky

![图片](https://mmbiz.qpic.cn/mmbiz_gif/zPh0erYjkib0Ad1QlrWE3Lz0J8IIicrKlQJRt6cZwrffUuls0x5jSBXLluG0GgtcexicUlNGzBbrJRCTxsoMYDK0A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

不知道大家平时业务开发中有没有碰到像上图一样的吸顶的需求：标题在滚动的时候，会一直贴着最顶上。

这种场景实际上很多：比如表格的标题栏、网站的导航栏、手机通讯录的人名首字母标题等等。如果让大家自己动手做的话，是不是会用 js 结合 css 来实现呢？以前确实是这样的，直到后来 `position` 属性新增了一个属性值 `sticky` ，前端程序员才迎来了小春天。

```
// css 部分
.container {
    background-color: oldlace;
    height: 200px;
    width: 140px;
    overflow: auto;
  }
  .container div {
    height: 20px;
    background-color: aqua;
    border: 1px solid;
  }
  .container .header {
    position: sticky;
    top: 0;
    background-color: rgb(187, 153, 153);
}

// html 部分
<div class="container">
  <div class="header">Header</div>
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

就这么简单？对，就这么简单，但是小伙伴们使用的时候要注意兼容性。

## 2. :empty 选择器

![图片](https://mmbiz.qpic.cn/mmbiz/zPh0erYjkib0Ad1QlrWE3Lz0J8IIicrKlQpu5du8GVCcyvdDGHjogYss6n4A3OdNUI6H3s7mUrvXX7AQgmYgia9Qw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1)

平时开发的时候数据都是通过请求接口获取的，也会存在接口没有数据的情况。这个时候正常的做法是给用户一个提示，让用户知道当前不是出 bug 了，而是确实没有数据。

一般的做法是我们人为的判断当前数据返回列表的长度是否为 0，如果为 0 则显示一个 “暂无数据” 给用户，反之则隐藏该提示。写过 Vue 的小伙伴是不是经常这么做：

```
<div>
    <template v-if="datas.length">
        <div v-for="data in datas"></div>
    </template>
    <template v-else>
        <div>暂无数据</div>
    </template>
</div>
```

但是有了 `:empty` 这个选择器后，你大可以把这个活交给 CSS 来干。

```
.container {
    height: 400px;
    width: 600px;
    background-color: antiquewhite;
    display: flex;
    justify-content: center;
    align-items: center;
}
.container:empty::after {
    content: "暂无数据";
}
```

通过 `:empty` 选中内容为空的容器，然后通过伪元素为空容器添加提示。是不是方便很多呢？

## 3. gap

小伙伴们日常开发中，都有用过 `padding` 和 `margin` 吧，`margin` 一般用做边距，让两个元素隔开一点距离，但是对于一些场景下，我们很难通过计算得到一个除的尽的值，比如 100px 我要让 3 个元素等分，且每个元素隔开 10px，这就很尴尬了。

没关系！我们可以用 `gap` 属性，`gap` 属性它适用于 Grid 布局、Flex 布局以及多列布局，并不一定只是 Grid 布局中可以使用。

比如我们要让每个元素之间隔开 20px， 那么使用 `gap` 我们可以这样：

```
display: flex | grid；
gap: 20px;
```

是不是很简单呢？

## 4. background-clip: text

这个属性可能小伙伴们用的不多，有什么用呢？简单来说就是可以做一个带背景的文字效果：

![图片](https://mmbiz.qpic.cn/mmbiz/zPh0erYjkib0Ad1QlrWE3Lz0J8IIicrKlQlKC1f6EHibq105t5sM98t6ibAXeyGN3iadqFH4av2dicZZWAqpaSptFViaQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz/zPh0erYjkib0Ad1QlrWE3Lz0J8IIicrKlQMFFECH6MchndwxuwS8uNCibZl1ImDwUF6iccqQO4Ms1fYiblPOoNcgVWA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz/zPh0erYjkib0Ad1QlrWE3Lz0J8IIicrKlQW6SicjTiag01YVWAGg5m5EX8nWByDIBJ1SfP4rwG40MicgbcHTYYpq8xA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1)

图 1 + 图 2 = 图 3。是不是很惊艳呢？大家平时 `background-clip` 是不是都用来做一些裁切效果？in知道它还有个属性值是 `text` 吗？`background-clip: text` 用来做带背景的文字效果，相信大家平时浏览一些网站的时候都会看到类似的实现，实际上通过 CSS 我们也能做到这种效果，可不要傻傻的以为都是用制图工具做的。

## 5. user-select

很多人不知道这个属性是什么意思。大家看看下图：

![图片](https://mmbiz.qpic.cn/mmbiz_gif/zPh0erYjkib0Ad1QlrWE3Lz0J8IIicrKlQP3KTf62XPumHictGRj8o8j01nOl8ibGNibn1WGZEAwIWKbN3moPB0kTyQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

网页和 APP 有个不同点是，网页上的字是可以通过光标选中的，而 APP 的不行。

有的小伙伴可能会疑惑：那我网页上也用不着这个属性啊？

非也非也，我们知道现在很多新的技术产生，可以在 APP 上嵌套 **webview** 或者是**网页**，比如 **Electron** 做的 **桌面端应用** ，大家没见过哪个桌面端应用是可以光标选中的吧？

而 `user-select` 属性可以 **禁用光标选中** ，让网页看着和移动端一样。

## 6. :invalid 伪类

**`:invalid`** 表示任意内容未通过验证的 `input` 或其他 `form` 元素。什么意思呢？举个例子。

这是一个表单。

```
<form>
  <label for="url_input">Enter a URL:</label>
  <input type="url" id="url_input" />
  <br />
  <br />
  <label for="email_input">Enter an email address:</label>
  <input type="email" id="email_input" required/>
</form>
```

我们的需求是让 `input` 当值有效时，元素颜色为绿色，无效时为红色。

```
input:invalid {
  background-color: #ffdddd;
}

form:invalid {
  border: 5px solid #ffdddd;
}

input:valid {
  background-color: #ddffdd;
}

form:valid {
  border: 5px solid #ddffdd;
}

input:required {
  border-color: #800000;
  border-width: 3px;
}

input:required:invalid {
  border-color: #C00000;
}
```

有了 `:invalid` 属性后，我们就可以不用 JS 也能实现校验提示的效果了。

## 7. :focus-within 伪类

**`:focus-within`** 表示一个元素获得焦点，或该元素的后代元素获得焦点，就会匹配上。啥意思呢？

话不多说，先看图：

![图片](https://mmbiz.qpic.cn/mmbiz_gif/zPh0erYjkib0Ad1QlrWE3Lz0J8IIicrKlQ9gLqFOCO9ETGsFROsUygWxI5lH54JAS198Nvjqk3xHgbFudHB4Sdqg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

```
// CSS
form {
    border: 1px solid;
    width: 400px;
    height: 300px;
    display: flex;
    justify-content: center;
    align-items: center;
}
form:focus-within {
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.3);
    background-color: beige;
}

// HTML
<form>
  <input type="text" id="ipt" placeholder="请输入..." />
</form>
```

可以根据子元素的状态来改变父元素的样式，方便的很。也能玩出不少花样来。

有的小伙伴看完后可能会想：嗷，也没啥的啊，这有啥花样？

## 8. mix-blend-mode:difference

`mix-blend-mode:difference` 属性描述了元素的内容应该与元素的直系父元素的内容和元素的背景如何混合。其中，`difference` 表示差值。

![图片](https://mmbiz.qpic.cn/mmbiz/zPh0erYjkib0Ad1QlrWE3Lz0J8IIicrKlQ4JecYPstqNI79GNG1JYgmKxYzmvxWSO4DXrAxPhZ17tFh6GdYUvVBA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1)

如果给你这张图，你会怎么去绘制？是不是很麻烦。

但是如果用 `mix-blend-mode:difference` 这个属性，就可以很容易的实现。

```
.mode {
    display: flex;
    justify-content: center;
    align-items: center;
    mix-blend-mode:difference;
}
  .dark {
    position: relative;
    left: 6px;
    height: 24px;
    width: 24px;
    background-color: grey;
    border-radius: 50%;
}
.light {
    mix-blend-mode:difference;
    position: relative;
    left: -6px;
    height: 16px;
    width: 16px;
    border-radius: 50%;
    border: 4px solid grey;
}
```

