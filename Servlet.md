# Servlet
###Servlet
如果实现Servlet接口，需要重写5个方法，也就是Servlet的生命周期方法。方法分别为：

```java
  public void service(ServletRequest req, ServletResponse res)
			throws ServletException, IOException {
		res.getWriter().write("hello demo1...");
	}
	public void init(ServletConfig config) throws ServletException {
		
	}
	public ServletConfig getServletConfig() {
		return null;
	}
	public String getServletInfo() {
		return null;
	}
	public void destroy() {
		
	}
```
如果不使用Servlet创建类的话，需要手动配置web.xml文件：

```xml
<!-- 先配置Servlet信息 -->
<servlet>
	<!-- 配置Servlet名称，名称必须唯一 -->
	servlet-name>ServletDemo1</servlet-name>
	<!-- 配置Servlet的完全路径（包名+类名） -->
	<servlet-class>cn.yufei.servlet.ServletDemo1</servlet-class>
	</servlet>
				
	<!-- 配置Servlet映射（访问路径） -->
	<servlet-mapping>
	<!-- 配置Servlet名称，和上面的名称必须相同 -->
	<servlet-name>ServletDemo1</servlet-name>
	<!-- 配置虚拟路径（访问路径） -->
	<url-pattern>/demo1</url-pattern>
	</servlet-mapping>
```

