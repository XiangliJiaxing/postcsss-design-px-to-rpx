# postcss-design-px-to-rpx
[![NPM version](https://badge.fury.io/js/postcss-design-px-to-rpx.svg)](http://badge.fury.io/js/postcss-design-px-to-rpx)

[English](README.md) | [中文](README_CN.md) 

A plugin for [PostCSS](https://github.com/postcss/postcss) that generates uniapp units (rpx) from pixel units.

### Input
```css
.logo {
  height: 200DP;
  width: 200DP;
  margin: 200DP auto 50px auto;
  font-size: calc(16DP * var(--convert-rate));
}
```

### Output
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

## Getting Started

### Installation
Add via npm
```
$ npm install postcss-design-px-to-rpx --save-dev
```
or yarn
```
$ yarn add -D postcss-design-px-to-rpx
```

### Usage

Default Options:
```js
{
  unitToConvert: 'DP',
  viewportWidth: 414,
  unitPrecision: 5,
  propList: ['*'],
  viewportUnit: 'rpx',
  fontViewportUnit: 'rpx',
  selectorBlackList: [],
  exclude: undefined,
  include: undefined
}
```
- `unitToConvert` (String) unit to convert, by default, it is DP.
- `viewportWidth` (Number) The width of the viewport.
- `unitPrecision` (Number) The decimal numbers to allow the vw units to grow to.
- `propList` (Array) The properties that can change from px to vw.
  - Values need to be exact matches.
  - Use wildcard * to enable all properties. Example: ['*']
  - Use * at the start or end of a word. (['*position*'] will match background-position-y)
  - Use ! to not match a property. Example: ['*', '!letter-spacing']
  - Combine the "not" prefix with the other prefixes. Example: ['*', '!font*']
- `viewportUnit` (String) Expected units.
- `fontViewportUnit` (String) Expected units for font.
- `selectorBlackList` (Array) The selectors to ignore and leave as px.
    - If value is string, it checks to see if selector contains the string.
        - `['body']` will match `.body-class`
    - If value is regexp, it checks to see if the selector matches the regexp.
        - `[/^body$/]` will match `body` but not `.body`
- `exclude` (Regexp or Array of Regexp) Ignore some files like 'node_modules'
    - If value is regexp, will ignore the matches files.
    - If value is array, the elements of the array are regexp.
- `include` (Regexp or Array of Regexp) If `include` is set, only matching files will be converted,
    for example, only files under `src/mobile/` (`include: /\/src\/mobile\//`)
    - If the value is regexp, the matching file will be included, otherwise it will be excluded.
    - If value is array, the elements of the array are regexp.

> `exclude` and `include` can be set together, and the intersection of the two rules will be taken.

#### Use with PostCss configuration file

add to your `postcss.config.js`
```js
const path = require('path')
module.exports = {
  parser: require('postcss-comment'),
  plugins: [
    // must be used before postcss-dp-to-rpx
    require('postcss-dp-to-rpx')({
      unitToConvert: "DP",
      viewportWidth: 414,
      viewportUnit: "rpx"
    }),
    require('@dcloudio/vue-cli-plugin-uni/packages/postcss'),
  ]
}
```
