### BeautifulSoup库

#### BeautifulSoup库基本原理和使用方法
>基本原理是把任何给它的文档（此时它无法判别你给的文档是什么类型）当做一锅汤，并利用你输入的解析器来煲制这锅汤，注意导入库是import bs4
- 查看某个网页的html源代码有两种途径，一种是在网页中直接右键点击查看源代码，另一种是利用requests库，r.text就是源码内容（杂乱版）
- soup=BeautifulSoup(demo,'html.parser')，除了给出demo（源码即r.text）之外，还要给出解析demo的解释器，html.parser的意思是对demo进行html的解析，然后通过print(soup.prettify())来输出解析后的内容

BeautifulSoup库的使用方式：

    from bs4 import BeautifulSoup #BeautifulSoup是一个类
    soup=BeautifulSoup('<p>data</p>','html.parser') #第一个参数是我们需要解析的html格式的信息可以是变量，第二个参数是解析这个变量使用的解析器
    
在cmd上的测试结果：

    >>> import requests
    >>> r=requests.get('https://python123.io/ws/demo.html')
    >>> r.text #得到的是杂乱无章的源码信息，可以被看作是一个没有html标签的字符串文档，需要被BeautifulSoup库解析后才能使用
    '<html><head><title>This is a python demo page</title></head>\r\n<body>\r\n<p class="title"><b>The demo python introduces several python courses.</b></p>\r\n<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:\r\n<a href="http://www.icourse163.org/course/BIT-268001" class="py1" id="link1">Basic Python</a> and <a href="http://www.icourse163.org/course/BIT-1001870001" class="py2" id="link2">Advanced Python</a>.</p>\r\n</body></html>'
    >>> demo=r.text
    >>> from bs4 import BeautifulSoup
    >>> soup=BeautifulSoup(demo,'html.parser')
    >>> print(soup.prettify())
    <html>
     <head>
      <title>
       This is a python demo page
      </title>
     </head>
     <body>
      <p class="title">
       <b>
        The demo python introduces several python courses.
       </b>
      </p>
      <p class="course">
       Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
       <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">
        Basic Python
       </a>
       and
       <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">
        Advanced Python
       </a>
       .
      </p>
     </body>
    </html>

#### Beautiful Soup库的基本元素（也叫beautifulsoup4或bs4）
>Beautiful Soup库是解析、遍历、维护'标签树'的功能库（可处理HTML和XML），主要是用来解析标签类型的文件
标签格式

    <p>..</p>:标签 Tag
    <p class='title'>...</p> # p是这个标签的名Name，一般是成对出现，标明这个标签的范围
    #在第一对<>中除了名字p之外还存在一个相关的域，叫属性域，包含0个或多个属性Attributes，
    #用来定义标签的特点，而属性是由键值对构成的
引用的基本方式

    from bs4 import BeautifulSoup 
- Beautiful Soup类解析的是HTML和XML的文档，而这个文档是跟标签树（可以理解为一个字符串）一一对应的，经过BeautifulSoup类的处理，可以将标签树转化为BeautifulSoup类，事实上可以认为HTML文档、标签树和BeautifulSoup类是等价的，因此对BeautifulSoup类的处理就是对标签树的相关处理

导入HTML的使用方法，BeautifulSoup类对应一个HTML/XML文档的全部内容

    from bs4 import BeautifulSoup
    soup=BeautifulSoup('<html>data</html>','html.parser')
    soup2=BeautifulSoup(open('D://demo.html'),'html.parser')
- Beautiful Soup库解析器<br>
bs4的HTML解析器：soup=BeautifulSoup(mk,'html.parser') 条件：只要安装了bs4库即可使用<br>
lxml的HTML解析器：soup=BeautifulSoup(mk,'lxml') 条件：pip3 install lxml<br>
lxml的XML解析器：soup=BeautifulSoup(mk,'xml') 条件：pip3 install lxml<br>
html5lib的解析器：soup=BeautifulSoup(mk,'html5lib') 条件：pip3 install html5lib<br>

- BeautifulSoup类的五种基本元素<br>
Tag：标签，最基本的信息组织单元，分别用<>和</>标明开头和结尾<br>
Name：标签的名字，<p>...</p>的名字是'p'，格式：<tag>.name<br>
Attributes：标签的属性，字典组织形式，格式：<tag>.attrs<br>
NavigableString：标签内非属性字符串，<>...</>中字符串，即两个<>之间的内容且为一个字符串，格式：<tag>.string<br>
Comment：标签内字符串的注释部分，一种特殊的Comment类型<br>

