# postcss-design-px-to-rpx
[![NPM version](https://badge.fury.io/js/postcss-design-px-to-rpx.svg)](http://badge.fury.io/js/postcss-design-px-to-rpx)

[English](README.md) | [中文](README_CN.md)

将设计图px尺寸转换为uni-app rpx单位 的 [PostCSS](https://github.com/postcss/postcss) 插件. 不支持内联样式！

### 输入

```css
.logo {
  height: 200DP;
  width: 200DP;
  margin: 200DP auto 50px auto;
  font-size: calc(16DP * var(--convert-rate));
}
```

### 输出
app-plus
```css
.logo{
  height:110.4rpx;
  width:110.4rpx;
  margin:110.4rpx auto 50px auto;
  font-size:calc(8.832rpx * var(--convert-rate))
}
```
mp-weixin: 
```css
.logo{
  height:110.4rpx;
  width:110.4rpx;
  margin:110.4rpx auto 50px auto;
  font-size:calc(8.832rpx * var(--convert-rate))
}
```
h5:
```css
.logo[data-v-70604de4]{
  height:%?110.4?%;width:%?110.4?%;
  margin:%?110.4?% auto 50px auto;
  font-size:calc(%?8.832?% * var(--convert-rate))
}
```

## 上手

### 安装
使用npm安装
```
$ npm install postcss-design-px-to-rpx --save-dev
```
或者使用yarn进行安装
```
$ yarn add -D postcss-design-px-to-rpx
```

### 配置参数

默认参数:
```js
{
  unitToConvert: 'DP',
  viewportWidth: 414,
  unitPrecision: 8,
  propList: ['*'],
  viewportUnit: 'rpx',
  fontViewportUnit: 'rpx',
  selectorBlackList: [],
  replace: true,
  exclude: undefined,
  include: undefined
}
```
- `unitToConvert` (String) 需要转换的单位，默认为"DP"
- `viewportWidth` (Number) 设计稿的视口宽度
- `unitPrecision` (Number) 单位转换后保留的精度
- `propList` (Array) 能转化为rpx的属性列表
  - 传入特定的CSS属性；
  - 可以传入通配符"*"去匹配所有属性，例如：['*']；
  - 在属性的前或后添加"*",可以匹配特定的属性. (例如['*position*'] 会匹配 background-position-y)
  - 在特定属性前加 "!"，将不转换该属性的单位 . 例如: ['*', '!letter-spacing']，将不转换letter-spacing
  - "!" 和 "*"可以组合使用， 例如: ['*', '!font*']，将不转换font-size以及font-weight等属性
- `viewportUnit` (String) 希望使用的视口单位
- `fontViewportUnit` (String) 字体使用的视口单位
- `selectorBlackList` (Array) 需要忽略的CSS选择器，不会转为视口单位，使用原有的px等单位。
    - 如果传入的值为字符串的话，只要选择器中含有传入值就会被匹配
        - 例如 `selectorBlackList` 为 `['body']` 的话， 那么 `.body-class` 就会被忽略
    - 如果传入的值为正则表达式的话，那么就会依据CSS选择器是否匹配该正则
        - 例如 `selectorBlackList` 为 `[/^body$/]` , 那么 `body` 会被忽略，而 `.body` 不会
- `exclude` (Array or Regexp) 忽略某些文件夹下的文件或特定文件，例如 'node_modules' 下的文件
    - 如果值是一个正则表达式，那么匹配这个正则的文件会被忽略
    - 如果传入的值是一个数组，那么数组里的值必须为正则
- `include` (Array or Regexp) 如果设置了`include`，那将只有匹配到的文件才会被转换，例如只转换 'src/mobile' 下的文件
    (`include: /\/src\/mobile\//`)
    - 如果值是一个正则表达式，将包含匹配的文件，否则将排除该文件
    - 如果传入的值是一个数组，那么数组里的值必须为正则

> `exclude`和`include`是可以一起设置的，将取两者规则的交集。

#### 使用PostCss配置文件时

在`postcss.config.js`添加如下配置
```js
const path = require('path')
module.exports = {
  parser: require('postcss-comment'),
  plugins: [
    // 必须在'postcss-dp-to-rpx‘之前
    require('postcss-dp-to-rpx')({
      unitToConvert: "DP",
      viewportWidth: 414,
      viewportUnit: "rpx"
    }),
    require('@dcloudio/vue-cli-plugin-uni/packages/postcss'),
  ]
}
```
