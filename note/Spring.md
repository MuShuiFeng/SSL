#spring 
	javaEE架构的一个 轻量级的 核心架构, 解决业务逻辑层和其他各层的耦合度过高的问题
	##框架介绍
	一个JavaEE的轻量级开源的架构,目的是解决公司
	##spring技术结构
	|- 数据访问模块 DataAccess
	|- 处理浏览器的请求和响应的模块 web
	|- 整合test单元测试的模块 test
	|- 面向切面模块 aop
	|- 核心模块
	
	##核心技术
	IOC\DI & AOP & 事务管理
	##IOC\DI
		控制反转: inverse of control
			创建对象的权力交由spring框架完成
		依赖注入: dependemcy Inject
			|- 基于构造函数的依赖注入
			|- 基于设置函数的依赖注入
			|- 基于自动装配的依赖注入
			|- 基于注解的依赖注入
		底层原理:
			|- 通过xml解析,
			|- 工厂模式,
			|- 反射
		IOC的接口:
			|- ApplicationContext
				在加载配置文件时,直接创建对象
				|- ClassPathXmlApplicationContext();
					工程中的文件路径
				|- FilePathXmlApplicationContext("实际路径");
					在磁盘中的文件路径
			|- BeanFactory
				是spring默认提供的加载配置文件的接口
				在扫描后不先创建对象,调动时才会创建当前类的对象
				|- 普通bean BeanFactory
					<bean id="user" class="com.ssl.pojo.User" ></bean>
				|- 工厂bean FactoryBean
					必须实现FactoryBean
				*区别是beanFactory返回的是自己的实例,而factoryBean返回的需要返回的实例
				*例如在鱼铺能买到鱼,但是在工厂能买到鱼罐头
		##spring的开发:
		|- 下载 jar包:
				https://repo.spring.io/release/org/springframework/spring/
		|- 导jar包
			|- beans
			|- context
			|- expression
			|- core
			|- common-logging
		|- 添加核心配置文件
			
		###spring对bean的管理
		|- 创建当前类的对象
			通过无参构造
			如果没有无参构造方法
				<constructor-arg name="类的属性" value="要赋的值></constructor-arg>
		|-给对象的属性赋值
			|- 普通属性
			/给属性赋值的方法
				|- 构造方法
					<constructor-arg name="类的属性" value="要赋的值></constructor-arg>
				|- getter\setter方法
					<property name="类的属性" value="要赋的值" ></property>
					赋值时如果出现特殊字符
					<property name="类的属性">
						<value>
							<![CDATA[需要的值]]>
						</value>
					</property>
			|- 命名空间赋值
				    
		|- 对象
		/给对象赋值的方法
			<property name="对象名" ref="对应对象的id名" ></property>
			<bean id="对象名" class="对象所对应位置"></bean>
			
			<property name="对象名">
				<bean id="对象名" class="对象所对应位置"></bean>
			</property>
			
		|- 集合
			|- 使用标签进行赋值
			<property name="array">
				<array>
					<value></value>
					<value></value>
					<value></value>
					<value></value>
				</array>
			</property>
			<property name="map">
				<map>
					<entry key="">
						<value></value>
					</entry>
				</map>
			</property>
			|- 通过注释赋值
			在注释中添加
			xmlns:util="http://www.springframework.org/schema/util"
			添加后可以在xml中使用
			<util:list id="list">
				<value></value>
			</util;list>
			util的id与property的ref对应,可以使bean标签下的property使用
			<property name="list" ref="list"/>
			/例:
				<util:list id="list">
					<value>1</value>
				</util;list>
				<bean id="user" class="com.ssl.pojo.User">
					<property name="numbers" ref="list" />
				</bean>
		|- bean的作用域
			scope="singleton|prototype|request|session"
			默认为单例模式,可以设置为多例(prototype),请求(request),或者session
			
			/例:
				<bean id="user" class="com.ssl.pojo.User" scope="singleton" ></bean>
		|- 基于xml的自动装配
			<autowire="byName|byType">
			id要与实体类的变量名相同
			无法使用自动装配添加集合类型,只能添加单个对象
			*如果是byType,就不能再创建同一类型的bean
			/例:
				<bean id="user" class="com.ssl.pojo.User" scope="singleton" autowire="byName" ></bean>
		|- 初始化
			初始化方法需要自己写
			init-method="init"
		|- 销毁
			销毁方法需要自己写
			destory-method="detory"
		|- 后置处理器
			BeanPost 
			继承BeanPostProcessor
		###Bean的生命周期
			1. 扫描xml文件,并实例化当前类的对象,执行构造方法
			2. 给属性赋值
			3. 后置处理器
			4. 初始化
			5. 执行目标方法
			6. 销毁
		
		###外部文件加载实例
		|- 导入jar包
		|- 加需要的指定约束
				|- 导入数据库的驱动信息
			xmlns:context="http://www.springframework.org/schema/context"
		|- 加载外部文件
			<context:property-placeholdere location="classpath:db.properties"></context:property-placeholdere>
		##注解扫描器
			<context:component-scan base-package="com.ssl.daoImpl"/>
				将该包里的所有类扫描
		##组件扫描器
			<context:exclude-filter type="annotation" expression="不需要扫描的类的全路径名"/>
				将不需要扫描的类放在里面
			<context:include-filter type="annotation" expression="只想扫描的类的全路径名"/>
				将只想扫描的类放在里面
		##spring的注解开发
		用于特殊标识Java代码,语法结构:@注解名
		使用方式:放在方法上,类上,属性上
		目的:简化开发
		|- 创建对象的注解名
			|- @Component
				|- @Controller	控制层
				|- @Service		业务层
				|- @Repository	模型层
			这三个都基于Component,分别对应MVC设计模式	
		|- 用注解给属性赋值
			|- 普通属性:
				@Value
			|- 引用对象:
				可以省去给对象赋值的过程,直接创建对象
				@Autowried
					按类型注入
					/例:
						@Autowried
						private User user;
				@Qualifire
					按名字注入
					必须与Autowried连用
					@Autowired
					@Qualifier("UserDaoImpl")
				@Resource(不建议)
					仅供SE使用,按类型,名字注入,优先按类型注入
		|- 全局扫描器
			@Configuration
			@Componentcan(basePackages = "com.ssl")
			/例:
				@Configuration
				@ComponentScan(basePackages = "com.ssl")
				public class SpringConfig {}
					AnnotationConfigApplicationContext anno = new AnnotationConfigApplicationContext(SpringConfig.class);
		
		###开发顺序
		1. 导jar包
		2. 在配置文件中添加全局扫描器
			
	##AOP
		Aspect Oriented Programming
		是OOP面向对象编程的延申,通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术,利用ao可以对业务层等的各个部分进行隔离
		目的是使业务逻辑层与各部分之间的耦合度降低,最终提高程序的可重用性
		面向切面编程
		
		###动态代理方式:
		|- jdk代理
			应用场景:在有接口的情况下,使用jdk代理,spring默认使用的就是jdk代理
			/使用顺序:
			1. 创建目标类
			2. 创建切面类对象	
			/内部方法:
			1. newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler handler)
				loader: 当前代理类的加载器
				interfaces: Class类型的数组,放的是接口的反射类型
				handler: 接口
			/例:
				public static UserDao getUserDao(){
					UserDao userDao = new UserDaoImpl();
					MyAspect myAspect = new MyAspect();
					Class[] classes = {UserDao.class};
					UserDao userDao1 = (UserDao)Proxy.newProxyInstance(JDK_Proxy.class.getClassLoader(), classes, new InvocationHandler() {
						@Override
						public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
							myAspect.before();
							Object invoke = method.invoke(userDao,args);
							myAspect.after();
							return invoke;
						}
					});
					return userDao1;
				}
		|- cglib代理
			应用场景:没有接口的时候,只要普通类,就是用cglib代理
			/例:
				public static UserDaoImpl getUserDaoImpl(){
					UserDao userDao = new UserDaoImpl();
					MyAspect myAspect = new MyAspect();
					Enhancer enhancer = new Enhancer();
					//通过enhancer生成目标类的子类
					enhancer.setSuperclass(UserDaoImpl.class);
					enhancer.setCallback(new MethodInterceptor() {
						@Override
						public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
							myAspect.before();
							Object invoke = method.invoke(userDao,objects);
							myAspect.after();
							return invoke;
						}
					});
					UserDaoImpl u = (UserDaoImpl)enhancer.create();
					return u;
				}
		###开发顺序
			1. 导包
			2. 创建代理类,创建切面类
		###开发准备
		使用spring的aop时,提供一个aop框架,AspectJ 这个框架并不是spring的组件
		开发准备:
			1. aop专业术语:
				1. Target 		目标类
				2. JoinPoint 	连接点:目标类中需要添加增强的方法
				3. PointCut 	切入点:被添加增强的方法,就是切入点
				4. advice 		增强(通知)
				5. weaving 		织入
				6. proxy 		代理
				7. Aspect 		切面
					|- 切点
					|- 通知
			2. 切入点表达式
				execution([访问修饰符],[方法返回类型][包名][类名][方法名](入参))
				/例:
					public class User(){
					addUser(); selectUser();
					}
					/例如1:
					execution(* * com.ssl.pojo.User.addUser(..));
					/例如2:
					execution(* * com.ssl.pojo.User.*(..));
					/例如3:
					execution(* * com.ssl.pojo.*.*(..) throws);
			3. 通知
				1. before			前置通知
					在目标方法前添加功能
					
				2. afterReturning 	后置通知
					在目标方法运行后添加功能
					|- method=""
						要添加的方法
					|- pointcut-ref=""
						要添加方法的切点
					|- returning=""
						接收方法运行后的返回值,使用前要先
					
				3. around 			环绕通知
					目标方法前后都添加功能
					使用时,需要在通知类添加特殊方法
						public void around(ProceedingJoinPoint proceedingJoinPoint){
							System.out.println("事务开始");
							Object o = proceedingJoinPoint.proceed();
							System.out.println("事务结束");
						}
				4. affterThrowing 	异常通知
					只在发生异常的时候添加功能
					|- method
					|- pointcut-ref
					|- returning="e"
						返回异常发生后的异常对象
					需要添加特殊方法
						public static void afterThrow(Throwable  throwable){
						System.out.println("发生异常");
						System.out.println("throwable");
						}
					
				5. after			最终通知
					在最后执行
		###Spring的AOP开发:
		|- 基于xml
			|- 导包:
				|- beans
				|- context
				|- expression
				|- core
				|- aop
				|- aspects
				|- com.springsource.org.aopalliance-1.0.0
				|- com.springsource.org.aspectj.weaver-1.6.8.RELEASE
				|- common-logging
			|- 配置文件
			|- 定义接口和目标类
			|- 定义切面类
			|- 
		|- 基于注解
			|- 编写xml文件
			|- 添加注解扫描器
				<context:component-scan base-package=""/>
			|- 添加aop自动代理
				<aop:aspectj-autoproxy/>
				如果要使用cglib
				<aop:aspectj-autoproxy proxy-target-class="true"/>
			|- 在切面类和目标类上添加注解
				@Aspect @Service
			|- 再切面类中添加通知
				|- 公共切入点
					@Pointcut("execution(* com.ssl.dao.*.*(..))")
				|- 前置通知
					@Before(value = "myPointCut()" )
					
	## spring事务管理
		事务分为两种:
			|- 编程式事务(淘汰)
			|- 声明式事务
		### jdcTemplate 持久层操作对象
			*使用jdbcTemplate的queryForObject时,要用new BeanPropertyRowMapper<T>(T.class)包装
			|- XML开发顺序
				|- 导包
				|- 在xml中配置文件
					<context:property-placeholder location="db.properties"/>
					<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
						<property name="driverClassName" value="${driver}"/>
						<property name="url" value="${url}"/>
						<property name="username" value="${user}"/>
						<property name="password" value="${password}"/>
					</bean>
					<bean id="jdbcTemplete" class="org.springframework.jdbc.core.JdbcTemplate">
						<property name="dataSource" ref="dataSource"/>
					</bean>
					<bean id="userDaoImpl" class="com.ssl.daoImpl.UserDaoImpl">
						<property name="jdbcTemplate" ref="jdbcTemplete"/>
					</bean>
					<bean id="userService" class="com.ssl.serviceImpl.UserServiceImpl">
						<property name="userDao" ref="userDaoImpl"/>
					</bean>
				|- 使用spring的tx:advice ,将事务管理器中的方法变为增强
					<tx:advice method="" readOnly:"" timeOut="" propagation="" />
				|- 使用aop,将增强织入切入点
					<aop:config>
					<aop:advicor advice-ref="" pointCut=""/>
			|- 注解开发顺序
				|- 创建数据源的事务管理器
					DataSourceTransactionManager 注入数据源DataSource
				|- 添加驱动:
					tx
			|- Spring使用的接口:
				|- PlatformTransactionManager
					平台事务管理器
					|- JDBCDataSourceTransaction
					|- 
					|- 
					|- 
					|-
					*事务的传播特性
					多个事务相互调用时,如何管理事务
					|- PROPAGATION_REQUIRED
						当前方法执行时,如果执行在事务环境,则直接执行事务,若不在事务环境,则创建事务并执行
					|- PROPAGATION_REQUIRED_NEW
						当前方法执行时,强行使用自身事务,不管是否在事务环境中
				|- TransactionDefinition
					事务的定义
				|- TransactionStatus
					事务状态
		### spring JPA 底层为hibernate
			
	##Spring整合Junit单元测试
		
		###junit4
			@RunWith(SpringJunit4ClassRunner.class)
			@ContextConfiguration(locations = "classpath:bean.xml")
			public Class Test(){
			@Autowire
			UserService userService;
		###junit5
		|-一种方法
			@ExtendWith(SpringExtendsion.class)
			@ContextConfiguration(locations="classpath:bean.xml")
		|- 第二种
			@SpringJUnitConfig(locations ="classpath:Bean.xml")
		
#SpringMVC
	spring将已有的mvc设计模式设计成视图层框架
	Spring也可以视为将Servlet封装
	流程:
	1. DispatcherServlet(转发器)
	2. 控制器
	3. 业务层
	4. 模型层
	5. 数据库
	##简单流程:
		|- 导入依赖
		|- 在web.xml添加 DispatcherServlet
			*url-pattren的写法:
			|- /*
			拦截所有资源(css,js,image,map3,jsp)
			|- /
			拦截所有请求,放行jsp,拦截css,js,image,需要手动放行
			|- *.do
			放行所有请求,不支持resultful格式
		|- 配置springmvc的核心配置文件	
			1. 默认配置在WEB-INF文件夹下,并且命名规则为 xxx-servlet
			2. 添加init-param 名字必须是:contextConfigLocation
			需要创建三个bean
				|- 处理器映射器
				<bean id="handlerMapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
				|- 处理器适配器
				<bean id="handlerAdapter" class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
				|- 视图解析器
				<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver"/>
		|- 创建controller 类,实现controller接口,重写方法
		public class MVCController implements Controller {

			@Nullable
			@Override
			public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
				ModelAndView modelAndView = new ModelAndView();
				modelAndView.setViewName("Main");
				return modelAndView;
			}
		}
	##
		用户在发送请求时的地址
		localhost:8080/SpringMVC_Day01/hello
		地址会解析为三部分
		域名:		localhost
		站点:		SpringMVC_Day01
		控制器名: 	hello	
		
	##注解开发
		|- 在bean.xml上配置信息
			全局扫描器
			<context:component-scan base-package="com.ssl.controller/>
			处理器映射器和处理器适配器
			<mvc:annotation-driven/>
			给静态资源放行
			<mvc:default-servlet-handler/>
			配置视图解析器
			<bean id="viewResolver" class="org.springframework.web.view.InternalResourceViewResolver" />
		|- 在web.xml上配置servlet信息,
			***init-param的名字必须是contextConfigLocation
			<servlet>
				<servlet-name>springmvc</servlet-name>
				<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
				<init-param>
					<param-name>contextConfigLocation</param-name>
					<param-value>classpath:springmvc.xml</param-value>
				</init-param>
			</servlet>
			<servlet-mapping>
				<servlet-name>springmvc</servlet-name>
				<url-pattern>/*</url-pattern>
			</servlet-mapping>
		
		Resultful请求方式:
		普通方式:
		
		Resultful:
		url-pattern: *.do 不支持resultful格式
			<a href="/hello/admin" >切换</a>
			@RequestMapping("/hello/{a}/{b}")
			public String haha(@PathVariable("a") String uname
							   @PathVariable("b") String age ){
				System.out.println(uname);
				return "Main";
			}
		默认支持post和get,但是delete和put需要在web.xml中添加额外的备注
			<filter>
				<filter-name>hiddenHttpMethodFilter</filter-name>
				<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
			</filter>
			<filter-mapping>
				<filter-name>hiddenHttpMethodFilter</filter-name>
				<url-pattern>/*</url-pattern>
			</filter-mapping>
		在网页中使用时
			<form action="" method="post">
				<input type="hidden" name="_method" value="DELETE">
			</form>
		|- @RequestParam注解的使用
			应用场景当页面的变量名与入参的变量名不一致时
			需要手动映射@RequestParam("username")
			在jsp文件中使用requestSope.user
		|- @SessionAttributes(value={"user"})
			使用前要先存储Model,才可以使用
			在jsp文件中使用sessionScope.user
		
		|- 注解
			|- @RequestMapping(value="",method=RequestMethod.DELETE)
				用来映射url地址
			|- @PathVariable Restful
			|- @RequestParam("")
			|- @SessionAttributes("user",""),types=(User.class)
			
			|- @ModelAttribute注解
				|- 放在无参方法,会优先执行
				|- 放在有返回值的方法上,这个返回值就是实体
				|- 放在方法的入参中,作用属实接受之前的model对象数据
				|- 和@RequestMapping连用,这个方法的返回值就不走视图解析器
			|- @RequestHeader("String value") String str
				获取请求头中的信息,赋值给str
			|- @CookieValue("String value") String str
				value为在请求头中Cookie的名字
			
		|- 转发和重定向
			|- 转发
				|- 需要注释掉视图解析器
					return"/index.jsp"
				|- 
					return "forword:/WEB-INF/jsp/Main.jsp";
			|- 重定向
			return "redirect://jsp/Main.jsp";
				
	##spring处理json
		|- json对象
			var json = {"uname":value}
			json.uname;
		|- json数组
			var jsons = [{"uname":value},{"uname":value},{"uname":value}]
			用for循环遍历
		###处理json的注解
		|- @RequestBody
			处理不好时会出现错误415
			只接受json格式的对象
			在js方法中设置
			var a = stringify(form);
			将js对象转为json格式
			$.ajax(){
			url:"json",
			type:"post",
			data:"json",
			contentType:"application/json,
			dataType:"json"
			success:function(data){}
			}
			使用这个注解发送json格式时,需要将js对象转换为json字符串
		|- @ResponseBody
			@RequestMapping("/json")
			@ResponseBody
			public User testJson(){
			return "main";
			}
			返回一个json对象而不是返回一个视图解析对象
		
	##图片上传
	|- 在全局配置文件中配置文件上传转换器
		<bean id="multipartResolver"  class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
			<property name="defaultEncoding" value="UTF-8"/>
		</bean>
    </bean>
	|- form表单写法
		<form action="" method="" enctype="multipart/form-data">
			<input type="file" name="picture">
		</form>
	|- Controller写法
		@RequestMapping("")
		public String img(MultipartFile pic,HttpSession session,Model model){
			pic.getName();
			pic.getOriginalFileName();
			pic.getSize();
			pic.getInputStream();
			InputStream ips = pic.getInputStream();
			String path = session.getServletContext().geteRealPath("image")+File.separator+pic.getOriginalFileName();
			File file = new File(path);
			OutputStream ops = new FileOutputStream(file);
			byte[] bytes = new byte[ips.available()]
			ips.read(bytes)
			ops.write(bytes);
			ips.close();
			ops.close();
			return "";
		}
		
	|- 图片回显
		|- 在网页中
			<img alt="" src="">
			<script>
				$(function(){
					$("button").click(function(){
						var a = $("input[name='pic']").val();
						var form = $("#for").serialize();
						$.ajax({
							url:"upload",
							type:"post",
							data:form,
							success:function(data){
									 
									}
						})
					})
					
				})
			</script>
	##过滤器与拦截器
		过滤器在dispatcher里,拦截器在dispatcher外
		
	##SSM的工程
		|- 导包(24)个
		|- 配置spring&springmvc
		|- 
		
		
		
		
		
		
		
		
		
		
		