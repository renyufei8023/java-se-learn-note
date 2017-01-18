# Web
###web
web服务软件作用：把本地资源共享给外部访问

web应用目录结构：
- WebRoot(MyEclipse) 根目录 可以直接浏览器访问
- WEB-INF  不可以浏览器直接访问
    - classes 存放class字节码文件
    - lib     存放jar包文件
    - web.xml web应用配置文件，配置Servlet

####Servlet技术：用java语言开发动态资源的技术
开发一个Servlet的步骤(项目开发中不常用这种方式)
1. 创建一个java类，继承HttpServlet类
2. 重写HttpServlet的doGet方法
3. 把写好的servlet程序放到tomcat服务器运行
    3.1 把编译好的servlet的class文件拷贝到tomcat的一个web应用中（web应用的WEB-INF/classes目录）
    3.2 在当前web应用中配置servlet
        
```
<!-- servlet配置 -->
									<servlet>
										<servlet-name>HelloServlet</servlet-name>
										<servlet-class>gz.itcast.HelloServlet</servlet-class>
									</servlet>
									<!--  servlet的映射配置 -->
									<servlet-mapping>
										<servlet-name> HelloServlet </servlet-name>
										<url-pattern>/hello</url-pattern>
									</servlet-mapping>
```

4. 访问Servlet，http://localhost:8080/myweb/hello

在操作的时候遇到了问题，就是继承自HttpServlet，重写doGet方法的话默认会调用super的doGet方法，这时候就回报错405，我们应该把super的给去掉，原因以及解决方法如下：

```java
1，继承自HttpServlet的Servlet没有重写对于请求和响应的处理方法：doGet或doPost等方法；默认调用父类的doGet或doPost等方法；
2，父类HttpServlet的doGet或doPost等方法覆盖了你重写的doGet或doPost等方法；
不管是1或2，父类HttpServlet的doGet或doPost等方法的默认实现是返回状态代码为405的HTTP错误表示对于指定资源的请求方法不被允许。
解决方法：
1，子类重写doGet或doPost等方法；
 2， 在你扩展的Servlert中重写doGet或doPost等方法来处理请求和响应时 不要调用父类HttpServlet的doGet或doPost等方法，即去掉super.doGet(request, response)和super.doPost(request, response);
```

另一种方法

```java
* 项目复制到webapps目录下。
					
* 通过配置虚拟路径的方式。
   * 直接修改配置文件
   * 写到tomcat/conf/server.xml
	 * 找到<Host>标签，配置到Host标签的中间
	 * 目的：通过配置，配置访问路径，准确找到c:\\bb的文件
		* <Context docBase="文件夹的真实目录" path="虚拟路径（访问路径）" ><Context>
		* <Context docBase="C:\\bb" path="/itcast" ></Context>
		* 访问：http://localhost:80/itcast
							
    * 自己编写一个配置文件（格式）（推荐使用）
		* 自定义xxx.xml结尾文件，在$CATALINA_HOME/conf/[enginename]/[hostname]/ directory.目录下。
		* 把xxx当成虚拟（访问）路径。
						
		* 在xml的文件中编写。
			* 在哪个目录下：
			* $CATALINA_HOME/conf/[enginename]/[hostname]/ directory.
			* 如果找引擎的名称和主机的名称，在server.xml中找。
			* tomcat/conf/Catalina/localhost/ccc.xml
								
				* ccc.xml的文件编写什么内容？
					* <Context docBase="C:\\cc"></Context>
				* 访问：http://localhost:80/ccc
							
			
			* 配置虚拟主机（了解）	

```

###HTTP
http协议内容分为两种：
请求-》服务器

```java
GET /day09/hello HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:35.0) Gecko/20100101 Firefox/35.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-cn,en-us;q=0.8,zh;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: keep-alive
```
响应-》浏览器

```java
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Content-Length: 24
Date: Fri, 30 Jan 2015 01:54:57 GMT

this is hello servlet!!!
```

HTTP版本
- HTTP/1.0 
链接后，只能获取一个web资源 链接后，发送请求，服务器做出响应，链接立即断开。
- HTTP/1.1 链接后，可以获取多个web资源 链接后，发送请求，服务器做出响应，链接不会立即断开。
再次发送请求，直接有一段时间没操作，自动断开。

###请求头

```java
Accept: text/html,image/*      -- 浏览器接受的数据类型
Accept-Charset: ISO-8859-1     -- 浏览器接受的编码格式
Accept-Encoding: gzip,compress  --浏览器接受的数据压缩格式
Accept-Language: en-us,zh-       --浏览器接受的语言
Host: www.it315.org:80          --（必须的）当前请求访问的目标地址（主机:端口）
If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT  --浏览器最后的缓存时间
Referer: http://www.it315.org/index.jsp      -- 当前请求来自于哪里
User-Agent: Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0)  --浏览器类型
Cookie:name=eric                     -- 浏览器保存的cookie信息
Connection: close/Keep-Alive            -- 浏览器跟服务器连接状态。close: 连接关闭  keep-alive：保存连接。
Date: Tue, 11 Jul 2000 18:23:51 GMT      -- 请求发出的时间
```

