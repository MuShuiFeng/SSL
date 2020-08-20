#Servlet应用
	
	##工程顺序
	1. 创建数据库
	2. 创建工程
	3. 配置tomcat服务器
		window,show view,servers,找自己
	4. 创建实体类,DAO层,Service层,
	
	##servlet体系
		|- Servlet接口
			首先该类会重写很多无用的方法
			|- GenericServlet
				继承这个类重写servlet方法,但是这个service方法无法处理Http的请求
				|- HttpServlet
					这个类可以处理Http的请求
		###方法
		|- Servlet接口常用方法
			init(ServletConfig config);
				初始化
			service(HttpServletRequest req, HttpServletResponse resp);
				接受请求
			destory();
				销毁
			getServletInfo();
				获取servlet的基本信息
			getServletConfig();
				获取servlet的config对象
		|- GenericServlet接口常用方法
			getServletName();
				获取servlet的名字
			getServletContext();
				获取servlet对象
			getInitParameter;
				获得初始化参数
			getInitParameterNames();
				获取多个初始化参数
		|- HttpServlet接口常用方法
			doPost();
				插入方法
			doGet();
				查询方法
			doDelete();
				删除方法(form表单不支持)
			doPut();
				修改方法(from表单不支持)
			getSession();
				
	##Servlet生命周期
		1. servlet 调用init方法
		2. 调用service
		3. 调用destory
		4. javm进行垃圾回收
	##Servlet原理
		1. tomcat服务器启动时,扫描web.xml
		2. 浏览器发送请求时,会将servlet进行初始化操作,进行init方法
		3. 
	##ServletRequest
		|- HttpServletRequest的应用
			request.getParamter(str);
				获取页面携带的参数
			request.getParamterMap();
				获取一个map集合
			request.getParamterValues(str);
				根据名字获取请求中的所有名字
			request.getParamterNames();
				获取请求中的所有名称
	##ServletResponse
		|- HttpServletResponse
		response.sendRedirect(String location);
			重定向
			使用重定向时,浏览器的地址会发生变化
			因为重定向是两次请求
			是先将数据传输到tomcat然后进入servlet,接着将数据回弹到浏览器
			一般用与数据库的插入修改删除
		response.getWriter();
			根据response对象可以获得一个打印流
		response.setHeader("方法","时间;网页");
			在制定延迟时间后通过该方法回到目标网页
			
	##Servlet中的域对象
		 域:区域,在代码中是作为域对象存在,是一个抽象的空间,作用是用来存储数据
		|- request
			本身也是一个域对象,但是生命周期短
				/方法:
				request.setAttribute("域名",数据);
					像request对象存储数据
				request.getAttribute("域名");
					通过request的域名,来获得数据
				request.removeAttribute("域名");
					从tomcat中删除这个域名
				
				request....forword(request,response);
					转发,将数据转发到tomcat服务器后转发到浏览器,但必须获得一个RequestDispatcher,
					/例:
					req.getRequestDispatcher("html/MainPage.html").forward(req, resp);
					/转发与重定向的区别是:
						转发是一次请求,发生在服务器内部的相互转换功能,可以携带参数,一般情况下用于查询
						重定向是两次请求,当客户端发送请求到服务器时,服务其接收到请求后,返回到客户端,重新创建一个新的请求,去请求目标地址,一般用于增加,删除,修改
		|- seesion
			是一个会话,也是域对象,但是生命周期长,与cookie对象连用,基本用于记住用户名和密码功能
			session是独家占用客户端的
			
			获取session:
				HttpSession session = request.getSession();
			
			方法:
				session.setAttribute("域名",数据);
				session.getAttribute("域名");
				session.setMaxInactiveInterval(int interval);
					设置session的有效时间,interval的单位是秒
				seesion.removeAttribute(String name);
					移除这个session会话
					
			session的消失方法		
				1. 设置session的有效时间
					setMaxInactiveInterval(int interval);
				2. invalidate();
					注销request获取的session
				3. 在web.xml中添加
					<session-config>
						<session-timeout>30(分钟)
		|- servletContext
			域对象,属于全局
		
			多个servlet可以共享serletContext中的属性
			方法:
				context.setAttribute("域名",值);
				context.getAttribute("域名");
				
		|- servletConfig
			代表当前servlet的基本信息(个体servlet),使用config对象可以获取servlertContext
			每一个servlet创建时都会创建一个servletConfig
			方法:
	##jsp
		java server page(Java服务页面)
		属于Java和html的结合体,但是jsp编译后是一个servlet,而不是html
		|- jsp指令
			|- page指令
			代表当前页面,设置当前页面的基本信息,可以设置字符集,引入Java类,session是否可用,EL表达式是否可用
			/例:
				<%@ page import="#" isELIgnored="true" session="true" language="java"@%>
			|- include
			<%@ include file="#" @%>
				为了包含其他页面,页面整合使用
			|- taglib
				<%@ taglib prefix="前缀" uri="路径" @%>
				用来引入jstl,前缀为jstl标签的前缀
		
		|- EL表达式
			${}
			从tomcat服务器中获得域名所代表的值
			例:
				${userName}
			*不用域中域对象名称相同
			${sessionScope.userName}
			${requestScope.userName}
			${applicationScope.userName}
			
		
		|- 动作元素
			作用:
				只能在请求处理阶段起作用
			|- <jsp:include page="包含的页面"  >
				用来包含页面,
				flush是是否刷新缓存
		
		|- 小功能
			|- 脚本
				<% 用于添加Java代码 %>
				例:
					<% for(,,){ %> 需要运行的代码 <% } %>
			|- 声明
				<%! 声明一个变量 %>
				例:
					<%! int i = 3; %>
			|- 表达式
				<%= %>
				例:
					<%= Date date = new Date(); %>
			
			
		|- 内置对象
			|- respect
				HttpServletRequest
			|- session
				HttpSession
			|- response
				HttpServletResponse
			|- application
				HttpApplication
			|- out
				输出
			|- pageContext
				当前页面的上下文
			|- page
				this 代表当前对象
			|- exception
				异常
			|- config
				代表servletConfig对象
				
	##JSTL标签库
		jsp standard tag lib 
		jsp的标准标签库
		五个类别
		|- 核心标签库 core
			所以标签都是用 c 开头
			常用:
				<c:forEach item="" var="">
				<c:if test="true|false">
					就是if
				/例:
					<c:if test="${gender==1}">
					男
					</c:if>
				<c:forTokens="">
				
			例:
				<c:#>
		|- 格式化标签库 fmt
			<fmt:formatDate value="${user.birth}" pattern="yyyy-MM-dd" var="date">
			
			<fmt:parseDate value="str" partten="yyyy-MM-dd" var="date">
		|- jstl函数 fn
			必须用在el表达式中
			fn:length("str")
			fn:contains("","")
			fn:endWith()
			fn:split()
			fn:trim()
			fn:substring()
			/例:
				${fn:length("abc")}
		|- sql标签
		|- xml标签

