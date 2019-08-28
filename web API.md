##### 常见web API

1. querySelector(元素向下查询，返回一个)：获取指定元素中匹配CSS选择器的元素
2. querySelectorAll(元素向下查询，返回多个)：获取指定元素中匹配CSS选择器的所有元素
3. closest(元素向上查询)：该元素可以向上查询，也就是可以查询到父元素
4. dataset：获取元素以“data-”为前缀的属性集合
5. URLSearchParams(查询参数) new URLSearchParams(location.search).get("name");
6. hidden：隐藏元素
7. contenteditable：使元素可以被编辑
8. spellCheck：检查拼写
9. classList：类名控制器 这是一个对象，该对象里封装了许多操作元素类名的方法 add() remove() toggle() replace() contains()
10. getBoundingClientRect(元素空间结构详细信息):可以获取指定元素在当前页面的空间信息
11. contains(判断是否包含指定元素)
12. online(网络状态)：监听当前的网络状态变动，然后执行对应的方法
13. battery(电池状态)：navigator.getBattery() 获取设备的电池状态
14. vibration(设备震动)：navigator.vibrate()使设备进行震动
15. page visibility(页面可见性)：visibilitychange 监听页面可见性变化的
16. deviceOrientation(陀螺仪)：也就是设备的方向，又名重力感应，该API在IOS设备上失效的解决办法，将域名协议改成https
17. toDataUrl(画布内容转base64)：这个canvas的API，作用是将画布的内容转换成一个base64的图片地址
18. customEvent(自定义事件)
19. notification(桌面通知)
20. fullScreen(全屏)：不仅仅可以作用在documentElement上，还可以作用在指定元素
21. orientation(屏幕方向)：可以监听用户手机设备的旋转方向变化
