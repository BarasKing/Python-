### 利用requests实现搜索引擎关键词提交

#### 搜索引擎关键词提交接口
- 百度的关键词接口：http://www.baidu.com/s?wd=**keyword**
- 360的关键词接口：http://www.so.com/s?q=**keyword**
>其中的问号是进行url连接时自动产生的，这里需要用到params字段来进行接口的拼接

    import requests
    keyword='Python' #keyword里放你要搜索的目标
    try:
        kv={'wd':keyword}
        r=requests.get('http://www.baidu.com/s',params=kv)
        print(r.request.url)#查看Response对象提交request请求的url内容
        r.raise_for_status()
        print(len(r.txt))#查看搜索到的信息的长度
    except:
        print('爬取失败')
