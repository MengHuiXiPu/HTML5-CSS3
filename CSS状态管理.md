# CSS状态管理

CSS用于交互的方式无非就那么几种：

- 伪类：`:hover`、`:link`、`:active` ...
- 动画：`animation`
- 过渡动画：`transition`

这些交互方式组合起来，真的可以玩出一些花样，例如我们本文的主题，CSS的状态管理，一起来看个例子🌰

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS状态管理</title>
    <style>
        @keyframes statement {
            0% {
                --state: initial;
            }

            1%, 100% {
                --state: ;
            }
        }

        .zero2one {
            width: 100px;
            height: 100px;
            border: 1px solid black;
            --hover: var(--state) green;
            background: var(--hover, red);
            animation: statement 1ms linear 1 forwards paused;
        }

        .zero2one:hover {
            animation-play-state: running;
        }
    </style>
</head>
<body>
    <div class="zero2one">零一</div>
</body>
</html>
```

看一下具体的效果：

![图片](https://mmbiz.qpic.cn/mmbiz_gif/lgHVurTfTcx8mpPdQWGqNMYgonXaH8QJkamkpnI8xOHxVIcGOhS1Z0V9t3grXQ1QkJ31zbUgnPmx7sxialwtmXA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)小试牛刀

正方形本来是红色背景的，当我们鼠标移入后，背景颜色变为绿色，且不会再变回去。如果是普通的交互，我们应该只用到了 `:hover`，鼠标移出后，元素会恢复原来的颜色，而我们现在是如何做到的呢？

这是因为我们把 "鼠标移入方框" 这个动作存储下来了！这就是 "CSS状态管理" 我们一起来解读这段代码吧！

## CSS变量

来看一个CSS变量的例子

```
html {
  --state1: initial;
  --state2: ;
}

.zero2one {
  --color1: var(--state1) red; 
  --color2: var(--state2) blue;  
  
  color: var(--color1, yellow);   /* 最终字体颜色为yellow */
  background: var(--color2, pink); /* 最终背景色为blue */
}
```

其实这就是借助了 `var()` 函数第一个值无效时会用第二、第三个值、第n个值作为备选值的特性（如上述代码所示）

然后还有一个骚操作就是 `color: var(--color1, yellow)` 最终会展示黄色，因为变量 `--color1` 引用了另一个变量 `--state1: initial` ，正是因为值为 `initial` ，导致最终 `color` 展示了黄色，`--color1` 被认定为一个无效值

这时有人要说了，那我直接设置 `--color1: initial red;` 不就好了，为啥还要多引一个变量呢？我试过了，这样直接写是没用的，别问，问就是我也不知道！（有知道的小伙伴可以评论区告诉我~）

然后变量 `--color2` 引用了变量 `--state2: ;`，因为其值为空，所以其实变量 `--color2` 对应的也就是 `blue` ，那么 `var(--color2, pink)` 自然也是展示蓝色了

## 变量切换

借助刚刚了解的CSS变量的特性，我们可以让某个变量切换其值即可实现CSS的状态切换，如何不借助 JS 实现对CSS变量的切换呢？这时候就要借助我们文章开头提到的 `animation` 了

先定义一个 `keyframes`

```
@keyframes statement {
  0% {
    --state: initial;
  }

  1%, 100% {
    --state: ;
  }
}
```

在初始状态时将变量 `--state` 的值定义为 `initial`，非初始状态将变量 `--state` 的值定义为空

好像还是没有讲到如何切换。此时可以借助一个CSS属性 `animation-play-state` ，其控制了元素动画的运动状态，假设我们一开始给某个元素设置的运动状态为 `paused`

```
.zero2one {
  animation: statement 1ms linear 1 forwards paused;
}
```

一开始该元素就是暂停状态，所以根据我们定义的 `keyframes` 的初始状态来看，此时全局有一个变量 `--state`，值为 `initial`

然后可以在用户进行某些操作（`:hover`、`:active`等等）后，将该元素运动状态改为 `running`

```
.zero2one:hover {
  animation-play-state: running;
}
```

当将元素的动画状态设为 `running` 后，其动画已经不是初始状态了，并且因为我们设置了 `forwards` ，所以此时全局有一个变量 `--state`，其值为空

这样就做到了变量的动态切换

将上述两个技巧组合在一起，就实现了简易版的"CSS状态管理"

## 实战应用

由此还引申出了一个比较有意思的东西，那就是CSS实现画板！相信你们原理都懂了，那就直接放代码吧~

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS状态管理</title>
    <style>
        @keyframes statement {
            0% {
                --state: initial;
            }

            1%, 100% {
                --state: ;
            }
        }

        .zero2one {
            background: rgba(222 222 255 / 0.125);
            border: 1px solid #eee;
            display: inline-block;
        }

        li {
            list-style: none;
            display: inline-block;
            margin: 0;
            padding: 0;
            width: 3px;
            height: 3px;
            float: left;
            --bg-color: var(--state) green;
            background: var(--bg-color, transparent);
            animation: statement 1ms linear 1 forwards paused;
        }

        li:hover {
            animation-play-state: running;
        }
    </style>
</head>
<body>
    <ul class="zero2one">
      <li></li>
      <li></li>
      <!-- 此处省略 10000个li标签 -->
    </ul>
</body>
</html>
```

最终实现效果就是这样的，如下图所示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)CSS画板