# lcPython
study Python

## 正则表达式
代码

	import re
	a = '''asdfsafhellopass:
	 234455
	 worldafdsf
	 '''
	b = re.findall('hello(.*?)world',a)
	c = re.findall('hello(.*?)world',a,re.S)
	print 'b is ' , b
	print 'c is ' , c
结果

	b is  []
	c is  ['pass:\n\t234455\n\t']

正则表达式中，“.”的作用是匹配除“\n”以外的任何字符，也就是说，它是在一行中进行匹配。这里的“行”是以“\n”进行区分的。a字符串有每行的末尾有一个“\n”，不过它不可见。


## xpath学习重点

### etree的用法

	from lxml import etree
	html ="<html><body><h1>This is a test</h1></body></html>" 
	#将html转换成_Element对象
	element = etree.HTML(html)
	text = _element.xpath('//h1/text()')
	#通过xpath表达式获取h1标签中的文本
	print('result is: ', text)
所获取的text数据类型是列表

### Xpath

- 使用xpath helper或者是chrome中的copy xpath都是从element中提取的数据，但是爬虫获取的是url对应的响应，往往和elements不一样
- 获取文本
  - `a/text()` 获取a下的文本
  - `a//text()` 获取a下的所有标签的文本
  - `//a[text()='下一页']` 选择文本为下一页三个字的a标签

- `@符号`
  - `a/@href`
  - `//ul[@id="detail-list"]`

- `//`
  - 在xpath最前面表示从当前html中任意位置开始选择
  - `li//a` 表示的是li下任何一个标签

```

	    html.xpath('//li')   #获取所有子孙节点的li节点
    	html.xpath('//li/a') #获取所有子孙节点的li节点下的所有直接a节点
    	result=html.xpath('//li[@class="item"]') 匹配class="item"的li
    	html.xpath('//a[@href="tab"]/../@class') 获取a的父节点的class值
    	html.xpath('//a[@href="tab"]/parent::*/@class')获取a的父节点的class值
    	html.xpath('//li[@class="item"]/a/text()') #获取a节点下的内容
    	html.xpath('//li[@class="item"]//text()') #获取li下所有子孙节点的内容
    	html.xpath('//li/a/@href')  #获取a的href属性
    	html.xpath('//li//@href')   #获取所有li子孙节点的href属性
    
    	按熟悉选择
    	html.xpath('//li[@class="aaa"]/a') 只能匹配class仅为'aaa'的节点
    	html.xpath('//li[contains(@class,"aaa")]/a') 匹配class有'aaa'的节点

    	and or 的用法
    	html.xpath('//li[@class="aaa" and @name="fore"]/a/text()')
    	html.xpath('//li[contains(@class,"aaa") and @name="fore"]/a/text()')
    
    	按顺序选择
    	html.xpath('//li[1]/a/text()') #获取第一个li下a的内容
    	html.xpath('//li[last()]/a/text()') #获取最后一个li下a的内容
    	html.xpath('//li[position()>2 and position()<4]/a/text()') #获取大于2小于4的
    	html.xpath('//li[last()-2]/a/text()') #获取倒数第三个
    
    	XPath提供了很多节点选择方法，包括获取子元素、兄弟元素、父元素、祖先元素等，示例如下：
    	html.xpath('//li[1]/ancestor::*')  #获取所有祖先节点
    	html.xpath('//li[1]/ancestor::div')  #获取div祖先节点
    	html.xpath('//li[1]/attribute::*')  #获取所有属性值
    	html.xpath('//li[1]/child::*')  #获取所有直接子节点
    	html.xpath('//li[1]/descendant::a')  #获取所有子孙节点的a节点
    	html.xpath('//li[1]/following::*')  #获取当前子节之后的所有节点
    	html.xpath('//li[1]/following-sibling::*')  #获取当前节点的所有同级节点
  
```
  
### lxml使用注意点
- lxml能够修正HTML代码，但是可能会改错了
  - 使用etree.tostring观察修改之后的html的样子，根据修改之后的html字符串写xpath

- lxml 能够接受bytes和str的字符串

- 提取页面数据的思路
  - 先分组，渠道一个包含分组标签的列表
  - 遍历，取其中每一组进行数据的提取，不会造成数据的对应错乱


## csv文件的读写

给字符串添加逗号
`",".join(["name","herf"])`

## strip用法
python中往往使用剥除函数strip()来对用户的输入进行清理。strip函数的最一般形式为：

