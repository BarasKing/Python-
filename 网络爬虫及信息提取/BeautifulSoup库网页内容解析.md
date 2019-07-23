### BeautifulSoup库
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
