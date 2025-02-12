# 文字省略显示

关键在于 css 属性设置、容器空间设置

方式 1

```html
<!-- 设置 title 属性 hover 显示完整文字 -->
<span class="text-ellipsis" title="这是一段很长很长的文字，需要省略显示">
  这是一段很长很长的文字，需要省略显示
</span>
```

```css
.text-ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  flex: 1;
}
```
