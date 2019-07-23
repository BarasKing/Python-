### 利用ip138网站提交请求
>网址：www.ip138.com，实际上用爬虫本质就是模拟人的提交请求，只需要得到该网站的api接口即可实现
- 通过ip138网站提交IP地址的url链接是如下的接口形式：http://m/ip138.com/ip.asp?ip=**ipaddress**，其中的ipaddress就是你要查询的IP地址
>ip地址查询完整代码如下：

    import requests
    url='http://m.ip138.com/ip.asp?ip='
    try:
        r=requests.get(url+'202.204.80.112')
        r.raise_for_status()
        r.encoding=r.apparent_encoding
        print(r.text[-500:])#输出最后的一段内容，而非全部输出
    excpet:
        print('爬取失败')
