hash 模式，router.replace 只会影响锚点后的内容。  
window.location.search 获取查询字符串，url 的结构是`${protocol}://${hostname}${port}${pathname}${search}${hash}`

# 问题

##### 跳转的路由是当前路由，不会触发生命周期函数

如果需要重新获取数据，光是放在生命周期函数里是不行的。
