# 1.2 Requests 库的基本用法

`requests`库是 Python 写成的易用的 HTTP 库。通过一个简单的例子了解 `requests` 库的基本用法，这里传入百度的网址，并展示了应答的基本信息（类型、状态码等等）：

```python
import requests

response = requests.get("http://www.baidu.com")
print(type(response))
# <class 'requests.models.Response'>

print(response.status_code)
# 200

print(type(response.text))
# <class 'str'>

print(response.text)
# <!DOCTYPE html>...

print(response.cookies)
# <RequestsCookieJar[<Cookie BDORZ=27315 for .baidu.com/>]>
```

`requests`库提供了 `post`，`get`，`delete`，`head`，`options` 等不同的请求方法。

## 基本 GET 请求

最基本 GET 请求可参考上面的示例，我们还可以进行带参数的 GET 请求：

```python
response = requests.get("http://httpbin.org/get?name=xzy&id=5")
print(response.text)
'''
{
  "args": {
    "id": "5", 
    "name": "xzy"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Aocept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.21.0"
  }, 
  "origin": "106.44.2.230", 
  "url": "http://httpbin.org/get?name=xzy&id=5"
}
'''
```

我们还可以把参数保存在字典结构里面：

```python
data = {
    "name": "xzy",
    "id": 5
}

response = requests.get("http://httpbin.org/get", params=data)
print(response.text)
```

`requests` 库还提供了解析 `json` 文件的方法，可以直接使用 `response.json()`

此外，我们还可以获取二进制文件的内容：

```python
response = requests.get("https://github.com/favicon.ico")
print(response.content)
with open('favicon.ico', 'wb') as f:
    f.write(response.content)
    f.close()
```

使用 `headers` 参数来添加 `headers` 信息。

## 基本 POST 请求

```python
data = {
    "name": "xzy",
    "id": 5
}
response = requests.post("http://httpbin.org/post", data=data)
print(response.text)

"""
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "id": "5", 
    "name": "xzy"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Content-Length": "13", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.21.0"
  }, 
  "json": null, 
  "origin": "106.44.2.230", 
  "url": "http://httpbin.org/post"
}
"""
```

其他和 `get` 方法类似。

## 响应属性

从 `requests` 库中返回得到的响应 `response` 具有状态码 `status_code`，`cookies`，`headers`，`url`，`history`

我们可以直接判断状态码，并决定下一步的工作：

```python
# Request Successfully
response = requests.get("http://www.baidu.com")
exit() if not response.status_code == requests.codes.ok else print("Request Successfully")

# 404 not found
response = requests.get("http://www.jianshu.com/xzy.html")
exit() if not response.status_code == requests.codes.not_found else print("404 not found")
```

## 其他操作

以下直接展示几种操作的相关代码。

### 文件上传

```python
files = {"file": open("favicon.ico", "rb")}
response = requests.post("http://httpbin.org/post", files=files)
print(response.text)
```

### 获取 cookies

```python
response = requests.get("http://www.baidu.com")
for key, item in response.cookies.items():
    print(key, " = " , item)
```

### 维持会话

```python
s = requests.Session()
s.get("http://httpbin.org/cookies/set/number/123456789")
response = s.get("http://httpbin.org/cookies")
print(response.text)
```

这里为了保持模拟一个浏览器进行两次 `get` 请求，使用了 `Session()` 会话。

### 使用代理

```python
proxies = {
    "http": "http://104.193.226.137:443",
    "https": "https://104.193.226.137:443"
}
response = requests.get("https://www.taobao.com", proxies=proxies)
print(response.status_code)
```

### 异常处理

```python
from requests.exceptions import ReadTimeout
try:
    response = requests.get("http://www.baidu.com", timeout=0.1)
    print(response.status_code)
except ReadTimeout:
    print("Timeout")
```

更多信息可以查看 `Requests` 库的[官方文档](http://docs.python-requests.org/en/master/)。