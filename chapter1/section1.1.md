# 1.1 urllib 库的基本用法

## url 解析方法

```python
from urllib.parse import urlparse

url = "http://www.baidu.com/index.html;user?id=5#comment"
result = urlparse(url)
print(type(result))
print(result)

# <class 'urllib.parse.ParseResult'>
# ParseResult(scheme='http', netloc='www.baidu.com', path='/index.html', params='user', query='id=5', fragment='comment')
```

使用 `urlparse` 函数对 `url` 进行解析，得到 `ParseResult` 类型的结果，将 `url` 分成 6 个部分：`scheme` ，`netloc`， `path`， `params`， `query`， `fragment`。

我们可以通过 `scheme` 参数指定 `url` 的协议类型，通过 `allow_fragment` 参数来指定是否解析锚点链接（指定 `False` 会把锚点拼接到前一个部分）：

```python
result = urlparse("http://www.baidu.com/index.html;user?id=5#comment", scheme="https", allow_fragment=False)
```

同样，使用 `urlunparse` 函数能够将上面提到的各个部分组合成 `url`：

```python
from urllib.parse import urlunparse

data = ['https', 'www.baidu.com', 'index.html', 'user', 'id=5', 'comment']
print(urlunparse(data))
```

`urljoin` 函数可以对 `url` 进行拼接。如果前面的字段没有后面的字段，则进行拼接，否则以后面的字段为准。

`urlencode` 函数可以将字典转换成链接中的请求参数：

```python
from urllib.parse import urlencode

params = {
    "name": "xzy",
    "id": 5
}
url = "http://www.baidu.com?" + urlencode(params)
print(url)
```

`urllib.robotparser` 用于解析网站的 `robot.txt`，具体介绍和示例可见[官方文档](https://docs.python.org/3/library/urllib.robotparser.html)。