>对页面的基本操作如下

    >>> from bs4 import BeautifulSoup
    >>> import requests
    >>> url='https://python123.io/ws/demo.html'
    >>> r=requests.get(url)
    >>> demo=r.text #得到页面的文档
    >>> soup=BeautifulSoup(demo,'html.parser') #对文档用解析器进行解析，获取页面的HTML信息
    >>> soup
    <html><head><title>This is a python demo page</title></head>
    <body>
    <p class="title"><b>The demo python introduces several python courses.</b></p>
    <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>
    </body></html>
    
    #title 标题
    >>> soup.title # 获取页面的标题栏内容，即最上面的网页名称栏
    <title>This is a python demo page</title>
    
    #tag 标签
    >>> tag=soup.a #获取第一个标签a的内容并赋值给tag，注意：标签内容可由'tag=soup.标签名'来获取，此时的tag就是两个<>以及它们之间的内容，如果html中存在多个标签重名的情况，返回的tag则是html中的第一个标签内容
    >>> tag #查看标签tag的内容，包含两个<>以及之间的内容
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>
    >>> type(tag) #查看标签tag的类型，为bs4中定义的标签类型
    <class 'bs4.element.Tag'>
    
    #Name 标签名
    >>> tag.name #查看tag的标签名，显示为字符串类型
    'a'
    >>> tag.parent.name #查看tag父亲的标签名，所谓父亲指的是包含标签tag的上一层标签
    'p'
    >>> tag.parent.parent.name #查看tag父亲的父亲的标签名
    'body'
    
    #Attributes 属性字符串
    >>> tag=soup.a
    >>> tag.attrs #查看标签tag的属性Attributes的内容，为字典类型，显示出属性名和属性值的对应关系
    {'href': 'http://www.icourse163.org/course/BIT-268001', 'class': ['py1'], 'id': 'link1'}
    >>> tag.attrs['class'] #可以采用字典方式对每个属性进行信息提取
    ['py1']
    >>> tag.attrs['href'] #可以采用字典方式对每个属性进行信息提取
    'http://www.icourse163.org/course/BIT-268001'
    >>> type(tag.attrs) #查看属性的类型，为字典类型，如果没有属性，得到是空字典
    <class 'dict'>
    
    #NavigableString 非属性字符串
    >>> tag.string #查看标签tag的非属性string的内容，是两个<>之间的内容，且为字符串
    'Basic Python'
    >>> type(tag.string) #查看string的类型，为bs4定义的类型
    <class 'bs4.element.NavigableString'>
    >>> soup.p #查看p标签内容
    <p class="title"><b>The demo python introduces several python courses.</b></p>
    >>> soup.p.string #查看p标签的非属性string内容，注意在p标签中还包含有b标签，但是当输出string的时候并没有打印b标签，说明NavigableString是可以跨越多个标签层次的
    'The demo python introduces several python courses.'
    
    #Comment 注释 （在HTML页面中<!...>的形式表示注释）
    >>> newsoup=BeautifulSoup('<b><!--This is a comment--></b><p>This is not a comment</p>','html.parser')
    >>> newsoup.b.string #查看b标签的内容（注释）
    'This is a comment'
    >>> type(newsoup.b.string) #查看b标签的类型
    <class 'bs4.element.Comment'> #是bs4定义的注释类型
    >>> newsoup.p.string #查看p标签的内容
    'This is not a comment'
    >>> type(newsoup.p.string) #查看p标签的类型
    <class 'bs4.element.NavigableString'> #是bs4定义的非属性字符串类型
    #注意，注释和非注释在显示内容的时候并没有额外标记，只能通过类型查看，不过这种场景应用不多

#### 基于bs4库的HTML遍历内容方法
>遍历分为三种方式：下行遍历，上行遍历，水平遍历，其中的迭代类型只能用在for-in的遍历当中

##### 标签树的下行遍历
- .contents：子节点的列表，将<tag>所有儿子节点存入列表
- .children：子节点的迭代类型，与.contents类似，用于循环遍历儿子节点
- .descendants：子孙节点的迭代类型，包含所有子孙节点，用于遍历循环

