---
title: css
---

```css
//清除浮动
xxx:after {
  content: '.';
  clear: both;
  width: 0;
  height: 0;
  visibility: hidden;
  overflow: hidden;
  display: block;
}
```

### 外边距合并

https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing

### 垂直居中

top: 50%;
transform: translateY(-50%);

### inline 类型的 flex 容器

    display: inline-flex;
