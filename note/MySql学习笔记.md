#数据库
解决数据的持久化,有效并搞笑的管理数据,从而做到增删改查
1. 	DBMS数据库管理系统mysql
		用来创建和操纵数据库
2. 	DB 	database,数据库,存储数据的仓库
		可以用来存储数据
3. 	SQL 结构化查询语言
#配置数据库的字符集
	创建数据库时在创建时添加DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci
	create database db DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
#用命令行打开mysql
	mysql -u root -p root
	
##着重号
	esc下面的按键,实际操作为`字段名`,可以使用一些特殊字符,使得字符不参加编译

##约束:
	主键约束:
		primery key;
		复合主键:
			PRIMARY KEY('字段名','字段名');
		|- 修改主键
			alter table 表名
		|- 主键自增
			auto_increment;
	外键约束:
		foregin key;
		|- 创建外键
		constraint 外键名 foreign key(外键字段名) REFERENCES 主键所在表(主键字段)
		|- 	修改表时添加外键
		alter table 表名 add column 字段名 字段属性; 
		alter table 表名 add FOREIGN key(外键字段名) REFERENCES 主键所在表(主键字段)
	非空约束:
		not null;
	唯一约束:
		unique;
	检查约束: 
		check(字段名 ><= 值);(mysql不支持);
	默认约束:
		default;

		
##表复制:
	/复制表结构和数据
		create table 新表名 as select * from 表名 where 1=1;
	
	/复制表结构
		create table 新表名 as select * from 表名 where 1=2;

##针对SQL:
1.	DDL(Database Definition Language):
		数据库定义语言
		/使用该数据库
			use 库名;
			
		/将数据库字符集改为UTF-8;
			alter database 数据库名 character set utf8;
			
		/查看数据字符集
			show variabkes like '%character%';
		
		/显示表中的信息
			desc 表名;
			
		|- 创建
		
		/创建一个数据库
			create database 库名;
			
			
		/创建一个包含一定字段的一个表
			create table 表名(
				字段名 数据属性,
				字段名 数据属性
			);
			
		|- 	删除
		
		/删除该数据库
			drop database 库名;
			
		/删除该表
			drop table 表名;
		
		|- 修改数据
	
		/为该表增加一个属性
			alter table 表名 add [column](可写可不写) 字段名 数据类型;
			
		/为该表删除一个属性
			alter table 表名 drop 字段名;
		
		/将该字段修改成需要的类型
			alter table 表名 modify [column](可写可不写) 字段名 数据类型;	
			
		/将旧字段改成新字段
			alter table 表名 change 字段名 新字段名 数据类型
			
		/表名更改
			alter table 表名 rename to 新表名;

		
			
		
2.	DML
		数据库操纵语言
		|- 添加数据
		/insert into 表名(字段列表) values(值列表);
			
		/insert into 表名(字段列表) values(值列表),(值列表),(值列表);
			批量插入
		|- 删除数据
		/delete from 表名;
			直接删库;
			
		/delete from 表名 where 字段名 = 值;
			根据给定条件删除库内数据;
			
		/delete from 表名 where 字段名 in (值,值);
			根据字段中带值的全部删除;
		
		|-修改数据
		/update 表名 set 字段名 = 值 (,字段名 = 值  可写可不写) where 字段名 = 值;
		
		
