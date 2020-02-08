# pvfUtility 2020 HTTP 接口文档

pvfUtility 2020 一项重大的更新就是提供了HTTP接口进行自动化操作

这使得第三方工具可以利用http rest api进行文件读写

只需在软件内打开pvf,就能通过第三方工具进行下一步的操作

无需解压文件->再导入的繁琐操作!

软件目前(2020.1.2)提供了以下接口,如果需要更多的接口开放,欢迎联系QQ 1300271842

[可以加群讨论接口开发的事项](https://jq.qq.com/?_wv=1027&k=5gSQ5ks)

## 获取文件列表

```
http://127.0.0.1:[端口号]/list?path=[目录名称]
```

方法:GET

解释:

- 端口号:默认27000,**如果打开多个软件,那么端口号会变化,例如第二个就是27001**
- 目录名称:要获取的目录

返回:成功返回200状态码,内容为目录下的文件名,以换行符分割

示例代码(Java)

```java
HttpResponse<String> resp = request(HttpRequest.newBuilder().GET()
        .uri(URI.create("http://127.0.0.1:27000/list?path=worldmap/")).build());
if(resp.statusCode() == 200) {
    System.out.println(resp.body());
}
```

## 操作文件

```
http://127.0.0.1:[端口号]/file?name=[文件名称]
```

进行文件操作,注意HTTP方法不同会导致操作类型不同

解释:

- 端口号:默认27000,**如果打开多个软件,那么端口号会变化,例如第二个就是27001**
- 文件名称:要操作的文件名称

### 获取文件内容 

方法:GET

获取文件内容,返回200状态码表示成功获取文件内容(内容在Body中),404状态码表示文件不存在

示例代码(Java)

```java
HttpResponse<String> resp = request(HttpRequest.newBuilder().GET().uri(URI.create("http://127.0.0.1:27000/file?name=worldmap/worldmap.lst")).build());
if(resp.statusCode() == 200) {
    System.out.println(resp.body()); //读出文件内容
}
else if(resp.statusCode() == 404){
    System.out.println("文件不存在.");
}
```

### 上传新的文件内容

方法:POST

上传新的文件内容,返回200状态码表示成功覆写文件(或新增文件)

**注意需要右击-刷新一下才能看到新的文件夹!**

示例代码(Java)

```java
String newData = "test";
HttpResponse<String> resp = request(HttpRequest.newBuilder().POST(HttpRequest.BodyPublishers.ofString(newData))
        .uri(URI.create("http://127.0.0.1:27000/file?name=asd/new.lst")).build());
if(resp.statusCode() == 200) {
    System.out.println("成功");
}
```

### 删除文件

方法:DELETE

删除文件,返回200状态码表示成功,404表示文件不存在

实例代码(Java)

```java
HttpResponse<String> resp = request(HttpRequest.newBuilder().DELETE().uri(URI.create("http://127.0.0.1:27000/file?name=worldmap/worldmap.lst")).build());
if(resp.statusCode() == 200) {
    System.out.println("成功.");
}
else if(resp.statusCode() == 404){
    System.out.println("文件不存在.");
}
```