###HttpServletRequest
HttpServletRequest对象用于获取请求数据
请求行：

```java
request.getMethod();   请求方式
request.getRequetURI()   / request.getRequetURL()   请求资源
request.getProtocol()   请求http协议版本
```
请求头：

```java
request.getHeader("名称")   根据请求头获取请求值
request.getHeaderNames()    获取所有的请求头名称
```
实体内容：
```java
request.getInputStream()   获取实体内容数据
```

tomcat服务器首先会调用servlet的service方法，然后在service方法中再根据请求方式来分别调用对应的doXX方法

referer可以知道是从哪里来的。可以防止倒链

```java
 public void doGet(HttpServletRequest request, HttpServletResponse response)
			    throws ServletException, IOException {
			        response.setContentType("text/html;charset=utf-8");
			        
			        //得到referer头
			        String referer = request.getHeader("referer");
			        System.out.println("referer="+referer);
			        
			        /**
			         * 判断非法链接：
			         * 	1）直接访问的话referer=null
			         *  2）如果当前请求不是来自广告
			         */
			        if(referer==null || !referer.contains("/test2/adv.html")){
			            response.getWriter().write("当前是非法链接，请回到首页。<a href='/test2/adv.html'>首页</a>");
			        }else{
			            //正确的链接
			            response.getWriter().write("资源正在下载...");
			        }
			        
			    }
```
`request.setCharacterEncoding("utf-8");`该方法只能对请求实体内容的数据编码起作用。POST提交的数据在实体内容中，所以该方法对POST方法有效！GET方法的参数放在URI后面，所以对GET方式无效！！！


```java
public void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        /**
         * 设置参数查询的编码
         * 该方法只能对请求实体内容的数据编码起作用。POST提交的数据在实体内容中，所以该方法对POST方法有效！
         * GET方法的参数放在URI后面，所以对GET方式无效！！！
         */
        request.setCharacterEncoding("utf-8");
        
        
        /*	System.out.println("GET方式");
         //接收GET方式提交的参数
         String value = request.getQueryString();
         System.out.println(value);*/
        
        
        /**
         * 统一方便地获取请求参数的方法
         */
        System.out.println(request.getMethod()+"方式");
        //getParameter(name): 根据参数名得到参数值(只能获取一个值的参数)
        String name = request.getParameter("name");
        
        /**
         * 手动重新解码(iso-8859-1 字符串-> utf-8 字符串)
         */
        /*if("GET".equals(request.getMethod())){
         name = new String(name.getBytes("iso-8859-1"),"utf-8");
         }*/
        
        String password = request.getParameter("password");
        
        /*if("GET".equals(request.getMethod())){
         password = new String(password.getBytes("iso-8859-1"),"utf-8");
         }*/
        
        System.out.println(name+"="+password);
        
        System.out.println("=============================");
        Enumeration<String> enums = request.getParameterNames();
        while( enums.hasMoreElements() ){
            String paramName = enums.nextElement();
            
            //如果参数名是hobit，则调用getParameterValues
            if("hobit".equals(paramName)){
                /**
                 * getParameterValues(name): 根据参数名获取参数值（可以获取多个值的同名参数）
                 */
                System.out.println(paramName+":");
                String[] hobits = request.getParameterValues("hobit");
                for(String h: hobits){
                    /*	if("GET".equals(request.getMethod())){
                     h = new String(h.getBytes("iso-8859-1"),"utf-8");
                     }*/
                    System.out.print(h+",");
                }
                System.out.println();
                //如果不是hobit，则调用getParameter
            }else{
                String paramValue = request.getParameter(paramName);
                /*
                 if("GET".equals(request.getMethod())){
                 paramValue = new String(paramValue.getBytes("iso-8859-1"),"utf-8");
                 }*/
                
                System.out.println(paramName+"="+paramValue);
            }
        }
        
    }
    
    public void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        /*System.out.println("POST方式");
         InputStream in = request.getInputStream();
         byte[] buf = new byte[1024];
         int len = 0;
         while(  (len=in.read(buf))!=-1 ){
         System.out.println(new String(buf,0,len));
         }*/
        
        /**
         * 统一方便地获取请求参数的方法
         */
        /*System.out.println("POST方式");
         //根据参数名得到参数值
         String name = request.getParameter("name");
         String password = request.getParameter("password");
         System.out.println(name+"="+password);
         
         System.out.println("=============================");
         Enumeration<String> enums = request.getParameterNames();
         while( enums.hasMoreElements() ){
         String paramName = enums.nextElement();
         String paramValue = request.getParameter(paramName);
         System.out.println(paramName+"="+paramValue);
         }*/
        
        //一定调用doGet方式
        this.doGet(request, response);
    }
```

