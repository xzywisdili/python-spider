# 1.3 BeautifulSoup 库的基本用法

## BeautifulSoup 库提供的解析器

```python
BeautifulSoup(markup, parser)
```

- Python 标准库：`parser` 取 `html_parser`
- lxml HTML 解析器：`parser` 取 `lxml`
- lxml XML 解析器：`parser` 取 `xml`
- html5lib：`parser` 取 `html5lib`

## 基本用法

```python
from bs4 import BeautifulSoup

html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""

soup = BeautifulSoup(html_doc, 'lxml')
print(soup.prettify())
print(soup.title.string)
```

这里使用 lxml 解析一段 html 文档。通过将文档传入 `BeautifulSoup()` 得到相应对象，就可以接下去执行格式化操作，打印`title`了。这里可以看到原文档很多标签和内容是不完全的，而 `prettify()` 不仅可以美化显示，还可以补全这些标签，变成标准格式。

## 标签选择器

同样使用上面的 HTML 文档：
```python
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.title)
# <title>The Dormouse's story</title>

print(type(soup.title))
<class 'bs4.element.Tag'>

print(soup.head)
<head><title>The Dormouse's story</title></head>

print(soup.p)
<p class="title"><b>The Dormouse's story</b></p>
```
分别返回了HTML文档的 `title`，`head` 和 `p` 标签内容。如果有多个标签，则返回第一个标签。
标签选择同样可以进行嵌套选择，如 `soup.title.head`。

## 获取标签名称，属性和内容

```python
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""

soup = BeautifulSoup(html_doc, 'lxml')

# 获取标签名称
print(soup.title.name)
# title

# 获取标签属性
print(soup.p.attrs['name'])
print(soup.p['name'])
# dromouse

# 获取标签内容
print(soup.p.string)
# The Dormouse's story
```

获取标签属性有如上两种方式。

## 子节点和子孙节点

```python
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.p.contents)
# [<b>The Dormouse's story</b>]

for i, child in enumerate(soup.p.children):
    print(i, child)
# 0 <b>The Dormouse's story</b>
```

通过 `.contents` 可以获得指定标签的子节点，返回结果使用列表列出所有子节点。
同样也可以通过 `.children` 来得到子节点，区别在于 `.children` 得到的是迭代器而非列表。

```python
print(soup.p.descendants)
for i, child in enumerate(soup.p.descendants):
    print(i, child)
```

`.descendants` 会返回所有的子孙节点。

## 父节点和祖先节点

```python
print(soup.a.parent)
print(soup.a.parents)
```

使用 `.parent` 可以返回标签的所有父节点，而 `.parents` 可以返回所有的祖先节点。

## 标准选择方法

可以通过标签名，属性或内容从 HTML 文档中进行匹配查找，选择我们想要的所有结果：
> `find_all(name, attrs, recursive, text, **kwargs)`

例如我们想查找一个 HTML 文档里面的无序列表：
```python
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all('ul'))
print(type(soup.find_all('ul')[0]))
```

我们可以通过属性进行查找：
```python
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all(attrs={'id': 'list-1'}))
print(soup.find_all(attrs={'name': 'element'}))
```

属性参数通过字典类型进行传递，键名即为属性名称，键值为我们想要查找的属性值。

我们还可以根据内容进行查找：
```python
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.find_all(text='xzy'))
```
但这个方法只会返回已经查找的文本内容，有时候并不是那么有用。

与 `find_all()` 不同的是，`find()` 只会返回单个结果，其余用法相似：
> `find(name, attrs, recursive, text, **kwargs)`

除此之外还有 `find_parents()`，`find_next_silblings()`，`find_previous_siblings()` 等等方法。
具体可参考 [BeautifulSoup 库官方文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html)