#ajax

	##ajax的数据类型
		url:"",
		type:"",
		data:"",
		contentType:"application/json,
		dataType:""
		success:function(data){}
	##ajax开发步骤
	1. 获取引擎对象(XMLHttpRequest)
		var ajax = new XMLHttpRequest();
	2. 建立连接
		open(method,url,async)
			.open("post|get","userServlet","true|false");
				选择get或者post方法,与指定servlet连接,可以选择异步(true)或 同步请求(false)
				*如果是post方法,后面必须接send(String);
				*如果是get方法,后面必须是send();
	3. 发送数据
		.send(String);
			只能用于post
	4. onreadystatrchange
	##ajax两个属性
		responseText 返回字符串
		
		responseXML  返回xml
	##事件
		.onreadystatechange=function(){}
		在浏览器响应完毕后回调的函数
			存储函数,每当readyState属性发生变化时就会触发onreadystatechange事件
		.readyState = 0|1|2|3|4;
			存有XMLHttpRequest的状态,从0~4发生变化
			0:请求非初始化
			1:服务器连接已建立
			2:请求已接收
			3:请求处理中
			4:请求已完成,且响应已就绪
		.status = 200|404
	##json
		一种数据格式,类似与xml,比xml更小更快,更容易解析
		格式:
			{key,value}
		/例:
			var json = {cName:cName}
	##jQuery中的ajax
	*.serialize();
	序列化,将选中的标签序列化
	$.ajax(url,[setting]);
		/例:
			$.ajax({
					type:"GET",  //get方法
					url:target,  //目标地址
					dataType:"text",  //数据返回类型
					data:{"mark":"product"},  //给mark赋值,发送到服务器端
					success:function(data){
						$("#haha").load(data);	//数据返回后执行函数
					}
				});
	load(url,[data],[callback]);
	
	$.get(url,[data],[fn],[type]);
	$.post(url,[fn]);
	$.getJSON(url,[data],fn);
		只能接受json格式的数据
	$.getScript(url,[callback]);
	$.post(url,[data],[fn],[type]);
