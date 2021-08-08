## 部分 HTML 属性

## 1.Download

`download` 属性指定了当用户单击超链接时将下载目标(在 `href` 属性中指定的文件)。

```
<a href="/images/program.jpg" download>
```

## 2.ContentEditable

在HTML中，任何元素都可以编辑。使用一些JavaScript事件处理程序，你可以把你的网页转换成一个完整而快速的富文本编辑器。当一个HTML元素的 `contenteditable` 设置为 `true` 时，`document.execCommand()` 方法就可用了。这让你可以运行命令来操作可编辑区域的内容。

```
<div contenteditable="true">
  This text can be edited by the user.
</div>
```

## 3.Multiple

`multiple` 属性是一个布尔属性。当存在时，它指定用户可以一次选择多个选项。

```
<label for="tech">Choose a technology:</label>

<select name="tech" id="tech" multiple>
  <option value="angular">Angular</option>
  <option value="react">React</option>
  <option value="vue">vue</option>
</select>

<!-- or -->
<input type="file" multiple>
```

## 4.Poster

`poster` 属性指定了在视频下载时或用户点击播放按钮前显示的图像。如果不包括这个，将使用视频的第一帧来代替。

```
<video  poster="/gif/poster.gif">
  <source src="trailer.mp4" type="video/mp4">
</video>
```

## 5.Hidden

`hidden` 属性是一个布尔属性。如果存在，浏览器将不会显示指定了 `hidden` 属性的元素。

```
<p hidden>You won't see me on the browser, only developers can!</p>
```

## 6.Accept

`accept` 属性指定用户可以从文件输入对话框中选择的文件类型的过滤器。`accept` 属性只能与 `<input type="file">` 一起使用。

```
<form action="/...">
  <label for="img">Select image:</label>
  <input type="file" id="img" name="img" accept="image/*">
  <input type="submit">
</form>

<!-- or -->

<input type="file" accept=".jpg, .png" >
```