/**
     * 1)tomcat服务器把请求信息封装到HttpServletRequest对象，且把响应信息封装到HttpServletResponse
     * 2）tomcat服务器调用doGet方法，传入request，和response对象
     */
    public void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        /**
         * 3）通过response对象改变响应信息
         */
        /**
         * 3.1 响应行
         */
        //response.setStatus(404);//修改状态码
        //response.sendError(404); // 发送404的状态码+404的错误页面
        
        
        
        /**
         * 3.2 响应头
         */
        response.setHeader("server", "JBoss");
        
        
        /**
         * 3.3 实体内容(浏览器直接能够看到的内容就是实体内容)
         */
        //response.getWriter().write("01.hello world"); //字符内容。
        response.getOutputStream().write("02.hello world".getBytes());//字节内容
        
        
    }
    
    /**
     * 4)tomcat服务器把response对象的内容转换成响应格式内容，再发送给浏览器解析。
     */
    
```
####重定向

```java
public void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        /**
         * 需求： 跳转到adv.html
         * 使用请求重定向： 发送一个302状态码+location的响应头
         */
        /*response.setStatus(302);//发送一个302状态码
         response.setHeader("location", "/day09/adv.html"); //location的响应头
         */
        
        //请求重定向简化写法
        response.sendRedirect("/day09/adv.html");
        
        
    }
```
####定时刷新

```java
public void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        /**
         * 定时刷新
         * 原理：浏览器认识refresh头，得到refresh头之后重新请求当前资源
         */
        //response.setHeader("refresh", "1"); //每隔1秒刷新次页面
        
        /**
         * 隔n秒之后跳转另外的资源
         */
        response.setHeader("refresh", "3;url=/day09/adv.html");//隔3秒之后跳转到adv.html
    }
```
####content-type
resp.setContentType("html/text;charset=utf-8");相当于resp.setCharacterEncoding("utf-8");resp.setHeader("content-type", "text/html");这两句

```java
public void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        /**
         * 设置响应实体内容编码
         */
        //response.setCharacterEncoding("utf-8");
        
        /**
         * 1. 服务器发送给浏览器的数据类型和内容编码
         */
        //response.setHeader("content-type", "text/html");
        response.setContentType("text/html;charset=utf-8");//和上面代码等价。推荐使用此方法
        //response.setContentType("text/xml");
        //response.setContentType("image/jpg");
        
        
        //response.getWriter().write("<html><head><title>this is tilte</title></head><body>中国</body></html>");
        response.getOutputStream().write("<html><head><title>this is tilte</title></head><body>中国</body></html>".getBytes("utf-8"));
        
        
        /*File file = new File("e:/mm.jpg");
         *//**
            * 设置以下载方式打开文件
            *//*
               response.setHeader("Content-Disposition", "attachment; filename="+file.getName());
               *//**
                  * 下载图片
                  *//*
                     *//**
                        * 发送图片
                        *//*
                           FileInputStream in = new FileInputStream(file);
                           byte[] buf = new byte[1024];
                           int len = 0;
                           
                           //把图片内容写出到浏览器
                           while( (len=in.read(buf))!=-1 ){
                           response.getOutputStream().write(buf, 0, len);
                           }*/
    }
```

###常见的响应头

```java
Location: http://www.it315.org/index.jsp   -表示重定向的地址，该头和302的状态码一起使用。
Server:apache tomcat                 ---表示服务器的类型
Content-Encoding: gzip                 -- 表示服务器发送给浏览器的数据压缩类型
Content-Length: 80                    --表示服务器发送给浏览器的数据长度
Content-Language: zh-cn               --表示服务器支持的语言
Content-Type: text/html; charset=GB2312   --表示服务器发送给浏览器的数据类型及内容编码
Last-Modified: Tue, 11 Jul 2000 18:23:51 GMT  --表示服务器资源的最后修改时间
Refresh: 1;url=http://www.it315.org     --表示定时刷新
Content-Disposition: attachment; filename=aaa.zip --表示告诉浏览器以下载方式打开资源（下载文件时用到）
Transfer-Encoding: chunked
Set-Cookie:SS=Q0=5Lb_nQ; path=/search   --表示服务器发送给浏览器的cookie信息（会话管理用到）
Expires: -1                           --表示通知浏览器不进行缓存
Cache-Control: no-cache
Pragma: no-cache
Connection: close/Keep-Alive           --表示服务器和浏览器的连接状态。close：关闭连接 keep-alive:保存连接
```

####修改响应信息

```java
响应行： 
		response.setStatus()  设置状态码
响应头： 
		response.setHeader("name","value")  设置响应头
实体内容：
		response.getWriter().writer();
发送字符实体内容
		response.getOutputStream().writer()  发送字节实体内容 
```
![jpg](http://7xqmjb.com1.z0.glb.clouddn.com/20170118148470396331355.jpg?imageView2/0/format/jpg)
![jpg](http://7xqmjb.com1.z0.glb.clouddn.com/20170118148470398179459.jpg?imageView2/0/format/jpg)