3. 	DQL
		数据库查询语言
		select * from 表名;   查询所有      效率低
		select id from 表名;   单个查询      效率高
		select * from 表名 where id = ？;     条件查询
		select * from 表名 where id in(2,4);    条件查询
		select * from 表名 where name =？ and  pwd =？

		select * | 字段列表 | 别名 | 表达式 | 函数 | distinct from 表名where 条件

		别名查询
		select id as 编号,name as 姓名  from 表名
		
		逻辑运算符
		|- >大于,<小于,=等于,>=大于等于,<=小于等于,<> 不等于
		
		|- and   连接两个并列条件 同时满足
		select * from person where name= ？ and pwd =？;
		
		|- between xxx and xxx    	在两者之间并包含两者
		select name from emp where age between 18 and 26;
		
		|- or   满足一个
		select * from person where gender= ‘男’  or  age > 18;
		
		|- not   not in()
		select * from person where id not in(1,3);

		where 后面的模糊查询
		%	通配符 n个字符
		_ 	占位符 占1位
		%a% 包含
		%a 	结尾
		a% 	开头
		select * from person where name like ‘李%’;  查询姓李的
		
	##表达式
		|- dual 伪表
			select 表达式 from dual;
			例:
			select 1+1 from dual;
	
		|- distinct 去重
			去掉指定字段重复的值
			select distinct 字段名(,字段名) from 表名
	
		|- 相加
			/数字与数字相加为数字
			
			/字符与数字相加为数字
			
			/字符与字符相加为0
			
			/与null相加都为null;
			
	##函数
	
		|- 聚合函数
			/count();
			查数
				select count(*) from 表名;
			
			/max();
			最大
				select max(字段名) from 表名;
			/min();
			最小
				select min(字段名) from 表名;
		
			/sum();
			相加
				select sum(数值型) from 表名;
			/avg();
			平均数
				select avg(数值型) from 表名;
			/group
				
			
		|- 数学函数
			/ABS(x);
				绝对值
			/BIN(x);
				二进制
			/PI();
				圆周率
			/MOD(x,y);
				取余,
			/RAND();
				0~1之间取随机数
			/RAND(X,Y);
				X~Y之间取随机数
			/TRUNCATE(X,Y);
				返回X截断Y位的小数的结果
			
			
		|- 单行函数
			/where 字段名 between A and B;
				输出A和B之间的数据(包含AB)
		
			/where 字段名 in A;
				包含条件就输出
			
			/is null
			
			/is not null
		
			/not 
		
			/or
				select * from where 字段名 like 正则表达式 or 字段名 not like 正则表达式;
			/like 
			select * from where 字段名 like 正则表达式 ;
			select * from where 字段名 not like 正则表达式 ;
				模糊查询
				|- 	%		通配符
					a%		以a开头的字符
					%a		以a结尾的字符
					%a%		包含a的字符
				|- 	_ 		占位符
					_a%		a前一个字符的字符串
					__a%	a前两个字符的字符串
		
		|- 分组函数
			/group by 字段名
				查询该字段并按该字段分组
		
		|- 字符串函数
			/字符型数据的长度:
				length(字段名)
			
			/concat(x,y);
				拼接x和y
			
			/INSERT(str,x,y,instr);
				从str的第x个位置插入instr的前Y个字符
				
		|- 日期函数和时间函数
			|- 时间类型
				/DATETIME
					yyyy-MM-dd hh-MM-ss
				/DATE
					yyyy-MM-dd
				/TIME
					hh-MM-ss
				/YEAR
					yyyy
				/MONTH
					MM
				/DAY
					dd
			/TIMESTAMPDIFF(YEAR|MONTH|DAY,被减时间,相减时间)
			时间相减函数
			/last_day();
			时间的最后一天;
			/now();
			获取系统当前时间
			/current_date();
			返回当前时间的年月日
			/month();
			获取当前时间月份
			/year();
			获取当前时间年份
			/day();
			获取当前时间日期
			/week();
			获取当前时间周
			/timestampdiff(year|month|day , 开始时间,结束时间);
			截取需要时间
			/str_to_date('1999-09-09','%Y-%m-%d %h-%i-%s');
			字符串转成日期
			/date_format(date,'%Y-%m-%d %h-%i-%s');
			日期转成字符串
			
		|- 条件函数
			/if
			
			/case when
				select ename case gender when 0 then '女'
										 when 1 then '男' 
										 when 2 then '为止' end as 性别
				from emp;
		
		/MD5加密函数
			MD5(XXX);
			将指定字符串随即便成为长度为32的字符串,提高了数据的安全性,无法被解密
			//加盐
				在MD5基础上再次加密
		/uuid();
			mysql自己提供的随机生成主键的方法
