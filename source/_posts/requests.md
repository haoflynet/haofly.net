---
title: "Python requests模块"
date: 2016-08-07 11:02:39
updated: 2016-09-28 09:48:00
categories: python
---
# python requests模块
一直以来我都看不惯python自带的urllib包的繁琐的使用方法，所以我都使用的requests包来代替原生方法。它能方便的发送GET和POST请求，支持HTTPS，基本上能模拟人类真实的访问。

## 发送请求

	# GET请求
	response = requests.get(url, timeout=5)
	r = requests
	
	# POST请求
	r = requests.post(url, data=参数字典) 
	
	# json请求
	r = requests.post(url, data=json.dumps(data), headers={'content-type': 'applicationjson')
	r = json()	# 获取json响应

#### 自定义HTTP头

```python
headers = {
	'User-Agent': '注意名称'
}
requests.get(url, headers=headers)
```

#### 会话对象

以这种方式可以跨请求保持某些参数，不用再自己提取上一次请求的信息了，比如cookie等，但是需要注意的是，即使使用了会话，方法级别的参数并不会跨请求保持，如果要跨方法，可以使用with

```python
s = requests.Session()
s.get(url)
r = s.get(next_url)

with requests.Session() as s:
	s.get(url)
```

#### 上传文件

类似这样的post请求，就是把文件和要发送的字段一起发送上去的

```tex
-----------------------------241652170216373
Content-Disposition: form-data; name="value_1"

12345
-----------------------------241652170216373
Content-Disposition: form-data; name="value_2"

67890

--273f13699c02429db4eb95c97f757d38
Content-Disposition: form-data; name="value_2"; filename="value_2"

67890
```

上传方式

```python
r = S.post(url, files={
  'file_0': open('filename', 'rb'),		# 这里的字段名一般都取file_0，然后顺序下去
  'file_1': open('filename2', 'rb'),
  '其他字段': (None, '字段值')
})
```

## 获取响应

	r.cookies	# 这是cookie对象
#### 重定向

```
response.status_code  #HTTP status，http状态码
# 如果直接请求，如果发生重定向，那么response.status_code = 200, response.history = 301
print([x for x,y in A.__dict__.items() if type(y) == FunctionType])
```
# TroubleShooting:
- **设置最大重试次数**  
  之前发现设置了timeout时间却没反应，原来是因为查询不到ip地址，导致在timeout时间内就已经默认在重试了，要设置就得先执行语句`requests.adapters.DEFAULT_RETRIES=5`

- **无法获取httponly的cookie信息**  
  通过`headers.get('Set-Cookie')`获取cookies字符串，然后通过正则匹配来查找

- **发送https请求出现警告: `Suppress InsecureRequestWarning: Unverified HTTPS request is being made`**

  这是因为在发送https的时候未使用证书进行认证，如果一定要关闭这个警告添加这样的语句:

  ```pythohn
  import requests
  from requests.packages.urllib3.exceptions import InsecureRequestWarning

  requests.packages.urllib3.disable_warnings(InsecureRequestWarning)
  ```

- **user-agent列表﻿**  
```
user_agent_list = [  
    'Mozilla/5.0 (Linux; Android 4.1.1; Nexus 7 Build/JRO03D) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.166  Safari/535.19',  
    'Mozilla/5.0 (Linux; U; Android 4.0.4; en-gb; GT-I9300 Build/IMM76D) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30',  
    'Mozilla/5.0 (Linux; U; Android 2.2; en-gb; GT-P1000 Build/FROYO) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1',  
    'Mozilla/5.0 (Android; Mobile; rv:14.0) Gecko/14.0 Firefox/14.0',  
    'Mozilla/5.0 (Android; Tablet; rv:14.0) Gecko/14.0 Firefox/14.0',  
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0',  
    'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:21.0) Gecko/20130331 Firefox/21.0',  
    'Mozilla/5.0 (Windows NT 6.2; WOW64; rv:21.0) Gecko/20100101 Firefox/21.0',  
    'Mozilla/5.0 (Linux; Android 4.0.4; Galaxy Nexus Build/IMM76B) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.133 Mobile Safari/535.19',  
    'Mozilla/5.0 (Linux; Android 4.1.2; Nexus 7 Build/JZ054K) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.166 Safari/535.19',  
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.93 Safari/537.36',  
    'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/535.11 (KHTML, like Gecko) Ubuntu/11.10 Chromium/27.0.1453.93 Chrome/27.0.1453.93 Safari/537.36',  
    'Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.94 Safari/537.36',  
    'Mozilla/5.0 (iPhone; CPU iPhone OS 6_1_4 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) CriOS/27.0.1453.10 Mobile/10B350 Safari/8536.25',  
    'Mozilla/4.0 (Windows; MSIE 6.0; Windows NT 5.2)',  
    'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)',  
    'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0)',  
    'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)',  
    'Mozilla/5.0 (compatible; WOW64; MSIE 10.0; Windows NT 6.2)',  
    'Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.9.168 Version/11.52',  
    'Opera/9.80 (Windows NT 6.1; WOW64; U; en) Presto/2.10.229 Version/11.62',  
    'Mozilla/5.0 (PlayBook; U; RIM Tablet OS 2.1.0; en-US) AppleWebKit/536.2+ (KHTML, like Gecko) Version/7.2.1.0 Safari/536.2+',  
    'Mozilla/5.0 (MeeGo; NokiaN9) AppleWebKit/534.13 (KHTML, like Gecko) NokiaBrowser/8.5.0 Mobile Safari/534.13',  
    'Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_6; en-US) AppleWebKit/533.20.25 (KHTML, like Gecko) Version/5.0.4 Safari/533.20.27',  
    'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/533.20.25 (KHTML, like Gecko) Version/5.0.4 Safari/533.20.27',  
    'Mozilla/5.0 (iPad; CPU OS 5_0 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3',  
    'Mozilla/5.0 (iPhone; CPU iPhone OS 5_0 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3',  
    'Mozilla/5.0 (iPad; CPU OS 5_0 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3',  
]
```