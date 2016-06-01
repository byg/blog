---
title: 解决移动端滚动穿透问题
date: 2016-06-01 21:08:26
tags:
- javascript
- js
- mobile
- 滚动穿透
---

## 问题
众所周知，移动端当有 fixed 遮罩背景和弹出层时，在屏幕上滑动能够滑动背景下面的内容，这就是著名的滚动穿透问题

之前搜索了一圈，找到下面两种方案
<!-- more -->
## css 之 `overflow: hidden`
```scss
.modal-open {
  &, body {
    overflow: hidden;
    height: 100%;
  }
}
```
页面弹出层上将 `.modal-open` 添加到 html 上，禁用 html 和 body 的滚动条
但是这个方案有两个缺点：
 * 由于 html 和 body的滚动条都被禁用，弹出层后页面的滚动位置会丢失，需要用 js 来还原
 * 页面的背景还是能够有滚的动的效果

## js 之 touchmove + preventDefault
```js
modal.addEventListener('touchmove', function(e) {
  e.preventDefault();
}, false);
```
这样用 js 阻止滚动后看起来效果不错了，但是也有一个缺点：
* 弹出层里不能有其它需要滚动的内容（如大段文字需要固定高度，显示滚动条也会被阻止）

上面两个方案都有缺点，今天用英文关键字 google 了一下，才发现原来还有更好的方案
## 解决方案 `position: fixed`
```css
body.modal-open {
    position: fixed;
    width: 100%;
}
```
如果只是上面的 css，滚动条的位置同样会丢失
所以如果需要保持滚动条的位置需要用 js 保存滚动条位置关闭的时候还原滚动位置

[完整的示例](/demo/modal-scroll.html)

## 参考
* https://github.com/twbs/bootstrap/issues/15852
* https://github.com/mathiasbynens/document.scrollingElement
* https://segmentfault.com/q/1010000002942948