### 搭建http服务器
---

#### net/http
```
http.HandlerFunc
(w http.ResponseWriter, req *http.Request)
```
* 先执行 **req.ParseForm()** 才能使用form
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
> 先执行req.ParseMultipartForm()

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

* 在线图片
```
// 关键的一步，目录后要带上"/"
http.HandleFunc("/image/", imageServer)
// 访问图片，将图片转为byte写入响应体内
func imageServer(w http.ResponseWriter, req *http.Request) {
	fmt.Printf("imageServer:%v, %v\n", req.URL, req.URL.Path)
  //根据自身的目录调整path
	file, err := os.Open(".." + req.URL.Path)
	if err != nil {
		w.Write([]byte(err.Error()))
		return
	}

	defer file.Close()
	buff, err := ioutil.ReadAll(file)
	if err != nil {
		w.Write([]byte(err.Error()))
		return
	}
	w.Write(buff)
}
```
* 带form的post
```
// 要求格式
{
  "touser": "OPENID",
  "template_id": "TEMPLATE_ID",
  "page": "index",
  "miniprogram_state":"developer",
  "lang":"zh_CN",
  "data": {
      "number01": {
          "value": "339208499"
      },
      "date01": {
          "value": "2015年01月05日"
      },
      "site01": {
          "value": "TIT创意园"
      } ,
      "site02": {
          "value": "广州市新港中路397号"
      }
  }
}
----------------------------------------------------------------------
	// 地址
	urlString := wxMsgSend + "?access_token=" + accToken.at
	// 获取时间
	postTime := getCurrentDateTimeWX()
	// 包装为map
	var sss = map[string]interface{}{
		"touser":            openid,
		"template_id":       "mGEdmENb4c23N1E4uXK6W_fvytR1iKfwOETJaKacsjE",
		"page":              "index",
		"miniprogram_state": "developer",
		"lang":              "zh_CN",
		"data": map[string]interface{}{
			"character_string1": map[string]interface{}{
				"value": "339208499",
			},
			"date3": map[string]interface{}{
				"value": postTime,
			},
			"thing5": map[string]interface{}{
				"value": "xxx投递广告",
			},
		},
	}
	// 一定要转json
	sssRes, _ := json.Marshal(&sss)
	fmt.Printf("test:%v\n", string(sssRes))

	req, err := http.NewRequest("POST", urlString, strings.NewReader(string(sssRes)))
	req.Header.Set("Content-Type", "application/x-www-form-urlencoded")
	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		// handle error
		fmt.Printf("PostForm err\n")
		return
	}

	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		// handle error
		fmt.Printf("ReadAll err\n")
		return
	}

	fmt.Println(string(body))
```
