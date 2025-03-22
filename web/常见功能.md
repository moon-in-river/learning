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

# 上传文件

- post 请求
- 可以用 URLSearchParams 序列化中文文件名，特殊符号能处理吗，比如 [、/~！等等
- 需要支持的文件格式，选择时要校验、提交时也要校验
- 同时传多个文件，最多传文件数限制
- swagger 上显示 Paramters，普通 json 参数和 FormData 格式的 file 一起放，只需要设置 ContentType 为 FormData，文件实体放在 data，其他参数放在 params
- 防止用户重复上传，上传成功后关闭弹窗、或者清空已选中文件（需要清空 input 元素的 files 列表才行，对应 eps 的 el-input，需要用 ref 获取元素，再调用 clearFiles api。在 on-success 里调用没用，在 upload success 回调里才被触发）
