# 使用vue-cli
```bash
vue init webpack yourapp
```
# 安装所需的插件
```bash
$ npm i -S postcss-aspect-ratio-mini postcss-px-to-viewport postcss-write-svg postcss-cssnext postcss-viewport-units cssnano
```
# 修改根目录下的`.postcssrc.js`
```json
module.exports = {
  "plugins": {
    "postcss-import": {},
    "postcss-url": {},
    "postcss-aspect-ratio-mini": {},
    "postcss-write-svg": {
      utf8: false
    },
    "postcss-cssnext": {},
    "postcss-px-to-viewport": {
      viewportWidth: 750, // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
      viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
      unitPrecision: 3, // 指定px转换为视窗单位值的小数位数（很多时候无法整除）
      viewportUnit: 'vw', // 指定需要转换成的视窗单位，建议使用vw
      selectorBlackList: ['.ignore', '.hairlines'], // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
      minPixelValue: 1, // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
      mediaQuery: false // 允许在媒体查询中转换px
    },
    "postcss-viewport-units": {},
    "cssnano": {
      preset: "advanced",
      autoprefixer: false,
      "postcss-zindex": false // 千万要设为false
    }
  }
}
```
因为`cssnano`的`preset`为`advanced`，所有还要另外再安装一下：
```bash
$ npm i -S cssnano-preset-advanced
```
# `vw`的兼容处理

1. 在`index.html`引入`polyfill`文件：
```html
<!-- cdn引入 -->
<script src="//g.alicdn.com/fdilab/lib3rd/viewport-units-buggyfill/0.6.2/??viewport-units-buggyfill.hacks.min.js,viewport-units-buggyfill.min.js"></script>
<!-- 本地引入 -->
<script src="/static/viewport-polyfills.js"></script>
```
2. 在`index.html`中调用`viewport-units-buggyfill`：
```html
<script>
  window.onload = function () {
    window.viewportUnitsBuggyfill.init({
      hacks: window.viewportUnitsBuggyfillHacks
    });
  }
</script>
```
3. 为了防止部分浏览器`img`标签因为`content`不显示，全局添加：
```css
img {
  content: normal !important;
}
```
# 一些样式要点

当不想要把`px`转为`vw`时，要在对应的`html`上添加指定的类名`.ignore`或`hairlines`（一般用于设置`border-width: 0.5px`）
```html
<!-- html -->
<div class="test ignore"></div>
```
```css
/* css */
.ignore {
    margin: 10px;
    background-color: red;
}

.box {
    width: 180px;
    height: 300px;
}

.hairlines {
    border-bottom: 0.5px solid red;
}
```

可以使用`vw`的地方：
- 容器适配，可以使用vw
- 文本的适配，可以使用vw
- 大于1px的边框、圆角、阴影都可以使用vw
- 内距和外距，可以使用vw

设置固定长宽比：

```html
<!-- html -->
<div aspectratio ratio-16-9>
    <img aspectratio-content src="./assets/logo.png">
  </div>
```
```css
/* css */
[aspectratio] {
    position: relative;
  }

[aspectratio]::before {
  content: '';
  display: block;
  width: 1px;
  margin-left: -1px;
  height: 0;
}

[aspectratio-content] {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
}

[ratio-1-1] {
  aspect-ratio: '1:1';
}

[ratio-16-9] {
  aspect-ratio: '16:9';
}

[ratio-4-3] {
  aspect-ratio: '4:3';
}
```

移动端1px解决方案（其中一种）：

```html
<!-- html -->
<div class="one1px">
  test
</div>
```
```css
/* css */
.one1px {
  position: relative;
  width: 20vw;
  height: 10vw;
  background: #fff;
}

.one1px:after {
  content: '';
  position: absolute;
  left: 0;
  background: #ddd;
  width: 1px;
  height: 100%;
  -webkit-transform: scaleY(0.5);
  transform: scaleY(0.5);
  -webkit-transform-origin: 0 0;
  transform-origin: 0 0;

}
```