str.strip('序列‘)
其中，序列是一段字符串，该函数表示从头或者从尾部开始进行扫描，如果扫描的字符在序列字符串中，则剔除掉，一直到遇到一个不在序列字符串中的字符为止。

延伸的函数:
str.lstrip('序列')，则表示仅从头部第一个字符开始扫描，如果扫描的字符在序列字符串中，则剔除掉，直到遇到一个不在序列字符串中的字符为止。
str.rstrip('序列')，则表示仅从尾部第一个字符开始扫描，如果扫描的字符在序列字符串中，则剔除掉，直到遇到一个不在序列字符串中的字符为止。
特别的情况，如果函数圆括号里面的序列字符串为空，则默认为剔除首尾部的空白（包括空格、制表符），即strip()、lstrip()、rstrip().


## request请求方法使用content和text的区别
	import requests
	resp = requests.get("http://www.baidu.com")
	resp.text          //  返回的是一个经过解码后的字符串，是unicode类型
	resp.content    // 返回的是一个原生字符串，是bytes类型

## 使用代理的方法

#### 没有授权
	proxies = {
	"http": "http://12.34.56.79:9527",
	"https": "http://12.34.56.79:9527",
	}
####使用授权
`{"http" : "user：passwd@124.88.67.81:80"}`

## pyquery的用法

[https://www.jianshu.com/p/770c0cdef481](https://www.jianshu.com/p/770c0cdef481)

## 爬虫框架Scrapy的安装与基本使用

[https://www.jianshu.com/p/6bc5a4641629](https://www.jianshu.com/p/6bc5a4641629)

## BeautifulSoup用法
Beautiful Soup 是一个可以从HTML或XML文件中提取数据的Python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式

	from bs4 import BeautifulSoup
	soup = BeautifulSoup(html_doc, 'html.parser')
	print(soup.prettify())
	
方法

	soup.title
	# <title>The Dormouse's story</title>

	soup.title.name
	# u'title'
	
	soup.title.string
	# u'The Dormouse's story'
	
	soup.title.parent.name
	# u'head'
	
	soup.p
	# <p class="title"><b>The Dormouse's story</b></p>
	
	soup.p['class']
	# u'title'
	
	soup.a
	# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
	
	soup.find_all('a')
	# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
	#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
	#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
	
	soup.find(id="link3")
	# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>


## window.scrollBy()用法

	window.scrollBy(x-coord, y-coord);
	window.scrollBy(options)


X 是水平滚动的偏移量，单位：像素。
Y 是垂直滚动的偏移量，单位：像素。

正数坐标会朝页面的右下方滚动，负数坐标会滚向页面的左上方。

options 是一个包含三个属性的对象：

top 等同于  y-coord
left 等同于  x-coord
behavior  表示滚动行为，支持参数：smooth (平滑滚动)，instant (瞬间滚动)，默认值 auto，效果等同于 instant

例子

向下滚动一页：

	window.scrollBy(0, window.innerHeight);

向上滚动一页：

	window.scrollBy(0, -window.innerHeight);

使用 options：

	window.scrollBy({   
	  top: 100,
	  left: 100,   
	  behavior: "smooth" 
	});


## enumerate() 函数
enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。
Python 2.3. 以上版本可用，2.6 添加 start 参数。

	>>>seasons = ['Spring', 'Summer', 'Fall', 'Winter']
	>>> list(enumerate(seasons))
	[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
	>>> list(enumerate(seasons, start=1))       # 下标从 1 开始
	[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]

## 列表

	序号	方法
	1	list.append(obj)
	在列表末尾添加新的对象
	2	list.count(obj)
	统计某个元素在列表中出现的次数
	3	list.extend(seq)
	在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）
	4	list.index(obj)
	从列表中找出某个值第一个匹配项的索引位置
	5	list.insert(index, obj)
	将对象插入列表
	6	list.pop([index=-1])
	移除列表中的一个元素（默认最后一个元素），并且返回该元素的值
	7	list.remove(obj)
	移除列表中某个值的第一个匹配项
	8	list.reverse()
	反向列表中元素
	9	list.sort(cmp=None, key=None, reverse=False)
	对原列表进行排序



## selenium 隐藏元素的提取

图片alt隐藏属性提取

`get_attribute('innerHTML')`

货币符号乱码可用：

	encoding='utf-8-sig'

## Windows10远程桌面Ubuntu16.04
https://blog.csdn.net/woodcorpse/article/details/80503232
