### 网络爬虫的相关问题
>Web服务器默认接收人类访问，受限于编写水平和目的，网络爬虫会为Web服务器带来巨大的资源开销，而网络爬虫获得数据后的牟利用途将带来法律风险，网络爬虫可具备突破简单访问控制的能力（如设置密码，限制访问权限等），容易造成隐私泄露
#### 爬虫的尺寸
- Requests库：小规模，数据量小，爬取速度不敏感，但适用于90%以上的爬取需求，主要用于爬取网页
- Scrapy库：中规模，数据规模较大，爬取速度敏感（起码要大于网站数据更新速度），主要用于爬取网站及系列网站 
- 定制开发搜索引擎：大规模的搜索引擎，用于爬取全网
#### 爬虫的限制
- 来源审查：判断User-Agent进行限制，检查来访HTTP协议头的User-Agent域，只响应浏览器或友好爬虫的访问，用python的requests库访问，默认是'User-Agent': 'python-requests/2.22.0'
- 发布公告：robots.txt协议，告知所有爬虫网站的爬取策略，要求爬虫遵守
#### robots协议（Robots Exclusion Standard，网络爬虫排除标准）
- 作用：网站告知网络爬虫哪些页面可以抓取，哪些不行，如果该网站无robots协议，意味着可以无限制爬取所有内容
- 形式：在网站根目录下的robots.txt文件
robots协议基本语法：

    #注释，*代表所有，/代表根目录
    User-agent:*
    Disallow:/
    
- 对于类人类行为的爬虫爬取，可以不参考robots协议
#### 针对来源审查的解决方案
- 可以通过**r.request.headers**来查看我们发给url指定服务器的头部信息的内容，默认是'User-Agent': 'python-requests/2.22.0'，即告诉服务器这次访问是由python的request程序产生的，一些有来源审查的网站会使这样的访问出错，此时的r.status_code不等于200
- 可以通过更改头部信息，来模拟浏览器向目标服务器发起请求，首先需要构造键值对**kv={'user-agent':'Mozilla/5.0'}**,注意，这里的Mozilla/5.0说明的是此时的user-agent可能是一个浏览器（5.0表示版本号），而这个浏览器可能是火狐、Mozilla甚至是IE/10的浏览器，因为Mozilla/5.0是一个很标准的浏览器的身份标识的字段，然后在get函数中加入r=requests.get(url,headers=kv)
>通过headers字段让代码模拟浏览器向服务器提供HTTP请求，可以爬取对信息保护较好的网站的相关内容，代码如下

    import requests
    url=''
    try:
        kv={'user-agent':'Mozilla/5.0'}
        r=requests.get(url,headers=kv)
        r.raise_for_status()
        r.encoding=r.apparent_encoding
        print(r.text[1000:2000])
    except:
        print('爬取失败')