#Bootstrap框架
	基于jquery,在使用时必须引入jquery
	##框架引入方式
	1. 在线引入
		<!-- 最新版本的 Bootstrap 核心 CSS 文件 -->
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

		<!-- 可选的 Bootstrap 主题文件（一般不用引入） -->
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">

		<!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
		<script src="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
	2. 静态引入
		
#项目中发送请求,查询数据
	1. 普通的href,form提交,window.location.href=""
		到达servlet后,点击导航栏,就是用session,request存储页面
		前台页面用jsp跳转
		<jsp:include page=>
	2. ajax
		response.getWriter().writer();
		
##模态框修改
	<div class="modal fade bs-example-modal-lg" tabindex="-1" role="dialog" 	aria-labelledby="myLargeModalLabel">
		<div class="modal-dialog modal-lg" role="document">
			<div class="modal-content">
			...
			</div>
		</div>
	</div>

#分页
	分页有三种
		1. 假分页
			list接口下的方法
			sublist(start,end);
			
		2. limit分页
			页大小:pageSize
				自己设置页大小
			总记录数:totalCount
				总记录数=select count(*) from xxx;
			总页数:totalPage
				总页数=totalCount/pageSize
			当前页:currentPage
				当前页,
			页码:pageNo
				用来计算开始行
			数据
			int startRow = (pageNo-1)*pageSize;
			
			总页数 = 总记录数除页数
		3. hibernate框架的加载式分页
		
#MVC设计模式
	
	##数据库连接池
		database connective pool
	1. DBCP Database connective pool
	2. C3P0
	3. Druid 德鲁伊,阿里巴巴自行研发的数据库连接池
		方法:
			setMaxActive(int i);
				最大连接数
			setMinIdle(int i);
				最小连接数
			setInitialSize(int i);
				初始化个数
			setMaxWait(int i);
				最大等待时间,i为秒数,如果时间到了,没有连接上,就会提示连接超时,TimeOut异常
	功能:
	允许程序可以重复使用一个现有的数据库连接,不用重新创建了
	与正常连接数据库相似,区别在于需要创建相应的对象
	
	##过滤器
	Filter过滤器的特点是永远比servlet优先执行,并且Filter是在tomcat服务器启动的时候就启动
	多个Filter过滤器的运行顺序,是按照web.xml的先后顺序执行
	代码:
		 <filter>
			<filter-name>EncodingFilter</filter-name>
			<filter-class> </filter-class>
		</filter>
		<filter-mapping>
			<filter-name> </filter-name>
			<url-pattern>/*</url-pattern>
		</filter-mapping>

		
	##cookie和session
	HttpSession session = request.setSession();
	session存储于web服务端
	session失效时间
	1. 设置session的存活时间到期
	2. 调用session.invalidate(); 注销session
	3. 在web.xml中设置 session-config  到期时间
	4. 关闭服务器
	5. session.setMaxAge();
	
	cookie
	创建于servlet中
	*存储在浏览器端







	