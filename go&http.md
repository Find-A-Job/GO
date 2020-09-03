### 搭建http服务器
---

#### net/http
```
http.HandlerFunc
(w http.ResponseWriter, req *http.Request)
```
* 先执行 **req.ParseForm()** 才能使用各类form
* **req.Form**对应get,以下简称rf

|值|说明|类型|
|----|----|----|
|`rf.Get("key")`|key对应的值|``|
|``||``|
|``||``|
|``||``|
* **req.PostForm**对应post,以下简称rp

|值|说明|类型|
|----|----|----|
|`rp.Get("key")`|key对应的值|``|
|``||``|
|``||``|
|``||``|
* **req.MultipartForm**对应upload,以下简称rm

|值|说明|类型|
|----|----|----|
|`rm.Value`|额外参数|`map[string][]string`|
|`rm.File`|附带的文件|`map[string][]*FileHeader`|
|`rm.File["key"]`|key对应的文件|`[]*FileHeader`|
|`rm.File["key"][0]`|第一个文件|`*FileHeader`|
|`rm.File["key"][0].Filename `|文件名|`string`|
|`rm.File["key"][0].Header`|请求头|`textproto.MIMEHeader`|
|`rm.File["key"][0].Size`|文件大小|`int64`|
|`rm.File["key"][0].open()`|打开文件|`function`|
|``||``|
