### 数值运算函数
> 这些函数都属于python内置函数，可以被所有程序直接调用
- abs(x)，返回x的绝对值
- divmod(x,y)，商余操作，得到的结果是(x//y,x%y)即x与y的整除商和余数，如divmod(10,3)=(3,1)
- pow(x,y[,z])，幂余操作，得到的结果是(x\**y)%z即取x的y次幂的整除z的余数，如果不输入z，得到的结果是x\**y，如pow(3,pow(3,99),10000)=4587
- round(x[,d])，四合五入保留d位小数，如果不输入d则默认为取整，如round(-10.123,2)=-10.12,还可以用来比较浮点数相加是否相等，用去舍去不确定尾数
- max(x,y,z,...)和min(x,y,z,...)用于取最大值和最小值
- int(x) 可以将浮点数x取整，也可以将字符串x变为整数，注意此时的字符串x必须为整数而不能为浮点数，如果int('12.22')则会报错
- float(x) 可以将x变成浮点数，x可以是整数也可以是整数或者浮点数的字符串
- str(x) 可以将任意类型的x转化成字符串，如str(1.23)='1.23',str([1,2])='[1,2]'
- hex(x) 将整数x转化为十六进制的小写字符串形式，如hex(425)='0x1a9'
- oct(x) 将整数转化为八进制的小写字符串形式，如oct(425)='0o651'
- chr(u) 将Unicode编码转化为对应字符
- ord(s) 将字符s转化为对应的Unicode编码
- map(函数名，组合数据类型)（即对第二个参数代表的组合数据类型的每一个元素，都进行一次第一个参数代表的函数操作，生成一个map类型），如：map(eval,line.split(',')) 这里line是一个用逗号分隔的字符串类似于'12,33,87,123',此时先把line转化为分割后的列表['12','33','87','123']，然后利用map函数，用eval对列表中的每个元素都执行一次操作，当然此时生成的还是map类型，需要用ls=list(map(eval,line.split(',')))将之转化为列表

### 常用库及python使用注意点
>以下是一些tips
- '{0:.2f}'.format(1.2345)=1.23，.2f表示浮点数小数精度，即取小数点后两位；'{0:.2}'.format(1.2345)=1.2，.2表示字符串长度精度，即字符串的输出最大长度是2
- import time 标准库，time.time()获取1970s至今的秒数，time.ctime()返回当前时间，年月日时分秒星期，以及time.gmtime用于返回计算机内部时间变量格式，可以定义模板来输出相应的效果时间，如t=time.gmtime() time.strftime('%Y-%m-%d %H:%M:%S',t)->'2019-07-21 22:03:22'<br>
perf_counter() 返回一个CPU级别的精确时间计数值，单位为秒，由于这个计数值起点不确定，连续调用差值才有意义。如：start=time.perf_counter() end=time.perf_counter() t=end-start 获得的t为这个阶段的时间<br>
sleep(s) 使进程休眠s秒
- print()函数默认带换行符，如果要不换行，则输出时加end='',即print(s,end='')此时print输出后不会换行，而\r可以使光标回到当前行的最前面，这就可以使刷新本行的输出内容，如for i in range(10):print('\r{:^3.0f}%'.format(),end='') .0f表示不保留小数，注意在ide中\r功能被屏蔽，只有才cmd或shell中才能显示效果，如果做文本进度条，最好配合time.sleep(0.2)使变化不要这么快
- 二分支结构的紧凑形式：表达式1 if 条件 else 表达式2，若满足条件则返回表达式1，否则返回表达式2<br>
在python中，条件语句与其他语言有所不同：<br>
比如，多分支结构是if-elif-else<br>
比如，与逻辑不是用&&，而是用and；或逻辑不是用||，而是用or；非逻辑不是用！，而是用not。如if a>b and a>c :...； if a>b or a>c :...；if not a ：...<br>
比如，在其他语言中2<x<=4这样的形式是错误的，但是在python中是可以使用这种方式来进行同时的逻辑判断的
- 异常处理：try:语句块1 except:语句块2（程序运行出错后运行） else:语句块3（程序正常运行后才会运行） finally：语句块4（始终会执行）
- print('adad',32) 会输出adad 32而不会报错，中间有一个空格,print(s,end=',')表示不换行并且输出的s末尾跟一个逗号
- 用CTRL+C组合键可以退出某些无限循环，或者循环时间较长的程序
- 在for和while循环后可以加一个else：语句块，这个else只有当循环里的语句没有产生异常且正常完成时（不能用break退出，但可以用continue），才会执行
- random库，random.seed(10)，产生种子10对应的随机数序列，若不用seed指定随机数种子，则默认为当前系统时间为随机数种子<br>
random.random()生成一个[0.0,1.0)之间的随机小数;randint(a,b)生成一个[a,b]之间的随机整数，如random.randint(10,100)=64<br>
randrange(m,n[,k])生成一个[m,n)之间以k为步长的随机整数，如random.randrange(10,100,10)=80<br>
uniform(a,b)生成一个[a,b]之间的随机小数，如random.uniform(10,100)=13.09213681236<br>
- 如果某行代码特别长，可以在这段代码中增加\来进行换行，即将这一行代码分为好几行，并在每一行的后面跟一个\使之被系统看做是同一行
- 函数定义中，可以用可选参数，如def func(a,b=1)，此时b为可选参数，如果调用函数时输入b，则用你输入的值，否则就用默认值<br>
可以用可变参数，如def func(a,\*b)，此时b可以是一个参数也可以是多个参数,典型的如max和min函数<br>
参数传递可以用默认位置传递或者按照参数名称传递，如def func(a,b=1)  位置：func(8,3) 名称：func(b=3,a=8)<br>
函数内部如果要调用一般的全局变量，需要对全局变量进行global声明，但如果是调用组合数据类型，且在函数内部并没有创建这个组合数据类型，而全局变量中又恰好有这个组合数据类型，则相当于在函数内部调用全局变量的组合数据类型，此时无需声明global,但如果在函数内部被真实创建，则不会调用全局变量<br>
- PyInstaller库<br>
pyinstaller -h 查看帮助<br>
pyinstaller -F 在dist文件夹中生成独立文件，其中的build文件和cache文件可删除<br>
pyinstaller -i 图标名.ico -F 源文件名.py 即可生成指定图标的打包文件<br>
- 文件读取操作：<br>
打开文件：f=open('文件路径','打开方式')，其中打开方式有:r（只读），w（覆盖写），a（追加写），x（创建写），r+,w+,a+（同时有读写功能）<br>
内容读取：s=f.read(size=-1) （读入全部内容到字符串s中，如果给出参数，则读入前size长度）；　s=f.readline(size=-1)（读入一行内容，如果给出参数，读入该行前size长度）；　s=f.readlines(hint=-1)（读入文件所有行，以每行为元素形成列表，如果给出参数，读入前hint行）；<br>
文件逐行遍历：for line in f.readlines()：print(line) 或者 for line in f:print(line)，前者要将文件全部读入，耗费内存，而后者并不在内存中读入文件，因此效率更高<br>
文件写入：f.write(s)（向文件写入一个字符串s）； f.writelines(ls)（将一个元素全为字符串的列表写入元素，注意此操作是先把列表中的字符串合成一个字符串，然后统一写入，并不会分行写入）<br>
文件指针操作函数：f.seek(offset)（改变当前文件操作指针的位置，offset=0即为文件开头，offset=1即为当前位置，offset=2即为文件结尾）<br>
关闭文件：f.close()<br>
- wordcloud库：<br>
分隔：以空格分隔单词；统计：单词出现次数并过滤出现少的；字体：根据统计配置字号；布局：颜色环境尺寸<br>
创建词云对象：w=worldcloud.WorldCloud()（创建词云对象）<br>
加载词云文本：w.generate(txt)（向WorldCloud对象w中加载文本txt）如w.generate('python and wordcloud')<br>
输出词云文件：w.to_file(filename)（将词云输出为图像文件，可以是.jpg或.png格式）如w.to_file('outfile.png')在当前目录下输出图像文件，也可以加绝对路径<br>
**设置词云对象参数**：w=worldcloud.WorldCloud(width=400,height=200，min_font_size=4,max_font_size=自动,font_step=1,font_path=None,max_words=200,stop_words=集合)<br>
其中的参数有：<br>
1、图片参数：生成图片的宽度width,默认400;高度height,默认200<br>
2、词云字体参数：字体最小字号min_font_size,默认4号;最大字号max_font_size,根据高度自动调节<br>
3、词云中字体字号的步进间隔：font_step,默认为1<br>
4、指定字体文件的路径：font_path,默认为None，微软雅黑是'msyh.ttc'<br>
5、指定词云显示的最大单词数量:max_words,默认200<br>
6、指定词云不显示的单词集合:stop_words={'','',...}；<br>
7、指定词云形状：默认为长方形，需要引用imread()函数 from scipy.misc import imread->mk=imread('pic.png')->w=wordcloud.WordCloud(mask=mk) 其中'pic.png'为指定的词云形状<br>
8、指定词云图片的背景颜色：background_color，默认为黑色，w=wordcloud.WorldCloud(bakcground_color='white')指定为白色<br>