cmd测试内容如下：
    
    soup=BeautifulSoup(demo,'html.parser') #获取页面的HTML信息
    >>> soup.head #查看head头标签的内容
    <head><title>This is a python demo page</title></head> #含有两层标签
    >>> soup.head.contents #获取head标签的子节点的列表
    [<title>This is a python demo page</title>] #其子节点为title标签
    
    #.contents获得存储为列表类型的子节点，len得到子节点个数，并用列表下标索引取值
    >>> soup.body.contents #获取body标签的子节点的列表，子节点并不仅仅包括标签节点，也包括字符串节点，比如'\n'等也算子节点
    ['\n', <p class="title"><b>The demo python introduces several python courses.</b></p>, '\n', <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2"href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>, '\n']
    >>> len(soup.body.contents) #用len函数查看body标签儿子节点的数量
    5
    >>> soup.body.contents[1] #利用列表类型的下标查看body标签的第一个儿子节点的内容
    <p class="title"><b>The demo python introduces several python courses.</b></p>
    
    #.children遍历儿子节点
    >>> soup.body.children #查看body标签的子节点children类型的内容
    <list_iterator object at 0x000001E53C2ECB38>
    >>> len(soup.body.children) #查看children类型的长度，得知此类型无法计算长度
    Traceback (most recent call last):
      File "<pyshell#53>", line 1, in <module>
        len(soup.body.children)
    TypeError: object of type 'list_iterator' has no len()
    >>> for child in soup.body.children: #遍历输出body子节点children类型的内容，空行是由'\n'输出造成的，由于print附带了一个'\n'，因此会空两行
            print(child)


    <p class="title"><b>The demo python introduces several python courses.</b></p>


    <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>


    #.descendants遍历子孙节点
    >>> soup.body.descendants #查看子孙节点descendants的内容
    <generator object Tag.descendants at 0x000001D4A58CF138>
    >>> len(soup.body.descendants) #查看其长度，同上，没有长度
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: object of type 'generator' has no len()
    >>> for child in soup.body.descendants: #遍历输出子孙节点，空格原因同上
            print(child)


    <p class="title"><b>The demo python introduces several python courses.</b></p>
    <b>The demo python introduces several python courses.</b>
    The demo python introduces several python courses.


    <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>
    Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:

    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>
    Basic Python
     and
    <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>
    Advanced Python
    .

    
##### 标签树的上行遍历
- .parent：返回节点的父亲标签
- .parents：返回节点先辈标签的迭代类型，用于循环遍历先辈节点

cmd测试结果如下：

    >>> soup.title.parent #显示title标签的父亲标签
    <head><title>This is a python demo page</title></head>
    >>> soup.html.parent #显示html标签的父亲标签，即本身
    <html><head><title>This is a python demo page</title></head>
    <body>
    <p class="title"><b>The demo python introduces several python courses.</b></p>
    <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>
    </body></html>
    >>> soup.parent #显示soup的父亲标签，没有显示，即soup的父亲为空
    
    #标签树的上行遍历
    soup=BeautifulSoup(demo,'html.parser')
    >>> for parent in soup.a.parents: 对soup.a标签所有先辈的名字进行打印，在遍历的过程中会遍历到soup本身，而soup的先辈并不存在.name信息，需要做一个区分，如果其先辈是None，就不打印这方面的信息
            if parent is None:
                    print(parent)
            else:
                    print(parent.name)
    p
    body
    html
    [document]
##### 标签树的平行遍历
- .next_sibling：返回按照HTML文本顺序的下一个平行节点标签
- .previous_sibling：返回按照HTML文本顺序的上一个平行节点标签
- .next_siblings：迭代类型，返回按照HTML文本顺序的后续所有平行节点标签
- .previous_siblings：迭代类型，返回按照HTML文本顺序的前续所有平行节点标签
- 所有的平行遍历必须发生在同一个父节点下的各节点间，不同父节点的节点之间并不构成平行关系，遍历并不能排除Navigablestring

遍历形式如下：
    
    for sibling in soup.a.next_siblings: #遍历a的后续节点
        print(sibling)
    for sibling in soup.a.previous_siblings: #遍历a的前续节点
        print(sibling)
 
#### bs4库的prettify()方法
- 

实验效果如下：

    >>> from bs4 import BeautifulSoup
    >>> soup=BeautifulSoup(demo,'html.parser')
    >>> soup #查看soup原本的内容
    <html><head><title>This is a python demo page</title></head>
    <body>
    <p class="title"><b>The demo python introduces several python courses.</b></p>
    <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>
    </body></html>
    >>> soup.prettify() #对soup进行prettify方法使用后的内容，每一项后面多了\n
    '<html>\n <head>\n  <title>\n   This is a python demo page\n  </title>\n </head>\n <body>\n  <p class="title">\n   <b>\n    The demo python introduces several python courses.\n   </b>\n  </p>\n  <p class="course">\n   Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:\n   <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">\n    Basic Python\n   </a>\n   and\n   <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">\n    Advanced Python\n   </a>\n   .\n  </p>\n </body>\n</html>'
    >>> print(soup.prettify()) #把soup进行prettofy方法后的内容输出，分行明显
    <html>
     <head>
      <title>
       This is a python demo page
      </title>
     </head>
     <body>
      <p class="title">
       <b>
        The demo python introduces several python courses.
       </b>
      </p>
      <p class="course">
       Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
       <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">
        Basic Python
       </a>
       and
       <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">
        Advanced Python
       </a>
       .
      </p>
     </body>
    </html>
    
    #bs4和python3.x都支持utf-8编码    
    >>> soup=BeautifulSoup('<p>中文</p>','html.parser')
    >>> soup.p.string
    '中文'
    >>> print(soup.p.prettify())
    <p>
     中文
    </p>
