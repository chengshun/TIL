# Use Ajax Download File In Broswer

## 请求octet-stream类型的数据
```javascript
axios.get(url, {
  responseType: 'arraybuffer' // 将服务器返回的数据作为二进制数据处理
})
.then(response => {
  // ArrayBuffer 本身就是一个黑盒，不能直接读写所存储的数据，需要借助以下TypedArray来读写, Uint8Array是TypedArray提供的9中类型之一
  const data = new Uint8Array(response.data); // ArrayBuffer => unit8Array ?
  // Do something with the data
})
.catch(error => {
  console.error(error)
})
```

## 浏览器端下载
1. 将数据转为blob, 再根据blob创建data url
2. 动态创建a标签，将href设置为data url，同时添加download, 点击触发文件下载

```javascript
    // 这里可以成为动态文件创建，其正在创建一个类似文件的对象
    const blob = new Blob([data], { type: 'application/pdf' }); // data type [Unit8Array]
    // 创建一个Blob URL
    const url = URL.createObjectURL(blob);
    // 创建一个a标签进行下载
    const link = document.createElement('a');
    link.href = url;
    link.download = 'file.pdf';
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    // 释放Blob URL资源
    URL.revokeObjectURL(url);
```