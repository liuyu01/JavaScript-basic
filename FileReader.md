##### FileReader

HTML5 的 FileReader 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据。

其中File对象可以是来自用户在一个<input>元素上选择文件后返回的[`FileList`](https://developer.mozilla.org/zh-CN/docs/Web/API/FileList)对象,也可以来自拖放操作生成的 [`DataTransfer`](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer)对象,还可以是来自在一个[`HTMLCanvasElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement)上执行`mozGetAsFile()`方法后返回结果。

FileReader提供了如下方法：

| readAsArrayBuffer(file)   | 按字节读取文件内容，结果用ArrayBuffer对象表示 |
| ------------------------- | --------------------------------------------- |
| readAsBinaryString(file)  | 按字节读取文件内容，结果为文件的二进制串      |
| readAsDataURL(file)       | 读取文件内容，结果用data:url的字符串形式表示  |
| readAsText(file,encoding) | 按字符读取文件内容，结果用字符串形式表示      |
| abort()                   | 终止文件读取操作                              |
