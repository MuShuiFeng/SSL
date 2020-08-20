#ORM工具
	mybatis 和 hibernate 都属于 ORM映射工具
	hibernate 帮助把 jdbc封装,用实体类就可以操作数据库
		例: User User.hbm.xml
#整体结构
	sqlSessionFactoryBuilder 	构造者
	SqlSessionFactory			sql会话工厂
	SqlSession					sql会话		增删改查,事务回滚
	
#开发顺序
	1. 构建mybatis的核心文件
	2. 创建接口,xml
	
#开发模式
	1. dao+daoImpl 实现类
		使用sqlsession调用增删改查方法
	2. 动态代理
		直接使用接口,不需要使用实现类
		namespace 必须是接口的全路径名
		接口的方法名必须和select的id是一致的
		入参类型必须和parameterType是一致的
		返回值类型必须和resultType一致
	3. 注解开发:
		SQL语句和接口 
		@select("select * from user")
		public User login(){} 
#jsp内置对象
	|- Session
	|- request
	|- response
		
#功能
	
	##模糊查询
	${} = concat()
	#{} = ?
		select * from user where userName like concat("%",#{like},"%")
		select * from user where userName like "%${like}%";
	
	##分页
		1. limit
		2. rowBounds 对象分页
		3. pageHelper插件
	##批量删除
		使用动态sql 的foreach标签,
			item="遍历的变量名" collection="入参的容器类型" open="(" separator="," close=")"
		#{item的值}
		
#属性
	普通查询
	resultType
	parameterType
	级联查询
	resultMap
		|- id 主键
			|- property:实体中的属性名
			|- column: 去对方查询时需要的字段
		|- result 属性
			|- property:实体中的属性名
			|- column: 去对方查询时需要的字段
		|- association 级联查询的乙方
			|- property:实体中的属性名(自己实体表里乙方的外键的变量名)
			|- column: 去对方查询时需要的字段(外键的字段名)
			|- select: 具体地址的方法
			|- javaType: 在Java文件中的数据类型
		|- collection 级联查询的多方
			|- property:实体中的属性名
			|- column: 多对多查询时 携带自己的主键 去对方查询
			|- select: 具体地址的方法
			|- ofType: 集合的泛型
		
		
#PowerDesigner
	1. 创建概念图 conceptual Diagram
		Entity与Entity的关系
	2. physical Diagram


#级联查询
	mybatis级联查询:
	|-	多表联查
		selete * from card c left join user u 
			on c.user_id = u.uid
				where number = #{number};
	|- 	分开写
		select * from card where number = #{number};
		根据结果中的user_id去对方执行
		select * from user where uid = #{user_id};
	
#主键回填
	假设表中没有数据,要插入数据的时候,立刻返回当前插入的id
	三种写法:
	|- 使用selectKey标签
		select LAST_INSERT_ID();
		按主键自增
		selectKey的属性,keyProperty = 属性名,order="after|before"
	|- 在insert标签中,使用userGeneratedKeys = "true" ,keyProperty="主键名"
	|- 按照自定义规则生成主键
		selectKey的
			select if(max(主键) is null , 1 , (max(主键)+2)) from 表名
			
	
#mybatis的缓存
	将数据存储到内存中
	分为一级缓存和二级缓存
	默认开启一级缓存
	特点:
	|- 一级缓存是sqlSession级别的缓存,仅限于同一个sqlSession
		增删改会清楚缓存,防止脏读
	|- 二级缓存需要设置一个开关
		二级缓存即为xxxMapper.xml
		设置二级缓存
		|- 在全局配置文件中开启
			<setting name="cacheEnabled" value="true"/>
		|- 在mapper文件中
			<cache></cache>
				cache的属性
				|- eviction="FIFO"
					垃圾回收机制
				|- size="1024"
					最大的容量,能存多少个对象
				|- readOnly="true"
					是否是只读
				|- flushInterval="1000"
					多少秒之后缓存自动销毁
	
	缓存顺序
		先找二级缓存,没找到再找一级缓存,再没找到在从内存中创建
		如果二级缓存找到了,就直接返回
		如果二级没找到,在一级缓存中找到,也直接返回
	
	
	二级缓存的前提是,实体必须实例化
	
#全局配置文件
	setting 可以配置 日志,延迟加载,按需加载,缓存cache
	typeAliases 别名
	数据源 eveirments eveirment mappers mapper package
#级联查询
	<association property="查询方实体类中被查询方的对象" column="查询方自身的主键" 
	select="被查询方的根据查询方主键查询方法">
	查询一个
	<collection property="查询方实体类中被查询方的对象" column="查询方自身的主键"
	select="被查询方的根据查询方主键查询" ofType="被查询方的别名" fetchType="lazy|eager">
	
#延迟加载
	前提是使用级联查询
	<setting name="lazyloadingEnale" value="true">
	按需加载
	<setting name="aggressiveLazyLoading" vale="false">
	延迟加载触发方法
	<setting name="lazyLoadiTriggerMethods" value="">
#主键回填
	
#动态SQL	
	if test ="username !=null and username !=''"
	foreach 批量删除
		|- collection="被遍历的集合的类型"
		|- item = "被遍历的对象"
		|- 
	
	