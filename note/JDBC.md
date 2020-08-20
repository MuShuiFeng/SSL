#jdbc开发步骤
	1. 下载数据库链接java应用程序的jar包:
		mysql-connector-java-XXX.bin.jar .class文件
	2. 在java工程上创建lib文件夹,将jar包放入lib文件夹
	3. 右键jar包,点击buildPath
	4. 使用java代码连接数据库的常用接口
#JDBC常用接口
	|- 	Connection接口
		Java自带,数据库连接接口
		/作用:
			一个特定数据库的连接,sql语句执行结果是在一个连接上下文返回
		/常用方法:
			//close();
				关闭连接
			//commit();
				提交事务
			//createStatement();
				创建一个Statement对象
			//setTransansactionIsolation(int level);
				设置隔离级别
			//getTransactionIsolation();
				获取隔离级别
			//getAutoCommit();
				获取自动提交等级
			//getMetaData();
				获取元数据
			//rollback();
				回滚
			//rollback(savePoint);
				回滚到保存点
			//setSavePoint(String name);
				设置带名保存点
			//setSavePoint();
				设置默认保存点
				
	|- 	Statement接口
		Java自带,用来发送或执行sql语句
		/作用:
		
		/常用方法:
			//execute(String sql);
				提交sql语句,但是只能执行DML和DDL语言
			//executeUpdate(String sql);
				返回值是受影响的行数(int)
				执行sql语句,可以执行大多数语句,除了DQL
			//executeQuery(String sql);
				执行给定的select语句,返回一个ResultSet对象
				返回值是一个结果集,包含数据和表结构

	|- ResultSet接口
		/常用方法:
			//next();
				判断是否有下一条记录,有就返回true,没有就返回false
			//getXXX(int indexColumn)
				根据字段下标获取字段
			//getXXX(String Column);
				根据字段名获取字段
	|- PreparedStatement接口
		/常用方法:
			//executeUpdate();
				不需要参数
			//executeQuery();
				不需要参数
			//setXXX(?,值);
			给问号赋值

#工程结构
	xxx.xxx.tool	工具包
	xxx.xxx.vo		实体类包
	xxx.xxx.dao		接口包
	xxx.xxx.daoImpl	接口实现类
	xxx.xxx.service	业务包
	xxx.xxx.serviceImpl	业务实现类
	xxx.xxx.test	测试包
					
#jdbc开发
	1.	加载驱动
		Class.forName("当前驱动所在地");
			将Driver类加载到jvm虚拟机中
	2. 	获取连接Connection
		conn = DriverManager.getConnection(URL,USER,PASSWORD);
		URL = "jdbc:mysql://localhost:3306/Player?characterEncoding=utf-8";
	3. 	创建一个声明Statement
		Statement stat = conn.createStatement();
	4. 	写sql语句
		String sql = "语句";
	5.	将语句发送出去
		stat.execute(sql);
	6.	释放资源
		stat.close();
		conn.close();
		
	
#工具类的封装

	##DBTools
		1. 从db.properties中获得mysql的连接属性
			private static String DRIVER="";
			private static String URL="";
			private static String USER="";
			private static String PASSWORD="";	
			static{
				//获得db.properties文件的字节流
				InputStream ip = DBTools.class.getClassLoader().getResourceAsStream("db.properties");
				//创建properties对象
				Properties p = new Properties();
				//
				try {
					p.load(ip);
				} catch (IOException e) {
					e.printStackTrace();
				}
				DRIVER=p.getProperty("driver");
				URL=p.getProperty("url");
				USER = p.getProperty("user");
				PASSWORD = p.getProperty("password");
				Class.forName(DRIVER);
			}
		2. 加载驱动
		
	##将数据库连接变量提取
	使用一种文件格式:
		xxx.properties	文件,提取数据库的连接参数变量
	把驱动,数据库连接,用户名,密码都存到这个文件中,并使用Properties类把属性读取到类中
	
		load(InputStream inStream);
			从输入字节流中读取属性列表
		load(Reader reader);
			从一个简单的行导向格式中读取输入字符流的属性列表	
#ORM映射(持久层框架)
	Object Realtion Mapping
	
	ORM映射思想:
		Java是面向对象的编程语言
		DBMS是使用关系型来保证数据的完整性
		为了保持两者的平衡,使用ORM映射
		就是将数据库表与Java实体进行映射
		将字段与属性进行映射
		将记录和对象进行映射
		手动映射
		
	ORM三种映射关系:
		|- 数据库中的表 table和Java中程序中的类 class 进行映射
			table<--->class
		|- 数据库表中的记录 record 和Java程序中的 new 对象进行映射
			record<--->对象
		|- 数据库中表的字段column 和Java应用程序中类的属性 进行映射
			column<---> property

#DAO层
	Data Access Object 数据访问对象
	DAO层的开发:
	1. 导入mysql的驱动jar包
	2. 根据数据库表完成对实体的映射  --->ORM
	3. 编写DAO层的接口
	4. 编写DAO层的实现类
	5. 单元测试

	
#工程顺序
	1. 创建db.properties
	2. 创建DBTools
	3. 创建工程
#预编译
	PreparedStatement
	
	目的:
		1.	解决sql注入问题,防止注入不良信息
		2.	执行sql语句要比Statement效率更高
	
	select username from person where username = ? and pwd = ?;
	
	方法:
		/executeUpdate();
			不需要参数
		/executeQuery();
			不需要参数
		/setXXX(?,值);
			给问号赋值

	##在jdbc中设置事务
	1. 设置手动提交
	2. try catch
	3. 在catch里判断是否回滚



#service层
	普通DAO层
	A,B,C
	service层
	AB,BC,AC
	
	service层书写规范
	com.oracle.service
		在这个包下创建同名接口
		与DAO层Dao接口相似
		
	com.oracle.serviceImpl
#control层
	
#元数据
	从结果集中获得一个表的结构信息
	
	获取数据的数据
	ResultSet rs = ps.executeQuery();
		/获取结果集
	ResultSetMetaData metaData = rs.getMetaData();
		/获取结果集的元数据
	int type = metaData.getColumnType(int x);
		/获取第x个字段的字段类型
	String name = metaData.getColumnName(int x);
		/获取第x个字段的字段名
	String typeName = metaData.getColumnTypeName(int x);
		/获取第x个字段的字段类型名
	int size = metaData.getColumnDisplaySize(int x);
		/获取第x个字段的字段显示大小
	String tableName = metaData.getTableName();
		/获取表名
	
	
	
	
	
	
	
	
	
	
	
	
	