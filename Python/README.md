<<<<<<< HEAD

# python的安装

首先，从Python的官方网站 www.python.org下载最新的2.7.6版本，地址是这个：

http://www.python.org/ftp/python/2.7.6/python-2.7.6.msi

然后，运行下载的MSI安装包，不需要更改任何默认设置，直接一路点“Next”即可完成安装：

默认会安装到C:\Python27目录下，但是当你兴致勃勃地打开命令提示符窗口，敲入python后，会得到：

    ‘python’不是内部或外部命令，也不是可运行的程序或批处理文件。

这是因为Windows会根据一个Path的环境变量设定的路径去查找python.exe，如果没找到，就会报错。解决办法是把python.exe所在的路径C:\Python27添加到Path中。

现在，再打开一个新的命令行窗口（一定要关掉原来的命令行窗口，再新开一个），输入python： 

你看到提示符>>>就表示我们已经在Python交互式环境中了，可以输入任何Python代码，回车后会立刻得到执行结果。现在，输入exit()并回车，就可以退出Python交互式环境（直接关掉命令行窗口也可以！）。 

# pip安装
1. 在以下地址下载最新的PIP安装文件：http://pypi.python.org/pypi/pip#downloads
2. 下载pip-7.1.2.tar.gz (md5, pgp)完成之后，解压到一个文件夹，用CMD控制台进入解压目录，输入：
    python setup.py install  

安装好之后，我们直接在命令行输入pip，同样会显示‘pip’不是内部命令，也不是可运行的程序。因为我们还没有添加环境变量。
    C:\Python27\Scripts

# 用pip去安装其他python库
    python -m pip install jieta
    pip install jieta 

# join() function
## 对序列进行操作（分别使用' '与':'作为分隔符）
  
    >>> seq1 = ['hello','good','boy','doiido']
    >>> print ' '.join(seq1)
    hello good boy doiido
    >>> print ':'.join(seq1)
    hello:good:boy:doiido
  
## 对字符串进行操作
  
    >>> seq2 = "hello good boy doiido"
    >>> print ':'.join(seq2)
    h:e:l:l:o: :g:o:o:d: :b:o:y: :d:o:i:i:d:o
   
## 对元组进行操作
  
    >>> seq3 = ('hello','good','boy','doiido')
    >>> print ':'.join(seq3)
    hello:good:boy:doiido
  
## 对字典进行操作
  
    >>> seq4 = {'hello':1,'good':2,'boy':3,'doiido':4}
    >>> print ':'.join(seq4)
    boy:good:doiido:hello
  
## 合并目录
  
    >>> import os
    >>> os.path.join('/hello/','good/boy/','doiido')
    '/hello/good/boy/doiido'

## 如果当前电脑里所有的python程序访问网页或者服务器的时候都需要用到代理可以去python库的源文件修改代理，这样所有用到该源文件访问
    例如我在使用tushare访问数据的时候，实际上默认程序是不走代理的，虽然系统中配置了代理，依然不起任何作用，这个时候发现tushare在访问网页/服务器的时候，
    最终调用的是urlib2.py， 而平时在自己写脚本的时候在脚本中使用build一个新的ProxyHandler就可以了，如下：
    # -*- coding: utf-8 -*-
    import urllib
    import urllib2
    import gzip, StringIO
    import zlib
    
    '''
    proxy_handler = urllib2.ProxyHandler({'http': '135.245.48.34:8000'})
    opener = urllib2.build_opener(proxy_handler)
    r = opener.open('http://finance.sina.com.cn/realstock/company/sz300033/nc.shtml')
    print(r.read())
    '''
    
    request = urllib2.Request('http://www.163.com')
    request.add_header('Accept-encoding', 'gzip')
    
    proxy_handler = urllib2.ProxyHandler({'http': '135.245.48.34:8000'})
    opener = urllib2.build_opener(proxy_handler)
    
    response = opener.open(request)
    html = response.read()
    
    gzipped = response.headers.get('Content-Encoding')
    if gzipped:
        html = StringIO.StringIO(html)
        #html = zlib.decompress(html, 16 + zlib.MAX_WBITS)
        gzipper = gzip.GzipFile(fileobj=html)
        html = gzipper.read()
    else:
        typeEncode = sys.getfilesystemencoding()##系统默认编码
        infoencode = chardet.detect(html).get('encoding','utf-8')##通过第3方模块来自动提取网页的编码
        html = content.decode(infoencode,'ignore').encode(typeEncode)##先转换成unicode编码，然后转换系统编码输出
    
    print html
    
    但是在大量应用中很难做到直接去修改所有的库去build proxyhandler， 所以我们可以采用修改基础库urllib2中的ProxyHandler __init__的constext默认参数设置代理
    class ProxyHandler(BaseHandler):
    ...
    def __init__(self, proxies=None):   
    ----> 
    def __init__(self, proxies={'http': 'xxx.xxx.xxx.xxx:xx'}):    
    其中xxx.xxx.xxx.xxx:xx具体的代理，在浏览器的代理中可以查到.

