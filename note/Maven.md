#Maven
	是项目构建和依赖管理的工具,构建不是创建,是组合
	相似的是Ant蚂蚁,但是妈以只有项目构建的功能,几乎不用,产生的是:ant.jar
	##目录结构
	conf --> settings.xml 
	##maven的依赖仓库
		maven的依赖管理有三种
		|- 中央仓库
		|- 本地仓库
		|- 私服
		maven默认仓库地址
			c:\user\.m2
		更改方法
			在conf文件夹下的settings下面的
			<localRepository>D:\学习软件\mavenloacl</localRepository>
			更改中间地址
	##maven工程的目录结构
			|- Project
				|- src
					|- main
						|- java
							存放核心代码
						|- resources
							所有的配置文件,***.xml
					|- test
						|- java
							用来测试的位置
			|- pom.xml
				maven的核心配置文件
	##maven创建工程会有三种形式
		|- jar Java
		|- war Web工程
		|- pom 继承,聚合	
							
	##项目编译命令
		|- clean	清理
		|- compile	编译 
		|- test		测试
		|- report	报告
		|- package 	打包
		|- install	安装
		|- deploy	部署
							
	##scope标签
		类似于作用域
		|- compile
			将主程序和测试包的内容进行编译
		|- test
			只将测试包的内容编译
		|- provided
			
	##maven的继承与聚合
		|- 继承的作用
			可以做版本的依赖传递,可以更好的控制依赖
			|- 防止找不到父工程的pom.xml
				<relativePath>../Parent/pom.xml</relativePath>
		|- 聚合
			当工程继承关系搭建完毕,需要通过命令来维护maven项目
			有继承关系后,子工程是不可以自己执行maven命令				
							
							
							
							
							
							
							
							
							
							
							
							
							