4.	DCL
		数据库控制语言
		
		
		
	mysql的使用:
		/不区分大小写
		/以;结束
		/分行写
		/尽量提高可读性

##排序
	order by
		select * from 表名 order by 字段名 (asc);
		按该字段从小到大排序
		
		select * from 表名 order by 字段名 desc;
		按该字段从大到小排序
##过滤
	having 过滤条件
	与where功能相似,但是语法结构不同
	语法结构:
		在groupby 后面 , 功能是先分组再过滤,可以与Where联用
	/where 和 having 区别
		where  是先过滤在分组	不可以再后面使用分组函数
		having 是先分组再过滤	可以在后面使用分组函数
	
	select count(1) form emp groupby deptno having count(1)>5;
		
##分组
	group by	
		select * from 表名 group by	字段名;
	基本与count()组合使用,
		select cid 班级 , count(1) 人数 from class group by cid;
	
##分页
	limit	物理分页
		select * from 表名 limit 0,5;
		五个一页
		
	页码计算公式:
		int startRow = (页码-1)*页大小;
	
##打开sql脚本
	|- navicat
	|- cmd窗口
		source sql文件具体路径
		
		
##表关联查询
	即多表查询,级联查询
	
	###笛卡尔积现象
	没加条件的多表查询操作得到的结果
		解决办法:
			添加外键条件,确保条件成立
##表连接查询的语法结构
	|- sql 92版本 
		内连接:
			/等值连接:
			select girlname , boyname from girls g , boys b 
			where g.boyfriend_id = b.id
			(and or);
			
			/非等值连接:
			select salary , grade_level from 表名 ,表名1,表民2 where salary between 表1.等级 and 表2.等级 
			
			/自连接:
				自己链接自己查询
				查询员工姓名和上级姓名
			select a.ename 领导姓名, b.ename 员工姓名 from emp a, emp b where a.empno = b.mgr;
			
		外连接:
			支持的不好,基本忽略
	|- sql 99版本
		内连接:
			/等值连接:
				inner join on 
			select name from Girls g 
			inner join Boys b 
			(join)
			on g.boyFriend_id = b.id
			(where);
			
			/非等值连接:
			/自连接:
				select a.ename 领导姓名, b.ename 员工姓名 from emp a 
				inner join emp b 
				on a.empno = b.mgr
				(join 表3 on 表3.c = 表2.b)
				(where);
			
		外连接:
		查询两个表,一个表有数据,另一个表没有数据时使用外连接
		例:
			查询有男朋友的女明星的信息和男朋友姓名
			select g.* , b.boyName from beaty g
			inner join boys b 
			on g.boyfriend_id = b.id;
			
			
			/左外连接:
			left join on 
				
				left 左边是主表,会将主表所有信息显示出来,从表中没有的补空
				例:
					select <条件> from A left join B on a.key = b.key where b.key is null;
				
			/右外连接:
			right join on
				right 右面是主表,会将主表所有信息显示出来,从表中没有的补空
				
				例:
					select <条件> from A right join B on a.key = b.key where a.key is null;
				
			/全外连接:(mysql不支持)
			full join on
				
			/交叉连接:
			cross join 
				为获得笛卡尔积现象
				例:
					查询有男朋友的女明星的信息和男朋友姓名
					select g.* , b.boyName from beaty g
					cross join boys b ;
	
##E-R图
	一对一
	一对多
	多对多
	
##子查询
	sql语句中包含一个sql语句,就是子查询
			查询办公地点在纽约的人
			select ename,deptno from emp
			where deptno = (select deptno from dept
			where loc = 'NEW YORK');
			子查询可以放在where having from select后面
	
	
				