6. [jdBuyMask](https://github.com/JerryBluesnow/jdBuyMask/blob/master/jdBuyMask.py) - github


7. [cursor() — 数据库连接操作 python](https://www.cnblogs.com/qixu/p/6133429.html)

```
python 操作数据库，要安装一个Python和数据库交互的包MySQL-python-1.2.2.win32-py2.5.exe，然后我们就可以使用MySQLdb这个包进行数据库操作了。

     操作步骤如下：
    1、建立数据库连接
     import MySQLdb
     conn=MySQLdb.connect(host="localhost",user="root",passwd="sa",db="mytable")
　   cursor=conn.cursor()
    2、执行数据库操作
     n=cursor.execute(sql,param)
     我们要使用连接对象获得一个cursor对象,接下来,我们会使用cursor提供的方法来进行工作.
     这些方法包括两大类:1.执行命令,2.接收返回值

    3、cursor用来执行命令的方法:

　    callproc(self, procname, args):用来执行存储过程,接收的参数为存储过程名和参数列表,返回值为受影响的行数
　    execute(self, query, args):执行单条sql语句,接收的参数为sql语句本身和使用的参数列表,返回值为受影响的行数
　    executemany(self, query, args):执行单挑sql语句,但是重复执行参数列表里的参数,返回值为受影响的行数
　    nextset(self):移动到下一个结果集

　   4、cursor用来接收返回值的方法:

　    fetchall(self):接收全部的返回结果行.
　    fetchmany(self, size=None):接收size条返回结果行.如果size的值大于返回的结果行的数量,则会返回cursor.arraysize条数据.
　    fetchone(self):返回一条结果行.
　    scroll(self, value, mode='relative'):移动指针到某一行.如果mode='relative',则表示从当前所在行移动value条,如果mode='absolute',则表示从结果集的第一 行移动value条.
    
    5、下面的代码是一个完整的例子.

    #使用sql语句,这里要接收的参数都用%s占位符.要注意的是,无论你要插入的数据是什么类型,占位符永远都要用%s
    sql="insert into cdinfo values(%s,%s,%s,%s,%s)"
    #param应该为tuple或者list
    param=(title,singer,imgurl,url,alpha)
    #执行,如果成功,n的值为1
     n=cursor.execute(sql,param)
    #再来执行一个查询的操作
    cursor.execute("select * from cdinfo")
    #我们使用了fetchall这个方法.这样,cds里保存的将会是查询返回的全部结果.每条结果都是一个tuple类型的数据,这些tuple组成了一个tuple
    cds=cursor.fetchall()
    #因为是tuple,所以可以这样使用结果集
    print cds[0][3]
    #或者直接显示出来,看看结果集的真实样子
    print cds
    #如果需要批量的插入数据,就这样做
     sql="insert into cdinfo values(0,%s,%s,%s,%s,%s)"
    #每个值的集合为一个tuple,整个参数集组成一个tuple,或者list
     param=((title,singer,imgurl,url,alpha),(title2,singer2,imgurl2,url2,alpha2))
    #使用executemany方法来批量的插入数据.这真是一个很酷的方法!
     n=cursor.executemany(sql,param)
     需要注意的是(或者说是我感到奇怪的是),在执行完插入或删除或修改操作后,需要调用一下conn.commit()方法进行提交.这样,数据才会真正保 存在数据库中.我不清楚是否是我的mysql设置问题,总之,今天我在一开始使用的时候,如果不用commit,那数据就不会保留在数据库中,但是,数据 确实在数据库呆过.因为自动编号进行了累积,而且返回的受影响的行数并不为0.

     6、关闭数据库连接

     需要分别的关闭指针对象和连接对象.他们有名字相同的方法
     cursor.close()
     conn.close() 

Django操作数据库
     django是一个出色的用于python的web框架。django连接有操作数据库的api，使用起来十分简洁。我们在settings.py中配置好所要连接的数据库，然后在modules、view、urls中分别写好业务逻辑
```
## python操作mysql数据库的相关操作实例
```
# -*- coding: utf-8 -*-
#python operate mysql database
import MySQLdb
  
#数据库名称
DATABASE_NAME= ''
#host = 'localhost' or '172.0.0.1'
HOST= ''
#端口号
PORT= ''
#用户名称
USER_NAME= ''
#数据库密码
PASSWORD= ''
#数据库编码
CHAR_SET= ''
  
#初始化参数
def init():
    global DATABASE_NAME
    DATABASE_NAME= 'test'
    global HOST
    HOST= 'localhost'
    global PORT
    PORT= '3306'
    global USER_NAME
    USER_NAME= 'root'
    global PASSWORD
    PASSWORD= 'root'
    global CHAR_SET
    CHAR_SET= 'utf8'
      
#获取数据库连接
def get_conn():
    init()
    return MySQLdb.connect(host= HOST, user= USER_NAME, passwd= PASSWORD, db= DATABASE_NAME, charset= CHAR_SET)
  
#获取cursor
def get_cursor(conn):
    return conn.cursor()
  
#关闭连接
def conn_close(conn):
    if conn != None:
        conn.close()
  
#关闭cursor
def cursor_close(cursor):
    if cursor != None:
        cursor.close()
  
#关闭所有
def close(cursor, conn):
    cursor_close(cursor)
    conn_close(conn)
  
#创建表
def create_table():
    sql= '''
    CREATE TABLE `student` (
    `id` int(11) NOT NULL,
    `name` varchar(20) NOT NULL,
    `age` int(11) DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `name` (`name`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    '''
    conn= get_conn()
    cursor= get_cursor(conn)
    result= cursor.execute(sql)
    conn.commit()
    close(cursor, conn)
    return result
  
#查询表信息
def query_table(table_name):
    if table_name != '':
        sql= 'select * from ' + table_name
        conn= get_conn()
        cursor= get_cursor(conn)
        result= cursor.execute(sql)
        for rowin cursor.fetchall():
            print(row)
            #for r in row:      #循环每一条数据
                #print(r)
        close(cursor, conn)
    else:
        print('table name is empty!')
  
#插入数据
def insert_table():
    sql= 'insert into student(id, name, age) values(%s, %s, %s)'
    params= ('1','Hongten_a','21')
    conn= get_conn()
    cursor= get_cursor(conn)
    result= cursor.execute(sql, params)
    conn.commit()
    close(cursor, conn)
    return result
  
#更新数据
def update_table():
    sql= 'update student set name = %s where id = 1'
    params= ('HONGTEN')
    conn= get_conn()
    cursor= get_cursor(conn)
    result= cursor.execute(sql, params)
    conn.commit()
    close(cursor, conn)
    return result
  
#删除数据
def delete_data():
    sql= 'delete from student where id = %s'
    params= ('1')
    conn= get_conn()
    cursor= get_cursor(conn)
    result= cursor.execute(sql, params)
    conn.commit()
    close(cursor, conn)
    return result
  
#数据库连接信息  
def print_info():
    print('数据库连接信息:' + DATABASE_NAME+ HOST+ PORT+ USER_NAME+ PASSWORD+ CHAR_SET)
  
#打印出数据库中表情况
def show_databases():
    sql= 'show databases'
    conn= get_conn()
    cursor= get_cursor(conn)
    result= cursor.execute(sql)
    for rowin cursor.fetchall():
        print(row)
          
#数据库中表情况
def show_tables():
    sql= 'show tables'
    conn= get_conn()
    cursor= get_cursor(conn)
    result= cursor.execute(sql)
    for rowin cursor.fetchall():
        print(row)
  
     
def main():
    show_tables()
    #创建表
    result= create_table()
    print(result)
    #查询表
    query_table('student')
    #插入数据
    print(insert_table())
    print('插入数据后....')
    query_table('student')
    #更新数据
    print(update_table())
    print('更新数据后....')
    query_table('student')
    #删除数据
    delete_data()
    print('删除数据后....')
    query_table('student')
    print_info()
    #数据库中表情况
    show_tables()
      
  
if __name__== '__main__':
    main()
```

## python pip更新镜像源
如果想使用命令行pip命令进行下载：

使用pip的时候在后面加上-i参数，指定pip的下载源

pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple

上面命令每次运行需要指定网址，可进行永久修改：

windows下: 在user目录中创建一个pip目录，如：C:\Users（用户）\xx\pip，新建文件pip.ini，内容如下

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
linux下: 修改 ~/.pip/pip.conf （如果没有自己创建一个）， 内容如下：

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
=======

# python的安装

首先，从Python的官方网站 www.python.org下载最新的2.7.6版本，地址是这个：

http://www.python.org/ftp/python/2.7.6/python-2.7.6.msi

然后，运行下载的MSI安装包，不需要更改任何默认设置，直接一路点“Next”即可完成安装：

默认会安装到C:\Python27目录下，但是当你兴致勃勃地打开命令提示符窗口，敲入python后，会得到：

    ‘python’不是内部或外部命令，也不是可运行的程序或批处理文件。

这是因为Windows会根据一个Path的环境变量设定的路径去查找python.exe，如果没找到，就会报错。解决办法是把python.exe所在的路径C:\Python27添加到Path中。

现在，再打开一个新的命令行窗口（一定要关掉原来的命令行窗口，再新开一个），输入python： 

你看到提示符>>>就表示我们已经在Python交互式环境中了，可以输入任何Python代码，回车后会立刻得到执行结果。现在，输入exit()并回车，就可以退出Python交互式环境（直接关掉命令行窗口也可以！）。 

# pip安装
1. 在以下地址下载最新的PIP安装文件：http://pypi.python.org/pypi/pip#downloads
2. 下载pip-7.1.2.tar.gz (md5, pgp)完成之后，解压到一个文件夹，用CMD控制台进入解压目录，输入：
    python setup.py install  

安装好之后，我们直接在命令行输入pip，同样会显示‘pip’不是内部命令，也不是可运行的程序。因为我们还没有添加环境变量。
    C:\Python27\Scripts

# 用pip去安装其他python库
    python -m pip install jieta
    pip install jieta 

# join() function
## 对序列进行操作（分别使用' '与':'作为分隔符）
  
    >>> seq1 = ['hello','good','boy','doiido']
    >>> print ' '.join(seq1)
    hello good boy doiido
    >>> print ':'.join(seq1)
    hello:good:boy:doiido
  
## 对字符串进行操作
  
    >>> seq2 = "hello good boy doiido"
    >>> print ':'.join(seq2)
    h:e:l:l:o: :g:o:o:d: :b:o:y: :d:o:i:i:d:o
   
## 对元组进行操作
  
    >>> seq3 = ('hello','good','boy','doiido')
    >>> print ':'.join(seq3)
    hello:good:boy:doiido
  
## 对字典进行操作
  
    >>> seq4 = {'hello':1,'good':2,'boy':3,'doiido':4}
    >>> print ':'.join(seq4)
    boy:good:doiido:hello
  
## 合并目录
  
    >>> import os
    >>> os.path.join('/hello/','good/boy/','doiido')
    '/hello/good/boy/doiido'

## 如果当前电脑里所有的python程序访问网页或者服务器的时候都需要用到代理可以去python库的源文件修改代理，这样所有用到该源文件访问
    例如我在使用tushare访问数据的时候，实际上默认程序是不走代理的，虽然系统中配置了代理，依然不起任何作用，这个时候发现tushare在访问网页/服务器的时候，
    最终调用的是urlib2.py， 而平时在自己写脚本的时候在脚本中使用build一个新的ProxyHandler就可以了，如下：
    # -*- coding: utf-8 -*-
    import urllib
    import urllib2
    import gzip, StringIO
    import zlib
    
    '''
    proxy_handler = urllib2.ProxyHandler({'http': '135.245.48.34:8000'})
    opener = urllib2.build_opener(proxy_handler)
    r = opener.open('http://finance.sina.com.cn/realstock/company/sz300033/nc.shtml')
    print(r.read())
    '''
    
    request = urllib2.Request('http://www.163.com')
    request.add_header('Accept-encoding', 'gzip')
    
    proxy_handler = urllib2.ProxyHandler({'http': '135.245.48.34:8000'})
    opener = urllib2.build_opener(proxy_handler)
    
    response = opener.open(request)
    html = response.read()
    
    gzipped = response.headers.get('Content-Encoding')
    if gzipped:
        html = StringIO.StringIO(html)
        #html = zlib.decompress(html, 16 + zlib.MAX_WBITS)
        gzipper = gzip.GzipFile(fileobj=html)
        html = gzipper.read()
    else:
        typeEncode = sys.getfilesystemencoding()##系统默认编码
        infoencode = chardet.detect(html).get('encoding','utf-8')##通过第3方模块来自动提取网页的编码
        html = content.decode(infoencode,'ignore').encode(typeEncode)##先转换成unicode编码，然后转换系统编码输出
    
    print html
    
    但是在大量应用中很难做到直接去修改所有的库去build proxyhandler， 所以我们可以采用修改基础库urllib2中的ProxyHandler __init__的constext默认参数设置代理
    class ProxyHandler(BaseHandler):
    ...
    def __init__(self, proxies=None):   
    ----> 
    def __init__(self, proxies={'http': 'xxx.xxx.xxx.xxx:xx'}):    
    其中xxx.xxx.xxx.xxx:xx具体的代理，在浏览器的代理中可以查到.

6. [jdBuyMask](https://github.com/JerryBluesnow/jdBuyMask/blob/master/jdBuyMask.py) - github


## python pip更新镜像源
如果想使用命令行pip命令进行下载：

使用pip的时候在后面加上-i参数，指定pip的下载源

 pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
1
上面命令每次运行需要指定网址，可进行永久修改：

windows下: 在user目录中创建一个pip目录，如：C:\Users（用户）\xx\pip，新建文件pip.ini，内容如下

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
1
2
linux下: 修改 ~/.pip/pip.conf （如果没有自己创建一个）， 内容如下：

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

## 安装包制作
```
https://www.cnblogs.com/wswind/p/11856039.html

NSIS, Inno

```
- [使用开源安装包制作工具Inno Setup制作软件安装包](https://blog.csdn.net/wangzhichunnihao/article/details/108637473)
- [Inno SetUp Offical](https://jrsoftware.org/isdl.php#stable)
>>>>>>> aa11997 (update from personal pc)
