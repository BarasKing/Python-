### Scrapy爬虫框架
>scrapy不是一个函数功能库，而是一个爬虫框架，用户主要是对Spider和Item Pipelines以及两个中间件进行编写配置代码
#### 基本介绍
- 爬虫框架是实现爬虫功能的一个软件结构和功能组件集合<br>
爬虫框架是一个半成品，能够帮助用户实现专业网络爬虫<br>
- “5+2”结构：其中ENGINE模块、SCKEDULER模块、DOWNLOADER模块是Scrapy框架已有的实现，而框架的入口SPIDERS和出口ITEM PIPELINES是由用户来编写配置的
- Downloader Middleware：（中间件）<br>
目的：实施Engine、Scheduler和Downloader之间进行用户可配置的控制<br>
功能：修改、丢弃、新增请求或响应<br>
用户可以编写配置代码<br>
- Spider：解析Downloader返回的响应（Response）；产生爬取项（scraped item）；产生额外的爬取请求（Request）；需要用户编写配置代码
- Item Piplines：以流水线方式处理Spider产生的爬取项；由一组操作顺序组成，类似流水线，每个操作是一个Item Pipeline类型；可能操作包括：清理、检验和查重爬取项中的HTML数据、将数据存储到数据库；需要用户编写配置代码
- Spider Middleware：（中间件）<br>
目的：对请求和爬取项的再处理<br>
功能：修改、丢弃、新增请求或爬取项<br>
用户可以编写配置代码<br>
- 常用命令：Scrapy是为持续运行设计的专业爬虫框架，提供操作的**Scrapy命令行**<br>
命令行格式：>scrapy<command>[options][args] 其中command是scrapy是具体命令<br>
startproject：创建一个新工程 格式：scrapy startproject<name>[dir]<br>
genspider：创建一个爬虫 格式：scrapy genspider [options]<name><domain><br> 
settings：获得爬虫的配置信息 格式：scrapy settings [options]<br>
crawl：运行一个爬虫 格式：scrapy crawl<spider><br>
list：列出工程中所有爬虫 格式：scrapy list<br>
shell：启动URL调试命令行 格式：scrapy shell [url]<br>

操作步骤如下：
    
    #步骤一、生成一个工程文件
    在命令行中打开你想要存放爬虫工程文件的目录
    键入 scrapy startproject python123demo(文件名) ->生成一个工程目录
    目录下分别有：
    python123demo/ 外层目录
      scrapy.cfg ->部署scrapy爬虫的配置文件，这里的部署指将爬虫放在特定的服务器上，并在服务器上配置好相关的操作接口，对本机使用的爬虫来说，不需要更改部署的配置文件
      python123demo/->scrapy框架的用户自定义python代码，包含很多Python文件
        __init__.py->初始化脚本，用户不需要编写
        items.py->Items类的代码模板（继承scrapy库提供的item类），对于一般的例子，用户不需要编写
        middlewares.py->Middlewares代码模板（继承类），如果用户需要扩展middlewares的功能，则需要把扩展功能写到这个文件中
        pipelines.py->Piplines代码模板（继承类）
        settings.py->Scrapy爬虫的配置文件，如果希望优化爬虫功能，就要修改其中对应的配置项
        spiders/->Spiders代码模板目录（继承类），爬虫需要符合模板的约束
          __init__.py->初始文件，无需修改
          __pycache__/->缓存目录，无需修改

    #步骤二、在工程中产生一个scrapy爬虫
    键入 cd python123demo 打开你创建的工程目录
    键入 scrapy genspider demo(爬虫名) python123.io() #生成一个名称为demo的scrapy爬虫
    此时，在spiders目录下增加了一个demo.py文件（用上述命令生成），其内部代码如下：
    # -*- coding: utf-8 -*-
    import scrapy

    class DemoSpider(scrapy.Spider): #用面向对象方式编写了一个类Demospider，由于我们输入的名字叫demo，所有类名也叫DemoSpider，这个类必须是继承scrapy.Spider的子类
        name = 'demo' #当前爬虫名叫demo
        allowed_domains = ['python123.io'] #一开始用户输入的域名，爬虫只能爬取这个域名以下的相关链接
        start_urls = ['http://python123.io'] #以列表形式包含的一个或多个url就是scrapy框架所要爬取页面的初始页面

        def parse(self, response): #解析页面的空的方法，用于处理响应，解析从网络中爬取的内容形成字典类型，还能对网络中爬取的内容发现隐含的新的url
            pass
    
    #步骤三、配置产生的spider爬虫
    即修改demo.py文件，使他能够按照我们的要求访问链接并对相关内容进行爬取，这里定义的解析部分是将返回的HTML内容输出到一个文件中
    allow_domains这一行其实可以注释掉
    修改start_urls为想要爬取的链接：start_urls = ['http://python123.io/ws/demo.html]
    更改爬取方法的具体功能，爬取方法有两个参数，一个叫self,是面向对象类所属关系的标记；还有一个叫response，相当于从网络中返回内容所对应的对象。
    这里将返回的response对象的内容写到一个html文件中，首先定义这个文件的名字：（从响应的URL中提取文件名作为保存本地的文件名，然后将返回的内容保存为html文件）：
    def parse(self,response):
        fname=response.url.split('/')[-1]
        with open(fname,'wb') as f:
            f.write(response.body)
        self.log('Saved file %s.' %name)
    
    #步骤四、运行爬虫，获取网页
    在cmd输入scrapy crawl demo
    demo爬虫被执行，捕获页面存储在demo.html页面中

#### yield关键字的使用
>yield<=>生成器，生成器是一个不断产生值的函数，包含yield语句的函数是一个生成器，生成器在每次运行时会产生一个值（yield语句），产生值之后生成器会被冻结，直到这个函数被再次唤醒的时候，生成器继续执行，产生一个新的值，而生成器唤醒时所使用的局部变量的值跟之前执行所使用的值是一致的（也就是冻结前的值还能继续被使用），这样每次执行的时候就会产生一个数据，如果持续执行就会产生源源不断的数据
- 常在函数的for循环里执行yield，这样相当于外部每调用一次这个函数，函数里的for循环才会被唤醒进行一次遍历，返回这次遍历的结果，这样一来就不用把所有的结果存在一个列表里同时输出，而是一次只输出一个变量的值，占用的存储空间大大减少

举例如下：

    def gen(n):
        for i in range(n):
            yield i**2
     #这个函数执行时会首先执行for循环，当执行到yield这行语句时，这个函数将会被冻结，而当前yield所在行产生的值会被返回出来
     也就是说第一次调用gen函数时，会返回0**2，然后函数冻结，第二次调用的时候会返回1**2，然后继续冻结...直到for循环结束
     
     for i in gen(5):
        print(i,'',end='')
     0 1 4 9 16
     
     #另一种普通写法：
     def square(n):
        ls=[i**2 for i in range(n)]
        return ls
      for i in square(5):
        print(i,'',end='')
      0 1 4 9 16
生成器相比一次全部输出的优势：更节省存储空间、响应更迅速、使用更灵活,以下是scrapy框架中提交请求的等价写法

    def start_requests(self):
        urls=[
                'http://python123.io/ws/demo.html'
              ]
        for url in urls: #一次提交一个请求，尤其是在要访问的链接数十分庞大时，可以有效节省计算资源
            yield scrapy.Request(url=url,callback=self.parse)
    