###Servlet的运行过程
![jpg](http://7xqmjb.com1.z0.glb.clouddn.com/20170118148470558129128.jpg?imageView2/0/format/jpg)
步骤如下：
1. web服务器首先检查是否已经装载并创建Servlet的实例对象，如果创建了，则执行第四部，不然执行第二部
2. 装载并创建该Servlet的一个实例对象
3. 调用Servlet实例对象的init()方法
4. 创建一个用于封装HTTP请求消息的HttpServletRequest对象和一个代表HTTP响应消息的HttpServletResponse对象，然后调用Servlet的service()方法将请求个响应对象作为参数传递出去
5. WEB应用程序被停止或重新启动之前，Servlet引擎将卸载Servlet，并在卸载之前调用Servlet的destroy()方法。


```java
1）通过映射找到到servlet-class的内容，字符串： gz.itcast.a_servlet.FirstServlet
			2）通过反射构造FirstServlet对象
					2.1 得到字节码对象
					Class clazz = class.forName("gz.itcast.a_servlet.FirstServlet");
					2.2 调用无参数的构造方法来构造对象
					Object obj = clazz.newInstance();     ---1.servlet的构造方法被调用
			3）创建ServletConfig对象，通过反射调用init方法
					3.1 得到方法对象
					Method m = clazz.getDeclareMethod("init",ServletConfig.class);
					3.2 调用方法
					m.invoke(obj,config);             --2.servlet的init方法被调用
			4）创建request，response对象，通过反射调用service方法
					4.1 得到方法对象
					Methodm m =clazz.getDeclareMethod("service",HttpServletRequest.class,HttpServletResponse.class);
					4.2 调用方法
					m.invoke(obj,request,response);  --3.servlet的service方法被调用
			5）当tomcat服务器停止或web应用重新部署，通过反射调用destroy方法
					5.1 得到方法对象
					Method m = clazz.getDeclareMethod("destroy",null);
					5.2 调用方法
					m.invoke(obj,null);            --4.servlet的destroy方法被调用
```
![](http://p1.bqimg.com/567571/300696fc3172dd12.jpg)

###Servlet生命周期
对象从创建到销毁经历的过程，称之为对象的生命周期。
在对象生命周期过程中，在特定时刻肯定会执行一些特定的方法，这些方法称之为与生命周期相关的方法。
例如，人从出生到死亡经历的过程，为人的一个生命周期，在人生命周期过程中，必定有一些与生命周期息息相关的方法，例如吃饭、上学、结婚等，这些方法在人生命周期过程中某个特定时刻必定会执行，所以这些方法是人生命周期相关的方法。但不是说对象中的所有方法都与生命周期相关，例如人自杀，这个方法不是在生命周期中必定会执行。 

生命周期：实例被创建，对外提供服务，销毁。

1. Servlet被创建后，然后调用init方法进行初始化
 void init(ServletConfig config)只会调用一次
2. 从客户端发送所有的请求是service方法进行处理的
void service(ServletRequest req, ServletResponse res)，请求多少次就调用多少次
3. 从服务器中移除服务，调用destroy方法
void destroy()

Servlet的生命周期：第一次请求的时候，Servlet实例被创建，立即调用init方法进行初始化。实例通过service方法提供服务。服务器关闭或者移除服务时，调用destroy方法进行销毁
![jpg](http://7xqmjb.com1.z0.glb.clouddn.com/20170118148470609686503.jpg?imageView2/0/format/jpg)

###Servlet接口实现类
有两个默认实现类：GenericServlet和HttpServlet
GenericServlet实现了Servlet接口，HttpServlet继承自GenericServlet

`HttpServlet指能够处理HTTP请求的servlet，它在原有Servlet接口上添加了一些与HTTP协议处理方法，它比Servlet接口的功能更为强大。因此开发人员在编写Servlet时，通常应继承这个类，而避免直接去实现Servlet接口。
HttpServlet在实现Servlet接口时，覆写了service方法，该方法体内的代码会自动判断用户的请求方式，如为GET请求，则调用HttpServlet的doGet方法，如为Post请求，则调用doPost方法。因此，开发人员在编写Servlet时，通常只需要覆写doGet或doPost方法，而不要去覆写service方法。`

针对客户端的多次Servlet请求，通常情况下，服务器只会创建一个Servlet实例对象，也就是说Servlet实例对象一旦创建，它就会驻留在内存中，为后续的其它请求服务，直至web容器退出，servlet实例对象才会销毁。

![jpg](http://7xqmjb.com1.z0.glb.clouddn.com/20170118148470645645788.jpg?imageView2/0/format/jpg)
![jpg](http://7xqmjb.com1.z0.glb.clouddn.com/20170118148470649550861.jpg?imageView2/0/format/jpg)

###init() 和 init(ServletConfig) 关系
 init(ServletConfig) 是Servlet生命周期的方法
 GenericServlet内部 覆盖了 init(ServletConfig)  ,在 有参数init方法中调用 无参数init

###Servlet自动加载
第一次访问servlet的时候创建servlet对象。如果servlet的构造方法或init方法中执行了比较多的逻辑代码，那么导致用户第一次访问sevrlet的时候比较慢。
改变servlet创建对象的时机： 提前到加载web应用的时候！！！
在<servlet>标签下
				<load-on-startup>3</load-on-startup>
值是正整数，如果值越小，优先级越高
###Servlet缺省路径
		servlet的缺省路径（<url-pattern>/</url-pattern>）是在tomcat服务器内置的一个路径。该路径对应的是一个DefaultServlet（缺省Servlet）。这个缺省的Servlet的作用是用于解析web应用的静态资源文件。

		问题： URL输入http://localhost:8080/day10/index.html 如何读取文件？？？？

		1. 到当前day10应用下的web.xml文件查找是否有匹配的url-pattern。
		2. 如果没有匹配的url-pattern，则交给tomcat的内置的DefaultServlet处理
		3. DefaultServlet程序到day10应用的根目录下查找是存在一个名称为index.html的静态文件。
		4. 如果找到该文件，则读取该文件内容，返回给浏览器。
		5. 如果找不到该文件，则返回404错误页面。

结论： 先找动态资源，再找静态资源。


###Servlet的线程安全问题
当多个客户端并发访问同一个Servlet时，web服务器会为每一个客户端的访问请求创建一个线程，并在这个线程上调用Servlet的service方法，因此service方法内如果访问了同一个资源的话，就有可能引发线程安全问题。
如果某个Servlet实现了SingleThreadModel接口，那么Servlet引擎将以单线程模式来调用其service方法。
SingleThreadModel接口中没有定义任何方法，只要在Servlet类的定义中增加实现SingleThreadModel接口的声明即可。  
对于实现了SingleThreadModel接口的Servlet，Servlet引擎仍然支持对该Servlet的多线程并发访问，其采用的方式是产生多个Servlet实例对象，并发的每个线程分别调用一个独立的Servlet实例对象。
实现SingleThreadModel接口并不能真正解决Servlet的线程安全问题，因为Servlet引擎会创建多个Servlet实例对象，而真正意义上解决多线程安全问题是指一个Servlet实例对象被多个线程同时调用的问题。事实上，在Servlet API 2.4中，已经将SingleThreadModel标记为Deprecated（过时的）。   

###ServletCongig
在Servlet的配置文件中，可以使用一个或多个<init-param>标签为servlet配置一些初始化参数。
当servlet配置了初始化参数后，web容器在创建servlet实例对象时，会自动将这些初始化参数封装到ServletConfig对象中，并在调用servlet的init方法时，将ServletConfig对象传递给servlet。进而，程序员通过ServletConfig对象就可以得到当前servlet的初始化参数信息。

###ServletContext
我们可以通过两种方法获取

```java
//ServletContext context = this.getServletConfig().getServletContext();
        ServletContext context = this.getServletContext();
```

```java
public void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        //1.得到ServletContext对象
        //ServletContext context = this.getServletConfig().getServletContext();
        ServletContext context = this.getServletContext(); //（推荐使用）
        
        
        //2.得到web应用路径  /test3
        /**
         * web应用路径：部署到tomcat服务器上运行的web应用名称
         */
        String contextPath = context.getContextPath();
        
        System.out.println(contextPath);
        
        
        /**
         * 案例：应用到请求重定向
         */
        response.sendRedirect(contextPath+"/index.html");
    }
```

获得servletContext参数

```java
Enumeration<String> enumeration = this.getServletContext().getInitParameterNames();
		while (enumeration.hasMoreElements()) {
			String string = (String) enumeration.nextElement();
			System.out.println(this.getServletContext().getInitParameter(string));
		}
```

保存数据


```java
public class ContextDemo3 extends HttpServlet {
    
    public void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        //1.得到域对象
        ServletContext context = this.getServletContext();
        
        //2.把数据保存到域对象中
        //context.setAttribute("name", "eric");
        context.setAttribute("student", new Student("jacky",20));
        System.out.println("保存成功");
    }
    
}


class Student{
    private String name;
    private int age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public Student(String name, int age) {
        super();
        this.name = name;
        this.age = age;
    }
    @Override
    public String toString() {
        return "Student [age=" + age + ", name=" + name + "]";
    }
    
}
```
从域对象获取数据
```java
//1.得到域对象
        ServletContext context = this.getServletContext();
        
        //2.从域对象中取出数据
        //String name = (String)context.getAttribute("name");
        Student student = (Student)context.getAttribute("student");
        //System.out.println("name="+name);
        
        System.out.println(student);
```
转发

```java
response.getWriter().append("Served at: ").append(request.getContextPath());
//		request.setCharacterEncoding("utf-8");
//		request.setAttribute("name", "任玉飞");
		this.getServletContext().getRequestDispatcher("/demo1").forward(request, response);
```
从request域对象中获取数据

```java
String name = (String)request.getAttribute("name");
        System.out.println("name="+name);
```

###转发和重定向的区别

```
重定向和转发有一个重要的不同：当使用转发时，JSP容器将使用一个内部的方法来调用目标页面，新的页面继续处理同一个请求，而浏览器将不会知道这个过程。 与之相反，重定向方式的含义是第一个页面通知浏览器发送一个新的页面请求。因为，当你使用重定向时，浏览器中所显示的URL会变成新页面的URL, 而当使用转发时，该URL会保持不变。重定向的速度比转发慢，因为浏览器还得发出一个新的请求。同时，由于重定向方式产生了一个新的请求，所以经过一次重定向后，request内的对象将无法使用。
          怎么选择是重定向还是转发呢？通常情况下转发更快，而且能保持request内的对象，所以他是第一选择。但是由于在转发之后，浏览器中URL仍然指向开始页面，此时如果重载当前页面，开始页面将会被重新调用。如果你不想看到这样的情况，则选择转发。 
转发和重定向的区别 
不要仅仅为了把变量传到下一个页面而使用session作用域，那会无故增大变量的作用域，转发也许可以帮助你解决这个问题。
重定向：以前的request中存放的变量全部失效，并进入一个新的request作用域。
转发：以前的request中存放的变量不会失效，就像把两个页面拼到了一起。
```