##数据库事务管理(Transction)		
	事务指的是一件或多件事件的集合,要么全做,要么全不做
	
	###事务的特性
	|- 原子性:
		一个事务不会出现中止操作,要么全执行,要么全部执行,不会停在某一个阶段,不会被拆分
	
	|- 一致性(Consistency):
		在事务开始时和完成时,数据库中的数据都保持一致的状态
	
	|- 隔离性(Isolation):
		一个事务正在执行时,不能被其他事务影响
		为了防止事务操作间的混淆,所以采用一种串行化或序列化的方式来执行不同的事务,使得同一时间仅有一个请求来操作同一数据
		(前提是,事务在正确提交之前,不允许把事务操作的数据提供给其他事务)
		
		|- 隔离级别:
			|- 读未提交数据:	read uncommited
				会出现脏读,不可重复读,幻读
				
			|- 读已提交数据:	read commited
				解决了脏读,会出现不可重复读,幻读
				
			|- 可重复读:		reaptable read
				解决了脏读和不可重复读,可能会出现幻读
				
			|- 串行化:		serializable
				全部解决
			(mysql使用可重复读)
			
			/修改隔离级别的语句:
				set GLOBAL TRANSACTION ISOLATION LEVEL read UNCOMMITED;
				set GLOBAL TRANSACTION ISOLATION LEVEL read commited;
				set GLOBAL TRANSACTION ISOLATION LEVEL reaptable read;
				set GLOBAL TRANSACTION ISOLATION LEVEL serializable;
				设置 全局  事务        隔离      级别
		|- 锁:
			|- 悲观锁:
				select * from account where aid = 1 for update;
				
			|- 乐观锁(乐观锁是抽象的,不是具体的锁,是通过判断版本号来控制):
				时间戳与版本号
				版本号是一种字段
				
				T1查询的版本号是1,T1修改后版本号为2;
				T2读取的版本号是2,当T1回滚,版本号回滚到1,版本号相比较,即可判断数据是否被更改
				
					
	|- 持久性(Druability):
		事务一旦提交,永久有效,不能回滚
		
	
	###事务的提交方式
	/默认(自动提交):
		commit:
	/查询提交方式:
		show variables like '%commit%';
	
	/set autocommit =0;
		设置成手动提交
		//设置为手动提交时,需要在最下面写 commit;
	
	*客户端和客户端之间不会相互影响,客户端一修改为手动不会影响客户端二
	
	/begin;
		设置手动提交后,开启一个事务
	
	/回滚
		rollback();
		要在提交之前执行,如果执行了就不能回滚了
		//回滚至保存点
			rollback to a;
		
	/保存点:
		savepoint a;
		设置一个保存点,方便rollback;				
				
	###事务并发问题
	|- 更新丢失:
	|- 脏读:
		事务T1 修改了A表中的某一条记录,并将数据写回表中
		事务T2读取了同一条数据,这时T1由于某种原因,产生了回滚,造成了数据恢复到原始状态,此时T2读取到的数据与数据库表中的数据产生*不一致*,那么T2读取的数据即为脏读数据,也就是不正确的数据
		
	|- 不可重复读(体现update语句):
		是指在一个事务中,T1多次读取同一条记录,这个事务还没结束时,T2访问该记录,并进行修改,此时T1多次读取的记录不一致,这种现象即为不可重复读
	|- 幻读(体现insert语句):
		目前工资为5000的员工为10人,T1读取所有工资人数为10,此时T2插入一条工资为5000的记录,此时,T1读取工资为5000的人数为11人,此时T1即为幻读
				
	不可重复读的重点是修改:
		同样的条件,第一次读取的数据,再次读取时,数据有变化
				
##视图
	|- 创建视图
		create view 视图名 as select ename from emp;
	
	|- 查询视图:
		select * from 视图名;
		
	|- 删除视图:
		drop view 视图名;
	
	|- 修改视图:
		alter view 视图名;
		
	|- 修改视图数据:
		update view 视图名 as;
		
		
		
		
		
		
		
		