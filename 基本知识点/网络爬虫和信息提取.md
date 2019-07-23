### Requests库及HTTP协议

#### HTTP协议
- HTTP，超文本传输协议，是一种基于'请求与响应'模式的、无状态的应用层协议
- HTTP采用URL作为定位网络资源的标识，URL格式：http:\//host[:port][path]<br>
host:合法的Internet主机域名或IP地址<br>
port:端口号，缺省端口为80<br>
path:请求资源的路径<br>
URL是通过HTTP协议存取资源的Internet路径，一个URL对应一个数据资源<br>
##### HTTP协议对资源的六种操作
- GET：请求获取URL位置的资源
- HEAD：请求获取URL位置资源的响应消息报告，即获得该资源的头部信息
- POST：请求向URL位置的资源后附加新的数据
- PUT: 请求向URL位置存储一个资源，覆盖原URL位置的资源
- PATCH：请求局部更新URL位置的资源，即改变该处资源的部分内容，相对于PUT节省网络带宽
- DELETE：请求删除URL位置存储的资源

#### requests库的7个主要方法
- requests.request()：构造一个请求，支撑以下各方法的基础方法
- requests.get()：获取HTML网页的主要方法，对应于HTTP的GET
- requests.head()：获取HTML网页头信息的方法，对应于HTTP的HEAD
- requests.post():向HTML网页提交POST请求的方法，对应于HTTP的POST<br>
向URL POST一个字符串自动编码为data,POST 一个字典自动编码为form
- requests.put()：向HTML网页提交PUT请求的方法，对应于HTTP的PUT
- requests.patch()：向HTML网页提交局部修改请求，对应于HTTP的PATCH
- requests.delete()：向HTML页面提交删除请求，对应于HTTP的DELETE
##### requests.get(url,params=None,\*\*kwargs)方法
>获取网页的基本流程：先用r=requests.get()请求对象，然后用r.status_code检查是否连接成功，如果返回是404或其他则连接失败，如果返回是200则连接成功，可以继续使用r.text,r.content,r.encoding,r.apparent_encoding等属性获取相应信息
- url：拟获取页面的URL链接
- params:URL中的额外参数，字典或字节流格式，可选
- \*\*kwargs:12个控制访问参数
- r=requests.get()函数内部调用.request()函数并请求了一个Request对象，返回的是Response对象r，包含请求页面的所有资源
##### Response对象包含了如下五个属性：
- r.status_code：HTTP请求的返回状态，200表示连接成功，404以及其他编码代表连接失败
- r.text：HTTP响应内容的字符串形式，即URL对应的页面内容
- r.content: HTTP响应内容的二进制形式
- r.encoding:从HTTP header中猜测的响应内容编码方式，如果header中不存在charset，则认为编码为ISO-8859-1，此时应该用apparent_encoding的内容赋值给r.encoding来保证r.txt的输出中不是乱码而是相应的中文
- r.apparent_encoding：从网页内容中分析出的响应内容编码方式（备选编码方式），比encoding更加准确
- 可以先查看encoding和apparent_encoding的内容，如果不同，将后者的内容赋值给r.encoding，这样得到的r.text相比之前就会少一些乱码
##### requests库的六种常用的连接异常
- requests.ConnectionError：网络连接错误异常，如DNS查询失败、拒绝连接等
- requests.HTTPError：HTTP错误异常
- requests.URLRequired：URL缺失异常
- requests.TooManyRedirects：超过最大重定向次数，产生重定向异常
- requests.ConnectTimeout：连接远程服务器超时异常
- requests.Timeout：请求URL超时，产生超时异常
- r.raise_for_status()方法针对异常，如果不是200，产生异常requests.HTTPError，如果是则跳过

#### requests库方法的参数解析
- requests.request(method,url,\*\*kwargs)<br>
method：请求方式，对应七种：'GET','HEAD','POST','PUT','PATCH','delete','OPTIONS'<br>
url：拟获取页面的URL链接<br>
\*\*kwargs：控制访问的参数，共13个<br>

**params**：字典或字节序列，（键值对）作为参数增加到url中，服务器可根据这些参数筛选部分资源返回，如<br>

    kv={'key1':'value1','key2':'value2'}
    r=requests.request('GET','http:\//python123.io\/ws',params=kv)
    print(r.url)
    http:\//python123.io\/ws?key1=value1&key2=value2    
    
**data**：字典、字节序列或文件对象，作为Request的内容来添加，向服务器提交资源，是把data存到url指定的位置作为数据来存储，如<br>

    kv={'key1':'value1','key2':'value2'}
    r=requests.request('POST','http:\//python123.io\/ws',data=kv)
    body='主体内容'
    r=requests.request('POST','http:\//python123.io\/ws',data=body)
    
**json**：JSON格式的数据，作为Request的内容，如<br>

    kv={'key1':'value1'}
    r=requests.request('POST','http:\//python123.io\/ws',json=kv)
    
**headers**：字典，HTTP定制头，可以修改HTTP协议中的某些字段，可以模拟浏览器向服务器发起访问，如<br>

    hd={'user-agent':'Chrome/10'}
    r=requests.request('POST','http:\//python123.io\/ws',headers=hd)
    
cookies:字典或CookieJar,Request中的cookie<br>
auth：元组，支持HTTP认证功能<br>

**files**:字典类型，传输文件，向某个链接提交某个文件，如<br>

    fs={'file':open('data.xls','rb')}
    r=requests.request('POST','http:\//python123.io\/ws',files=fs)
    
timeout：设定超时时间，如果在规定时间内没有请求到资源则返回timeout异常，以秒为单位，如<br>

    r=requests.request('GET','http:\//www.baidu.com',timeout=10)
    
**proxies**：字典类型，设置http访问时的代理服务器，可以增加用户名和密码的设置，这样我们访问url时使用的地址就是代理服务器的IP地址，可以有效地隐藏用户爬取网页的源IP地址信息，有效地防止对爬虫的逆追踪（防止爬取网页时泄露自己的IP地址），如<br>

    pxs={'http':'http\//user:pass@10.10.10.1:1234'
         'https':'https:\//10.10.10.1:4321'}
    r=requests.request('GET','http:\//www.baidu.com',proxies=pxs)
    
allow_redirects：True/False，默认为True，重定向开关<br>
stream：True/False，默认为True，获取内容立即下载开关<br>
verify: True/False，默认为True，认证SSL证书开关<br>
cert：保存本地SSL证书路径开关<br>
- requests.get(url,params=None,\**kwargs)
- requests.head(url,\**kwargs)
- requests.post(url,data=None,json=None,\**kwargs)
- requests.put(url,data=None,\**kwargs)
- requests.patch(url,data=None,\**kwargs)
- requests.delete(url,\**kwargs)
- 其他方法中调用的kwargs与request()方法中调用的一致，因为这些方法都是基于request函数封装而成的

#### 爬取网页的通用代码框架
    import requests
    def getHTMLText(url):
        try:
            r=requests.get(url,timeout=30)
            r.raise_for_status()#如果状态不是200，引发HTTPError异常
            r.encoding=r.apparent_encoding
            return r.txt
        except:
            return '产生异常'

