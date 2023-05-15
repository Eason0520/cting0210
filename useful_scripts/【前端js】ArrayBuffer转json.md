# 场景

- 服务器`json`数据被转为字节流，然后通过传输协议返回给`js`
- `js`将其转换为`json`数据

方法一：

```
ws.onmessage = function (evt) {
	// evt.data是ArrayBuffer
    // 将其转换为uint8字节流
    var uint8_msg = new Uint8Array(evt.data);
    // 解码成字符串
    var decodedString = String.fromCharCode.apply(null, uint8_msg);
    console.log(decodedString); 
    // parse,转成json数据
    var data = JSON.parse(decodedString);
	console.log(data);
};
```



方法二：

```
// 使用TextDecoder
var enc = new TextDecoder("utf-8");
var uint8_msg = new Uint8Array(evt.data);
var decodedString = enc.decode(uint8_msg);
// 再转换为json
var data = JSON.parse(decodedString);
console.log(data);
```



方法三（文件下载）：

```
var blob = new Blob([data], {type: "application/vnd.ms-excel"});
var objectUrl = URL.createObjectURL(blob);
var aForExcel = $("<a><span class='forExcel'>下载excel</span></a>").attr("href", objectUrl);
aForExcel.attr("download", "table.xlsx");
$("body").append(aForExcel);
$(".forExcel").click();
aForExcel.remove();
```

