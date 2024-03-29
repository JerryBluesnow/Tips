# DB Tips

+ [mysql windows重置密码](https://www.cnblogs.com/lyq159/p/12051059.html)
+ [mysql windows重置密码2](https://www.cnblogs.com/wangbaobao/p/7087032.html)
+ [mysql命令大全](https://blog.csdn.net/qq_35409127/article/details/79760797)
<details>
<summary>MYSQL 命令行大全 (简洁、明了、全面)</summary>
    
    MYSQL常用命令  
    1.导出整个数据库  
        mysqldump -u 用户名 -p –default-character-set=latin1 数据库名 > 导出的文件名(数据库默认编码是latin1)  
        mysqldump -u wcnc -p smgp_apps_wcnc > wcnc.sql  
    2.导出一个表  
        mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名  
        mysqldump -u wcnc -p smgp_apps_wcnc users> wcnc_users.sql  
    3.导出一个数据库结构  
        mysqldump -u wcnc -p -d –add-drop-table smgp_apps_wcnc >d:wcnc_db.sql  
        -d 没有数据 –add-drop-table 在每个create语句之前增加一个drop table  
    4.导入数据库  
    A:常用source 命令  
        进入mysql数据库控制台，  
        如mysql -u root -p  
        mysql>use 数据库  
        然后使用source命令，后面参数为脚本文件(如这里用到的.sql)  
        mysql>source wcnc_db.sql  
    B:使用mysqldump命令  
        mysqldump -u username -p dbname > filename.sql  
    C:使用mysql命令  
        mysql -u username -p -D dbname < filename.sql  
    一、启动与退出  
        1、进入MySQL：启动MySQL Command Line Client（MySQL的DOS界面），直接输入安装时的密码即可。此时的提示符是：mysql>  
        2、退出MySQL：quit或exit  
    二、库操作  
    1、、创建数据库  
        命令：create database <数据库名>  
        例如：建立一个名为xhkdb的数据库  
        mysql> create database xhkdb;  
    2、显示所有的数据库  
        命令：show databases （注意：最后有个s）  
        mysql> show databases;  
    3、删除数据库  
        命令：drop database <数据库名>  
        例如：删除名为 xhkdb的数据库  
        mysql> drop database xhkdb;  
    4、连接数据库  
        命令：use <数据库名>  
        例如：如果xhkdb数据库存在，尝试存取它：  
        mysql> use xhkdb;  
        屏幕提示：Database changed  
    5、查看当前使用的数据库  
        mysql> select database();  
    6、当前数据库包含的表信息：  
        mysql> show tables; （注意：最后有个s）  
    三、表操作，操作之前应连接某个数据库  
        1、建表  
        命令：create table <表名> ( <字段名> <类型> [,..<字段名n> <类型n>]);  
        mysql> create table MyClass(  
        > id int(4) not null primary key auto_increment,  
        > name char(20) not null,  
        > sex int(4) not null default ’′,  
        > degree double(16,2));  
    2、获取表结构  
        命令：desc 表名，或者show columns from 表名  
        mysql>DESCRIBE MyClass  
        mysql> desc MyClass;  
        mysql> show columns from MyClass;  
    3、删除表  
        命令：drop table <表名>  
        例如：删除表名为 MyClass 的表  
        mysql> drop table MyClass;  
    4、插入数据  
        命令：insert into <表名> [( <字段名>[,..<字段名n > ])] values ( 值 )[, ( 值n )]  
        例如，往表 MyClass中插入二条记录, 这二条记录表示：编号为的名为Tom的成绩为.45, 编号为 的名为Joan 的成绩为.99，编号为 的名为  Wang  的成绩为.5.  
        mysql> insert into MyClass values(1,’Tom’,96.45),(2,’Joan’,82.99), (2,’Wang’, 96.59);  
    5、查询表中的数据  
    1)、查询所有行  
        命令：select <字段，字段，…> from < 表名 > where < 表达式 >  
        例如：查看表 MyClass 中所有数据  
        mysql> select  from MyClass;  
    2）、查询前几行数据  
        例如：查看表 MyClass 中前行数据  
        mysql> select  from MyClass order by id limit 0,2;  
        或者：  
        mysql> select  from MyClass limit 0,2;  
    6、删除表中数据  
        命令：delete from 表名 where 表达式  
        例如：删除表 MyClass中编号为 的记录  
        mysql> delete from MyClass where id=1;  
    7、修改表中数据：update 表名 set 字段=新值,…where 条件  
        mysql> update MyClass set name=’Mary’where id=1;  
    7、在表中增加字段：  
        命令：alter table 表名 add字段 类型 其他;  
        例如：在表MyClass中添加了一个字段passtest，类型为int(4)，默认值为  
        mysql> alter table MyClass add passtest int(4) default ’′  
    8、更改表名：  
        命令：rename table 原表名 to 新表名;  
        例如：在表MyClass名字更改为YouClass  
        mysql> rename table MyClass to YouClass;  
        更新字段内容  
        update 表名 set 字段名 = 新内容  
        update 表名 set 字段名 = replace(字段名,’旧内容’,’新内容’)  
    
        update article set content=concat(‘　　’,content);  
    字段类型  
    1．INT[(M)] 型：正常大小整数类型  
    2．DOUBLE[(M,D)] [ZEROFILL] 型：正常大小(双精密)浮点数字类型  
    3．DATE 日期类型：支持的范围是-01-01到-12-31。MySQL以YYYY-MM-DD格式来显示DATE值，但是允许你使用字符串或数字把值赋给DATE列  
    4．CHAR(M) 型：定长字符串类型，当存储时，总是是用空格填满右边到指定的长度  
    5．BLOB TEXT类型，最大长度为(2^16-1)个字符。  
    6．VARCHAR型：变长字符串类型  
    5.导入数据库表  
    　　（）创建.sql文件  
    　　（）先产生一个库如auction.c:mysqlbin>mysqladmin -u root -p creat auction，会提示输入密码，然后成功创建。  
    　　（）导入auction.sql文件  
    　　c:mysqlbin>mysql -u root -p auction < auction.sql。  
    　　通过以上操作，就可以创建了一个数据库auction以及其中的一个表auction。  
    　　6．修改数据库  
    　　（）在mysql的表中增加字段：  
    　　alter table dbname add column userid int(11) not null primary key auto_increment;  
    　　这样，就在表dbname中添加了一个字段userid，类型为int(11)。  
    　　7．mysql数据库的授权  
    　　mysql>grant select,insert,delete,create,drop  
    　　on . (或test./user./..)  
    　　to 用户名@localhost  
    　　identified by ‘密码’；  
    　　如：新建一个用户帐号以便可以访问数据库，需要进行如下操作：  
    　　mysql> grant usage  
    　　-> ON test.  
    　　-> TO testuser@localhost;  
    　　Query OK, 0 rows affected (0.15 sec)  
    　　此后就创建了一个新用户叫：testuser，这个用户只能从localhost连接到数据库并可以连接到test 数据库。下一步，我们必须指定testuser    这个用户可以执行哪些操作：  
    　　mysql> GRANT select, insert, delete,update  
    　　-> ON test.  
    　　-> TO testuser@localhost;  
    　　Query OK, 0 rows affected (0.00 sec)  
    　　此操作使testuser能够在每一个test数据库中的表执行SELECT，INSERT和DELETE以及UPDATE查询操作。现在我们结束操作并退出MySQL客户程 序：  
    　　mysql> exit  
    　　Bye9!  
    1:使用SHOW语句找出在服务器上当前存在什么数据库：  
    mysql> SHOW DATABASES;  
    2:2、创建一个数据库MYSQLDATA  
    mysql> Create DATABASE MYSQLDATA;  
    3:选择你所创建的数据库  
    mysql> USE MYSQLDATA; (按回车键出现Database changed 时说明操作成功！)  
    4:查看现在的数据库中存在什么表  
    mysql> SHOW TABLES;  
    5:创建一个数据库表  
    mysql> Create TABLE MYTABLE (name VARCHAR(20), sex CHAR(1));  
    6:显示表的结构：  
    mysql> DESCRIBE MYTABLE;  
    7:往表中加入记录  
    mysql> insert into MYTABLE values (“hyq”,”M”);  
    8:用文本方式将数据装入数据库表中（例如D:/mysql.txt）  
    mysql> LOAD DATA LOCAL INFILE “D:/mysql.txt”INTO TABLE MYTABLE;  
    9:导入.sql文件命令（例如D:/mysql.sql）  
    mysql>use database;  
    mysql>source d:/mysql.sql;  
    10:删除表  
    mysql>drop TABLE MYTABLE;  
    11:清空表  
    mysql>delete from MYTABLE;  
    12:更新表中数据  
    mysql>update MYTABLE set sex=”f”where name=’hyq’;  
    以下是无意中在网络看到的使用MySql的管理心得,  
    
    
    在windows中MySql以服务形式存在，在使用前应确保此服务已经启动，未启动可用net start mysql命令启动。而Linux中启动时可用“/etc/rc.d/ init.d/mysqld start”命令，注意启动者应具有管理员权限。  
    刚安装好的MySql包含一个含空密码的root帐户和一个匿名帐户，这是很大的安全隐患，对于一些重要的应用我们应将安全性尽可能提高，在这里应把 匿名帐户删除、root帐户设置密码，可用如下命令进行：  
    use mysql;  
    delete from User where User=””;  
    update User set Password=PASSWORD(‘newpassword’) where User=’root’;  
    如果要对用户所用的登录终端进行限制，可以更新User表中相应用户的Host字段，在进行了以上更改后应重新启动数据库服务，此时登录时可用如下  类似命令：  
    mysql -uroot -p;  
    mysql -uroot -pnewpassword;  
    mysql mydb -uroot -p;  
    mysql mydb -uroot -pnewpassword;  
    上面命令参数是常用参数的一部分，详细情况可参考文档。此处的mydb是要登录的数据库的名称。  
    在进行开发和实际应用中，用户不应该只用root用户进行连接数据库，虽然使用root用户进行测试时很方便，但会给系统带来重大安全隐患，也不利  于管理技术的提高。我们给一个应用中使用的用户赋予最恰当的数据库权限。如一个只进行数据插入的用户不应赋予其删除数据的权限。MySql的用户   管理是通过User表来实现的，添加新用户常用的方法有两个，一是在User表插入相应的数据行，同时设置相应的权限；二是通过GRANT命令创建具有  某种权限的用户。其中GRANT的常用用法如下：  
    grant all on mydb. to NewUserName@HostName identified by “password”;  
    grant usage on . to NewUserName@HostName identified by “password”;  
    grant select,insert,update on mydb. to NewUserName@HostName identified by “password”;  
    grant update,delete on mydb.TestTable to NewUserName@HostName identified by “password”;  
    若要给此用户赋予他在相应对象上的权限的管理能力，可在GRANT后面添加WITH GRANT OPTION选项。而对于用插入User表添加的用户，Password字    段应用PASSWORD 函数进行更新加密，以防不轨之人窃看密码。对于那些已经不用的用户应给予清除，权限过界的用户应及时回收权限，回收权限可以 通过更新User表相应字段，也可以使用REVOKE操作。  
    下面给出本人从其它资料(www.cn-java.com)获得的对常用权限的解释：  
    全局管理权限：  
    FILE: 在MySQL服务器上读写文件。  
    PROCESS: 显示或杀死属于其它用户的服务线程。  
    RELOAD: 重载访问控制表，刷新日志等。  
    SHUTDOWN: 关闭MySQL服务。  
    数据库/数据表/数据列权限：  
    Alter: 修改已存在的数据表(例如增加/删除列)和索引。  
    Create: 建立新的数据库或数据表。  
    Delete: 删除表的记录。  
    Drop: 删除数据表或数据库。  
    INDEX: 建立或删除索引。  
    Insert: 增加表的记录。  
    Select: 显示/搜索表的记录。  
    Update: 修改表中已存在的记录。  
    特别的权限：  
    ALL: 允许做任何事(和root一样)。  
    USAGE: 只允许登录–其它什么也不允许做。  
    ———————  
    MYSQL常用命令  
    有很多朋友虽然安装好了mysql但却不知如何使用它。在这篇文章中我们就从连接MYSQL、修改密码、增加用户等方面来学习一些MYSQL的常用命   令。  
    　　有很多朋友虽然安装好了mysql但却不知如何使用它。在这篇文章中我们就从连接MYSQL、修改密码、增加用户等方面来学习一些MYSQL的常用命   令。　  
    　　一、连接MYSQL　  
    　　格式：mysql -h主机地址-u用户名－p用户密码　　  
    　　、例：连接到本机上的MYSQL  
    　　首先在打开DOS窗口，然后进入目录mysqlbin，再键入命令mysql -uroot -p，回车后提示你输密码，如果刚安装好MYSQL，超级用户root是没 有密码的，故直接回车即可进入到MYSQL中了，MYSQL的提示符是：mysql> 　　  
    　　、例：连接到远程主机上的MYSQL  
    　　假设远程主机的IP为：.110.110.110，用户名为root,密码为abcd123。则键入以下命令：　　　  
    　　mysql -h110.110.110.110 -uroot -pabcd123 　　  
    　　（注:u与root可以不用加空格，其它也一样）　　  
    　　、退出MYSQL命令：exit （回车）  
    　　二、修改密码　　  
    　　格式：mysqladmin -u用户名-p旧密码password 新密码　  
    　　、例：给root加个密码ab12。首先在DOS下进入目录mysqlbin，然后键入以下命令　　  
    　　mysqladmin -uroot -password ab12 　　  
    　　注：因为开始时root没有密码，所以-p旧密码一项就可以省略了。　　  
    　　、例：再将root的密码改为djg345  
    　　mysqladmin -uroot -pab12 password djg345  
    MYSQL常用命令（下）  
    　　一、操作技巧  
    　　、如果你打命令时，回车后发现忘记加分号，你无须重打一遍命令，只要打个分号回车就可以了。也就是说你可以把一个完整的命令分成几行来  打，完后用分号作结束标志就OK。  
    　　、你可以使用光标上下键调出以前的命令。但以前我用过的一个MYSQL旧版本不支持。我现在用的是mysql-3.23.27-beta-win。  
    　　二、显示命令  
    　　、显示数据库列表。  
    　　show databases;  
    　　刚开始时才两个数据库：mysql和test。mysql库很重要它里面有MYSQL的系统信息，我们改密码和新增用户，实际上就是用这个库进行操作。  
    　　、显示库中的数据表：  
    　　use mysql；／／打开库，学过FOXBASE的一定不会陌生吧  
    　　show tables;  
    　　、显示数据表的结构：  
    　　describe 表名;  
    　　、建库：  
    　　create database 库名;  
    　　、建表：  
    　　use 库名；  
    　　create table 表名(字段设定列表)；  
    　　、删库和删表:  
    　　drop database 库名;  
    　　drop table 表名；  
    　　、将表中记录清空：  
    　　delete from 表名;  
    　　、显示表中的记录：  
    　　select  from 表名;  
    三、一个建库和建表以及插入数据的实例  
    　　drop database if exists school; //如果存在SCHOOL则删除  
    　　create database school; //建立库SCHOOL  
    　　use school; //打开库SCHOOL  
    　　create table teacher //建立表TEACHER  
    　　(  
    　　id int(3) auto_increment not null primary key,  
    　　name char(10) not null,  
    　　address varchar(50) default ‘深圳’,  
    　　year date  
    　　); //建表结束  
    　　//以下为插入字段  
    　　insert into teacher values(”,’glchengang’,’深圳一中’,’-10-10′);  
    　　insert into teacher values(”,’jack’,’深圳一中’,’-12-23′);  
    　　注：在建表中（）将ID设为长度为的数字字段:int(3)并让它每个记录自动加一:auto_increment并不能为空:not null而且让他成为主字段   primary key  
    　　（）将NAME设为长度为的字符字段  
    　　（）将ADDRESS设为长度的字符字段，而且缺省值为深圳。varchar和char有什么区别呢，只有等以后的文章再说了。  
    　　（）将YEAR设为日期字段。  
    　　如果你在mysql提示符键入上面的命令也可以，但不方便调试。你可以将以上命令原样写入一个文本文件中假设为school.sql，然后复制到c:\    下，并在DOS状态进入目录\mysql\bin，然后键入以下命令：  
    　　mysql -uroot -p密码< c:\school.sql  
    　　如果成功，空出一行无任何显示；如有错误，会有提示。（以上命令已经调试，你只要将//的注释去掉即可使用）。  
    四、将文本数据转到数据库中  
    　　、文本数据应符合的格式：字段数据之间用tab键隔开，null值用\n来代替.  
    　　例：  
    　　rose 深圳二中1976-10-10  
    　　mike 深圳一中1975-12-23  
    　　、数据传入命令load data local infile “文件名” into table 表名;  
    　　注意：你最好将文件复制到\mysql\bin目录下，并且要先用use命令打表所在的库。  
    五、备份数据库：（命令在DOS的\mysql\bin目录下执行）  
    　　mysqldump –opt school>school.bbb  
    　　注释:将数据库school备份到school.bbb文件，school.bbb是一个文本文件，文件名任取，打开看看你会有新发现。  
    一.SELECT语句的完整语法为：  
    SELECT[ALL|DISTINCT|DISTINCTROW|TOP]  
    {|talbe.|[table.]field1[AS alias1][,[table.]field2[AS alias2][,…]]}  
    FROM tableexpression[,…][IN externaldatabase]  
    [WHERE…]  
    [GROUP BY…]  
    [HAVING…]  
    [ORDER BY…]  
    [WITH OWNERACCESS OPTION]  
    说明：  
    用中括号([])括起来的部分表示是可选的，用大括号({})括起来的部分是表示必须从中选择其中的一个。  
    1 FROM子句  
    FROM 子句指定了SELECT语句中字段的来源。FROM子句后面是包含一个或多个的表达式(由逗号分开)，其中的表达式可为单一表名称、已保存的查询   或由INNER JOIN、LEFT JOIN 或RIGHT JOIN 得到的复合结果。如果表或查询存储在外部数据库，在IN 子句之后指明其完整路径。  
    例：下列SQL语句返回所有有定单的客户：  
    SELECT OrderID,Customer.customerID  
    FROM Orders Customers  
    WHERE Orders.CustomerID=Customers.CustomeersID  
    2 ALL、DISTINCT、DISTINCTROW、TOP谓词  
    (1) ALL 返回满足SQL语句条件的所有记录。如果没有指明这个谓词，默认为ALL。  
    例：SELECT ALL FirstName,LastName  
    FROM Employees  
    (2) DISTINCT 如果有多个记录的选择字段的数据相同，只返回一个。  
    (3) DISTINCTROW 如果有重复的记录，只返回一个  
    (4) TOP显示查询头尾若干记录。也可返回记录的百分比，这是要用TOP N PERCENT子句（其中N 表示百分比）  
    例：返回%定货额最大的定单  
    SELECT TOP 5 PERCENT*  
    FROM [ Order Details]  
    ORDER BY UnitPrice*Quantity*(1-Discount) DESC  
    3 用AS 子句为字段取别名  
    如果想为返回的列取一个新的标题，或者，经过对字段的计算或总结之后，产生了一个新的值，希望把它放到一个新的列里显示，则用AS保留。  
    例：返回FirstName字段取别名为NickName  
    SELECT FirstName AS NickName ,LastName ,City  
    FROM Employees  
    例：返回新的一列显示库存价值  
    SELECT ProductName ,UnitPrice ,UnitsInStock ,UnitPrice*UnitsInStock AS valueInStock  
    FROM Products  
    二.WHERE 子句指定查询条件  
    1 比较运算符  
    比较运算符含义  
    = 等于  
    > 大于  
    < 小于  
    >= 大于等于  
    <= 小于等于  
    <> 不等于  
    !> 不大于  
    !< 不小于  
    例：返回年月的定单  
    SELECT OrderID, CustomerID, OrderDate  
    FROM Orders  
    WHERE OrderDate>#1/1/96# AND OrderDate<#1/30/96#  
    注意：  
    Mcirosoft JET SQL 中，日期用‘#’定界。日期也可以用Datevalue()函数来代替。在比较字符型的数据时，要加上单引号’’，尾空格在比较中被忽    略。  
    例：  
    WHERE OrderDate>#96-1-1#  
    也可以表示为：  
    WHERE OrderDate>Datevalue(‘/1/96’)  
    使用NOT 表达式求反。  
    例：查看年月日以后的定单  
    WHERE Not OrderDate<=#1/1/96#  
    2 范围（BETWEEN 和NOT BETWEEN）  
    BETWEEN …AND…运算符指定了要搜索的一个闭区间。  
    例：返回年月到年月的定单。  
    WHERE OrderDate Between #1/1/96# And #2/1/96#  
    3 列表（IN ，NOT IN）  
    IN 运算符用来匹配列表中的任何一个值。IN子句可以代替用OR子句连接的一连串的条件。  
    例：要找出住在London、Paris或Berlin的所有客户  
    SELECT CustomerID, CompanyName, ContactName, City  
    FROM Customers  
    WHERE City In(‘London’,’Paris’,’Berlin’)  
    4 模式匹配(LIKE)  
    LIKE运算符检验一个包含字符串数据的字段值是否匹配一指定模式。  
    LIKE运算符里使用的通配符  
    通配符含义  
    ？任何一个单一的字符  
     任意长度的字符  
    # 0~9之间的单一数字  
    [字符列表] 在字符列表里的任一值  
    [！字符列表] 不在字符列表里的任一值  
    - 指定字符范围，两边的值分别为其上下限  
    例：返回邮政编码在（）-0000到（）-9999之间的客户  
    SELECT CustomerID ,CompanyName,City,Phone  
    FROM Customers  
    WHERE Phone Like ‘(171)555-####’  
    LIKE运算符的一些样式及含义  
    样式含义不符合  
    LIKE ‘A’A后跟任意长度的字符Bc,c255  
    LIKE’[]’5*5 555  
    LIKE’?5’5与之间有任意一个字符55,5wer5  
    LIKE’##5’5235，5kd5,5346  
    LIKE’[a-z]’a-z间的任意一个字符5,%  
    LIKE’[!0-9]’非-9间的任意一个字符0,1  
    LIKE’[[]’1,  
    三.用ORDER BY子句排序结果  
    ORDER子句按一个或多个（最多个）字段排序查询结果，可以是升序（ASC）也可以是降序（DESC），缺省是升序。ORDER子句通常放在SQL语句的最    后。  
    ORDER子句中定义了多个字段，则按照字段的先后顺序排序。  
    例：  
    SELECT ProductName,UnitPrice, UnitInStock  
    FROM Products  
    ORDER BY UnitInStock DESC , UnitPrice DESC, ProductName  
    ORDER BY 子句中可以用字段在选择列表中的位置号代替字段名，可以混合字段名和位置号。  
    例：下面的语句产生与上列相同的效果。  
    SELECT ProductName,UnitPrice, UnitInStock  
    FROM Products  
    ORDER BY 1 DESC , 2 DESC,3  
    四.运用连接关系实现多表查询  
    例：找出同一个城市中供应商和客户的名字  
    SELECT Customers.CompanyName, Suppliers.ComPany.Name  
    FROM Customers, Suppliers  
    WHERE Customers.City=Suppliers.City  
    例：找出产品库存量大于同一种产品的定单的数量的产品和定单  
    SELECT ProductName,OrderID, UnitInStock, Quantity  
    FROM Products, [Order Deails]  
    WHERE Product.productID=[Order Details].ProductID  
    AND UnitsInStock>Quantity  
    另一种方法是用Microsof JET SQL 独有的JNNER JOIN  
    语法：  
    FROM table1 INNER JOIN table2  
    ON table1.field1 comparision table2.field2  
    其中comparision 就是前面WHERE子句用到的比较运算符。  
    SELECT FirstName,lastName,OrderID,CustomerID,OrderDate  
    FROM Employees  
    INNER JOIN Orders ON Employees.EmployeeID=Orders.EmployeeID  
    注意：  
    INNER JOIN不能连接Memo OLE Object Single Double 数据类型字段。  
    在一个JOIN语句中连接多个ON子句  
    语法：  
    SELECT fields  
    FROM table1 INNER JOIN table2  
    ON table1.field1 compopr table2.field1 AND  
    ON table1.field2 compopr table2.field2 OR  
    ON table1.field3 compopr table2.field3  
    也可以  
    SELECT fields  
    FROM table1 INNER JOIN  
    （table2 INNER JOIN [( ]table3  
    [INNER JOER] [( ]tablex[INNER JOIN]  
    ON table1.field1 compopr table2.field1  
    ON table1.field2 compopr table2.field2  
    ON table1.field3 compopr table2.field3  
    外部连接返回更多记录，在结果中保留不匹配的记录，不管存不存在满足条件的记录都要返回另一侧的所有记录。  
    FROM table [LEFT|RIGHT]JOIN table2  
    ON table1.field1comparision table.field2  
    用左连接来建立外部连接，在表达式的左边的表会显示其所有的数据  
    例：不管有没有定货量，返回所有商品  
    SELECT ProductName ,OrderID  
    FROM Products  
    LEFT JOIN Orders ON Products.PrductsID=Orders.ProductID  
    右连接与左连接的差别在于：不管左侧表里有没有匹配的记录，它都从左侧表中返回所有记录。  
    例：如果想了解客户的信息，并统计各个地区的客户分布，这时可以用一个右连接，即使某个地区没有客户，也要返回客户信息。  
    空值不会相互匹配，可以通过外连接才能测试被连接的某个表的字段是否有空值。  
    SELECT   
    FROM talbe1  
    LEFT JOIN table2 ON table1.a=table2.c  
    1 连接查询中使用Iif函数实现以值显示空值  
    Iif表达式：Iif(IsNull(Amount,0,Amout)  
    例：无论定货大于或小于￥，都要返回一个标志。  
    Iif([Amount]>50,?Big order?,?Small order?)  
    五. 分组和总结查询结果  
    在SQL的语法里，GROUP BY和HAVING子句用来对数据进行汇总。GROUP BY子句指明了按照哪几个字段来分组，而将记录分组后，用HAVING子句过滤 这些记录。  
    GROUP BY 子句的语法  
    SELECT fidldlist  
    FROM table  
    WHERE criteria  
    [GROUP BY groupfieldlist [HAVING groupcriteria]]  
    注：Microsoft Jet数据库Jet 不能对备注或OLE对象字段分组。  
    GROUP BY字段中的Null值以备分组但是不能被省略。  
    在任何SQL合计函数中不计算Null值。  
    GROUP BY子句后最多可以带有十个字段，排序优先级按从左到右的顺序排列。  
    例：在‘WA’地区的雇员表中按头衔分组后，找出具有同等头衔的雇员数目大于人的所有头衔。  
    SELECT Title ,Count(Title) as Total  
    FROM Employees  
    WHERE Region = ‘WA’  
    GROUP BY Title  
    HAVING Count(Title)>1  
    JET SQL 中的聚积函数  
    聚集函数意义  
    SUM ( ) 求和  
    AVG ( ) 平均值  
    COUNT ( ) 表达式中记录的数目  
    COUNT ( ) 计算记录的数目  
    MAX 最大值  
    MIN 最小值  
    VAR 方差  
    STDEV 标准误差  
    FIRST 第一个值  
    LAST 最后一个值  
    六. 用Parameters声明创建参数查询  
    Parameters声明的语法:  
    PARAMETERS name datatype[,name datatype[, …]]  
    其中name 是参数的标志符,可以通过标志符引用参数.  
    Datatype说明参数的数据类型.  
    使用时要把PARAMETERS 声明置于任何其他语句之前.  
    例:  
    PARAMETERS[Low price] Currency,[Beginning date]datatime  
    SELECT OrderID ,OrderAmount  
    FROM Orders  
    WHERE OrderAMount>[low price]  
    AND OrderDate>=[Beginning date]  
    七. 功能查询  
    所谓功能查询,实际上是一种操作查询,它可以对数据库进行快速高效的操作.它以选择查询为目的,挑选出符合条件的数据,再对数据进行批处理.功能  查询包括更新查询,删除查询,添加查询,和生成表查询.  
    1 更新查询  
    UPDATE子句可以同时更改一个或多个表中的数据.它也可以同时更改多个字段的值.  
    更新查询语法:  
    UPDATE 表名  
    SET 新值  
    WHERE 准则  
    例:英国客户的定货量增加%,货运量增加%  
    UPDATE OEDERS  
    SET OrderAmount = OrderAmount 1.1  
    Freight = Freight*1.03  
    WHERE ShipCountry = ‘UK’  
    2 删除查询  
    DELETE子句可以使用户删除大量的过时的或冗于的数据.  
    注:删除查询的对象是整个记录.  
    DELETE子句的语法:  
    DELETE [表名.]  
    FROM 来源表  
    WHERE 准则  
    例: 要删除所有年前的定单  
    DELETE   
    FROM Orders  
    WHERE OrderData<#94-1-1#  
    3 追加查询  
    INSERT子句可以将一个或一组记录追加到一个或多个表的尾部.  
    INTO 子句指定接受新记录的表  
    valueS 关键字指定新记录所包含的数据值.  
    INSERT 子句的语法:  
    INSETR INTO 目的表或查询(字段,字段,…)  
    valueS(数值,数值,…)  
    例:增加一个客户  
    INSERT INTO Employees(FirstName,LastName,title)  
    valueS(‘Harry’,’Washington’,’Trainee’)  
    4 生成表查询  
    可以一次性地把所有满足条件的记录拷贝到一张新表中.通常制作记录的备份或副本或作为报表的基础.  
    SELECT INTO子句用来创建生成表查询语法:  
    SELECT 字段,字段,…  
    INTO 新表[IN 外部数据库]  
    FROM 来源数据库  
    WHERE 准则  
    例:为定单制作一个存档备份  
    SELECT   
    INTO OrdersArchive  
    FROM Orders  
    八. 联合查询  
    UNION运算可以把多个查询的结果合并到一个结果集里显示.  
    UNION运算的一般语法:  
    [表]查询UNION [ALL]查询UNION …  
    例:返回巴西所有供给商和客户的名字和城市  
    SELECT CompanyName,City  
    FROM Suppliers  
    WHERE Country = ‘Brazil’  
    UNION  
    SELECT CompanyName,City  
    FROM Customers  
    WHERE Country = ‘Brazil’  
    注:  
    缺省的情况下,UNION子句不返回重复的记录.如果想显示所有记录,可以加ALL选项  
    UNION运算要求查询具有相同数目的字段.但是,字段数据类型不必相同.  
    每一个查询参数中可以使用GROUP BY 子句或HAVING 子句进行分组.要想以指定的顺序来显示返回的数据,可以在最后一个查询的尾部使用OREER BY    子句.  
    九. 交叉查询  
    交叉查询可以对数据进行总和,平均,计数或其他总和计算法的计算,这些数据通过两种信息进行分组:一个显示在表的左部,另一个显示在表的顶部.  
    Microsoft Jet SQL 用TRANSFROM语句创建交叉表查询语法:  
    TRANSFORM aggfunction  
    SELECT 语句  
    GROUP BY 子句  
    PIVOT pivotfield[IN(value1 [,value2[,…]]) ]  
    Aggfounction指SQL聚积函数,  
    SELECT语句选择作为标题的的字段,  
    GROUP BY 分组  
    说明：  
    Pivotfield 在查询结果集中创建列标题时用的字段或表达式,用可选的IN子句限制它的取值.  
    value代表创建列标题的固定值.  
    例:显示在年里每一季度每一位员工所接的定单的数目:  
    TRANSFORM Count(OrderID)  
    SELECT FirstName&’’&LastName AS FullName  
    FROM Employees INNER JOIN Orders  
    ON Employees.EmployeeID = Orders.EmployeeID  
    WHERE DatePart(“yyyy”,OrderDate)= ‘’  
    GROUP BY FirstName&’’&LastName  
    ORDER BY FirstName&’’&LastName  
    POVOT DatePart(“q”,OrderDate)&’季度’  
    十.子查询  
    子查询可以理解为套查询.子查询是一个SELECT语句.  
    1 表达式的值与子查询返回的单一值做比较  
    语法:  
    表达式comparision ANY|ALL|SOME  
    说明：  
    ANY 和SOME谓词是同义词,与比较运算符(=,<,>,<>,<=,>=)一起使用.返回一个布尔值True或False.ANY的意思是,表达式与子查询返回的一系列的  值逐一比较,只要其中的一次比较产生True结果,ANY测试的返回True值(既WHERE子句的结果),对应于该表达式的当前记录将进入主查询的结果中.ALL 测试则要求表达式与子查询返回的一系列的值的比较都产生True结果,才回返回True值.  
    例:主查询返回单价比任何一个折扣大于等于%的产品的单价要高的所有产品  
    SELECT  FROM Products  
    WHERE UnitPrice>ANY  
    (SELECT UnitPrice FROM[Order Details] WHERE Discount>0.25)  
    2 检查表达式的值是否匹配子查询返回的一组值的某个值  
    语法:  
    [NOT]IN(子查询)  
    例:返回库存价值大于等于的产品.  
    SELECT ProductName FROM Products  
    WHERE ProductID IN  
    (SELECT PrdoctID FROM [Order DEtails]  
    WHERE UnitPrice*Quantity>= 1000)  
    3检测子查询是否返回任何记录  
    语法:  
    [NOT]EXISTS (子查询)  
    例:用EXISTS检索英国的客户  
    SELECT ComPanyName,ContactName  
    FROM Orders  
    WHERE EXISTS  
    (SELECT   
    FROM Customers  
    WHERE Country = ‘UK’AND  
    Customers.CustomerID= Orders.CustomerID) 
</details>

## update 数据库
UPDATE impu SET barring = REPLACE(barring, 1, 0) where display_name = 13912345000;
UPDATE impu SET barring = REPLACE(barring, 1, 0) where identity like '%tel%';

UPDATE impu SET barring = REPLACE(barring, 1, 0) where identity like '%tel%';
UPDATE impu SET barring = REPLACE(barring, 0, 1) where identity like '%tel%';
UPDATE nds_trusted_domains SET trusted_domain = REPLACE(trusted_domain, "ims.mnc001.mcc001.3gppnetwork.org", "ims.mnc000.mcc460.3gppnetwork.org") where trusted_domain like '%ims.mnc%';

## install mysql
    https://www.cnblogs.com/fanshudada/p/9781794.html
    https://dev.mysql.com/downloads/mysql/
    https://itsfoss.com/install-mysql-ubuntu/
    sudo apt install mysql-server -y
    wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.30-linux-glibc2.12-x86_64.tar.gz --no-check-certificate
    [配置请参考](https://www.jianshu.com/p/276d59cbc529)

## install libaio.so.1
    apt-get install libaio-dev

# install libnuma.so.1
    apt-get install libnuma-dev

# 2020-09-10T01:16:58.429180Z 0 [ERROR] Fatal error: Can't change to run as user 'mysql' ;  Please check that the user exists!
    bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql

## add user group
    groupadd mysql
    useradd -g mysql mysql 

## install chkconfig tool
    1. sudo apt install sysv-rc-conf

    当我安装sysv-rc-conf时，报了如下的错：
    E: Unable to locate package sysv-rc-conf（无法定位sysv-rc-conf包）

    在软件源列表sources.list（该文本的位置在/etc/apt/sources.list）文件中的末尾添加如下内容：
    deb http://archive.ubuntu.com/ubuntu/ trusty main universe restricted multiverse
    2、sudo apt-get update
    3、apt-get install sysv-rc-conf
    4. sysv-rc-conf --list 
    5. cp /usr/sbin/sysv-rc-conf /usr/sbin/chkconfig

## install mariadb
    + download from : https://.org/download/#entry-header
    + wget https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.5.5/bintar-linux-x86_64/mariadb-10.5.5-linux-x86_64.tar.gz --no-check-certificate
    + apt-get install mariadb-server
    + [MariaDB三种方法安装及多实例实现](https://blog.51cto.com/13695854/2127892)
    + [MariaDB安装及基本配置（CentOS6.9）](https://www.cnblogs.com/52py/p/8074541.html)
    + [安装MariaDB和简单配置](https://www.cnblogs.com/wu-chao/p/9138665.html)
    + [Mysql创建新用户方法](https://blog.csdn.net/leili0806/article/details/8573636)
    + NOTES: you must remove all your mysql/mariadb from your system, or althrogh it seems the installation well
     but some actions during installation may fail without reporting any error to you.
    + after clean all mysql, you could run command to install
    + + apt-get install mariadb-servr
    + + after installation, run "whereis mysql" and "whereis mysqld", you could see the right output
    + + run command: mysql_secure_installation
    + + run command: mysql_install_db
    + + run command: service mysql start    (some procedure may advise you to run: service mysqld start, but in my system, mysqld service could not be found)
    + + show variables like "%character%";show variables like "%collation%";

## java.sql.SQLException: Access denied for user 'root'@'localhost'
    最终通过一下方式解决的， 依然很奇怪
    + + 首先实验 mysql -uroot -p命令是否需要正确的密码，只有需要输入正确的密码，才能继续进行； 然后，
    + + 修改数据库密码 update user set password=password("admin") where user="root";
    + + 刷新数据库flush privileges;

    + 如果不行采用执行下面的command：
    UPDATE mysql.user SET authentication_string = PASSWORD('admin'), plugin = 'mysql_native_password' WHERE User = 'root' AND Host = 'localhost';

    systemctl restart mariadb
    然后就可以了

## 解决mariadb设置初始密码不生效方法【Mariardb】

默认是无密码的，换用了更安全的认证，但是很多链接需要密码，所以，，，需要设置密码
操作如下：
```
root@ubuntu:mysql
mariadb>use mysql;
mariadb>update user set password=PASSWORD("admin") where User='root';
mariadb>update user set plugin="mysql_native_password";
mariadb>flush privileges;
mariadb>exit;
```
https://mariadb.com/kb/en/set-password/

## [Linux MySQL 常见无法启动或启动异常的解决方案](https://www.cnblogs.com/youjianjiangnan/p/10259151.html)

## remove mysql
    apt-get autoremove mysql* --purge
    删除mysql完整步骤：

    1、 sudo apt-get remove mysql-server.
    2、sudo apt-get autoremove mysql-server
    3、sudo apt-get remove mysql-common
    4、
    sudo rm /var/lib/mysql/ -R
    sudo rm /etc/mysql/ -R
    sudo apt-get autoremove mysql* --purge
    sudo apt-get remove apparmor

    再次安装mysql命令：sudo apt-get install mysql-server mysql-common

    To start mysqld at boot time you have to copy
    support-files/mysql.server to the right place for your system

    PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
    To do so, start the server, then issue the following commands:

    '/usr/bin/mysqladmin' -u root password 'new-password'
    '/usr/bin/mysqladmin' -u root -h default password 'new-password'

    Alternatively you can run:
    '/usr/bin/mysql_secure_installation'

    which will also give you the option of removing the test
    databases and anonymous user created by default.  This is
    strongly recommended for production servers.

    See the MariaDB Knowledgebase at http://mariadb.com/kb or the
    MySQL manual for more instructions.

    You can start the MariaDB daemon with:
    cd '/usr' ; /usr/bin/mysqld_safe --datadir='/var/lib/mysql'

    You can test the MariaDB daemon with mysql-test-run.pl
    cd '/usr/mysql-test' ; perl mysql-test-run.pl

    Please report any problems at http://mariadb.org/jira

    The latest information about MariaDB is available at http://mariadb.org/.
    You can find additional information about the MySQL part at:
    http://dev.mysql.com
    Consider joining MariaDB's strong and vibrant community:
    https://mariadb.org/get-involved/

## install ncat
    apt-get install netcat

    nc -vz -w 5 127.0.0.1 777 &> /dev/null && echo "online" || echo "offline"
    nohup nc -l -p 777 &
    nohup nc -lk -s 127.0.0.1 -p 777 &  
    使用案例如下：

    1、测试TCP端口

    nc -vz ip tcp-port

    2、测试UDP

    nc -uvz ip udp-port

    3、临时监听TCP端口

    nc -l port

    4、永久监听TCP端口

    nc -lk port

    5、临时监听UDP

    nc -lu port

    6、永久监听UDP

    nc -luk port


## alter user 'root'@'localhost' identified by '123456';


## command

### 查找指定库中所有表名
+ select table_name from information_schema.tables where table_schema='fusion';

+ select radio_id from fusion.usr_authority where usr_au=5 AND usr_id=9

+ select * from fusion.radio_info where radio_id in (select radio_id from fusion.usr_authority where usr_au=5 AND usr_id=1);

+ select * from fusion.radio_info where radio_type = #{typeCode} AND where radio_id in (select radio_id from fusion.usr_authority where usr_au=5 AND usr_id=1) order by radio_id ASC 

+ select * from fusion.radio_info where radio_id in (select radio_id from fusion.usr_authority where usr_au=5 AND usr_id=#{usrId}) AND radio_type = #{typeCode} order by radio_id ASC

selectByTypeWithUsrAuthority
```
select * from usr_info;
+----+----------+----------+-----------------+---------------------+
| id | username | password | description     | created_date        |
+----+----------+----------+-----------------+---------------------+
|  1 | admin    | admin    | 系统管理员      | 2020-07-28 22:50:18 |
| 13 | admin1   | 12345    | p：12345        | 2021-03-03 11:05:42 |
| 15 | admin2   | 12345    | P:12345         | 2021-03-06 18:40:39 |
|  9 | guest    | 123456   | guest           | 2020-08-27 23:11:52 |
| 17 | liyan    | liyan    | ceshi           | 2021-03-09 15:51:23 |
| 16 | test     | test     | test            | 2021-03-09 14:40:28 |
+----+----------+----------+-----------------+---------------------+
6 rows in set (0.000 sec)
```

select table_name from information_schema.tables where table_schema='fusion';

select * from radio_info;

select * from usr_authority where usr_id=13;
```
| usr_id | radio_id | radio_type | usr_au |
```
countClusterVHFRadio

selectCluterRadios

select COUNT(*) from radio_info,usr_info where radio_type=5 and username=7;


objeEditForm: {
                radioType: '',
                radioId: '',
                radioChannel: 0,
                radioName: '',
                radioCity: '',
                radioDesc: ''
            },

insert into communicating_info values(”,’glchengang’,’深圳一中’,’-10-10′); 

insert into radio_info values(101, '深圳101'， 'Radio', 'QD', 3, 1, 1, 1, '2020-08-27 23:11:52');

```
| radio_info | CREATE TABLE `radio_info` (
  `radio_id` int(16) NOT NULL DEFAULT 0,
  `radio_name` varchar(50) NOT NULL DEFAULT '',
  `radio_des` varchar(50) NOT NULL DEFAULT '',
  `radio_city` varchar(50) NOT NULL DEFAULT '',
  `radio_type` tinyint(2) NOT NULL,
  `radio_stat` int(12) NOT NULL DEFAULT 0,
  `radio_cnl` int(2) NOT NULL DEFAULT 0,
  `device_id` int(16) DEFAULT 0,
  `created_date` timestamp NOT NULL DEFAULT current_timestamp(),
  PRIMARY KEY (`radio_id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 |
```

alter table cere_buyer_bank change bank_card bank_card varchar(50) character set utf8;

show create table cere_buyer_bank;

## 解决mariadb无法访问的问题
```
root in idomall in mysql on  master [+]
⚡️ mysql -upackmall -ppackmall -h 127.0.0.1
ERROR 1045 (28000): Access denied for user 'packmall'@'192.168.80.1' (using password: YES)

root in idomall in mysql on  master [+]
⚡️

root in idomall in mysql on  master [+]
⚡️ ls
conf  docker-compose.yaml  initdb

root in idomall in mysql on  master [+]
⚡️ docker-compose down
Stopping mariadb-c ... done
Removing mariadb-c ... done
Removing network mysql_default

root in idomall in mysql on  master [+] took 3s
⚡️ docker-compose up -d
Creating network "mysql_default" with the default driver
Creating mariadb-c ... done

root in idomall in mysql on  master [+]
⚡️ mysql -upackmall -ppackmall -h 127.0.0.1
ERROR 1045 (28000): Access denied for user 'packmall'@'172.26.0.1' (using password: YES)

root in idomall in mysql on  master [+]
⚡️ mysql -upackmall -ppackmall -h 127.0.0.1
ERROR 1045 (28000): Access denied for user 'packmall'@'172.26.0.1' (using password: YES)

root in idomall in mysql on  master [+]
⚡️ ls
conf  docker-compose.yaml  initdb

grant all privileges on *.* to 'root'@'%' identified by 'password';
update `mysql`.`user` set `Grant_priv` = 'Y' where `user` = 'root';
delete from user where user='root' and host='localhost';
flush privileges;

update mysql.user set Grant_priv='Y' where User='root' and Host='%';

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

grant all privileges on awesome.* to 'root'@'192.168.*.*' identified by 'passwd';
FLUSH PRIVILEGES;
```

```
docker run --name mysql -d -p 3306:3306 -v /var/lib/mysql:/var/lib/mysql --entrypoint "/usr/bin/mysqld_safe" mysql_image
```

## mysql连接docker报错_本地宿主机通过mysql命令连接mysql Docker容器中的服务器报错 ERROR 2002 (HY000)...
```
1、具体所错与下所示：[user@cluster2 ~]$ mysql -uroot -p

Enter password:

ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)

2、根据报错提示，是本地mysql连接服务器时，没有找到/var/lib/mysql/mysql.sock文件。那么从这入手，我们看到mysql容器中的服务器启动后的mysql.sock文件在哪。

我们进入mysql容器中看看：sudo docker exec -it mysql /bin/bash

查看mysql配置文件my.cnfroot@9e313681fc3e:/# cat /etc/mysql/my.cnf

我们看到下面的一些配置：[mysqld]

pid-file = /var/run/mysqld/mysqld.pid

socket = /var/run/mysqld/mysqld.sock

datadir = /var/lib/mysql

secure-file-priv= NULL

# Disabling symbolic-links is recommended to prevent assorted security risks

symbolic-links=0

这里可以看到mysqld.sock的目录是在/var/run/mysqld目录下，但是这个目录，我们并没有挂载主机目录，下面我们重新运行mysql容器，挂载相应的容器，如下所示：sudo docker run --name=mysql -it -p 3306:3306 -v /opt/data/mysql/mysqld:/var/run/mysqld -v /opt/data/mysql/db:/var/lib/mysql -v /opt/data/mysql/conf:/etc/mysql/conf.d -v /opt/data/mysql/files:/var/lib/mysql-files -e MYSQL_ROOT_PASSWORD=123456 --privileged=true -d mysql

使用以上命令运行mysql容器，这里使用/opt/data/mysql中的相关目录挂载到mysql容器中，启动容器之后你会发现mysql服务器无法正常启动，报错如下：2018-12-29T06:25:09.480604Z 0 [ERROR] [MY-010273] [Server] Could not create unix socket lock file /var/run/mysqld/mysqld.sock.lock.

2018-12-29T06:25:09.480619Z 0 [ERROR] [MY-010268] [Server] Unable to setup unix socket lock file.

2018-12-29T06:25:09.480625Z 0 [ERROR] [MY-010119] [Server] Aborting

2018-12-29T06:25:11.289786Z 0 [System] [MY-010910] [Server] /usr/sbin/mysqld: Shutdown complete (mysqld 8.0.13) MySQL Community Server - GPL.

[user@cluster2 mysql]$ sudo docker logs mysql

2018-12-29T06:25:09.095301Z 0 [Warning] [MY-011070] [Server] 'Disabling symbolic links using --skip-symbolic-links (or equivalent) is the default. Consider not using this option as it' is deprecated and will be removed in a future release.

2018-12-29T06:25:09.095434Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.13) starting as process 1

2018-12-29T06:25:09.478963Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.

2018-12-29T06:25:09.480604Z 0 [ERROR] [MY-010273] [Server] Could not create unix socket lock file /var/run/mysqld/mysqld.sock.lock.

2018-12-29T06:25:09.480619Z 0 [ERROR] [MY-010268] [Server] Unable to setup unix socket lock file.

2018-12-29T06:25:09.480625Z 0 [ERROR] [MY-010119] [Server] Aborting

2018-12-29T06:25:11.289786Z 0 [System] [MY-010910] [Server] /usr/sbin/mysqld: Shutdown complete (mysqld 8.0.13) MySQL Community Server - GPL.

说是无法创建：Could not create unix socket lock file /var/run/mysqld/mysqld.sock.lock。这说明本地的/opt/data/mysql/mysqld目录，mysql容器中无权限防问。

注意：这里的/opt/data/mysql目录下的所示目录都是mysql容器启动时自动创建的，这里只有db目录的用户和组是：polkitd input，其它的就是root root，因此，我们要将其它几个目录的用户和组都改成polkitd input。命令如下：cd /opt/data

sudo chown -R polkitd:input mysql

这样，删除mysql docker 容器。重新创建：sudo docker rm mysqlsudo docker run --name=mysql -it -p 3306:3306 -v /opt/data/mysql/mysqld:/var/run/mysqld -v /opt/data/mysql/db:/var/lib/mysql -v /opt/data/mysql/conf:/etc/mysql/conf.d -v /opt/data/mysql/files:/var/lib/mysql-files -e MYSQL_ROOT_PASSWORD=123456 --privileged=true -d mysql

这样容器中的mysql就能正常运行了。

3、本地主机连接容器的mysql时，需要查到 /var/lib/mysql/mysql.sock。我们启动mysql容器后，在/opt/data/mysql/mysqld目录下有一个mysqld.sock。我们要把这个文件链接到本地主机的var/lib/myql目录中。sudo ln -s /opt/data/mysql/mysqld/mysqld.sock /var/lib/mysql/

这样在运行mysql -uroot -p输入密码就能正常连接，docker容器中的mysql服务了。

4、本地主机只需要安装mysql-community-client包就可以了。具体参考百度。
```

## postgres to mysql

- [pg2mysql](http://www.lightbox.ca/pg2mysql.php)


## 一定要-v /var/run/mysqld:/var/run/mysqld， 不然host无法访问mysql，跟权限无关
docker run --name=mysql -it -p 3306:3306 -v /var/run/mysqld:/var/run/mysqld -v /var/lib/mysql:/var/lib/mysql -v /opt/data/mysql/conf:/etc/mysql/conf.d -v /opt/data/mysql/files:/var/lib/mysql-files -e MYSQL_ROOT_PASSWORD=admin --privileged=true -d mysql:5.7