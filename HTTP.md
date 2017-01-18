# StringBulder
###StringBulder
在实际项目中，如果我们需要对String进行修改等操作比较频繁，应该使用StringBulder或者StringBuffer这两个类，这两个类用法基本一致，区别在于前者是非同步的，后者是同步的，县城安全的。

###实现原理
与String类似，他也有一个char数组，不过没有使用`final`进行修饰，所有它可以被修改。与String不同，字符数组中不一定所有位置都已经被使用，它有一个实例变量，表示数组中已经使用的字符个数，定义如下：

```java
int count；
```
StringBuilder集成自`AbstractStringBuilder`，默认的构造方法是：

```java
public StringBuilder() {
    super(16);
}
```

调用父类的构造方法，父类对应的构造方法是

```java
AbstractStringBuilder(int capacity) {
    value = new char[capacity];
}
```
new StringBuilder()这句代码，内部会创建一个长度为16的字符数组，count的默认值为0。

###append的实现

```java
public AbstractStringBuilder append(String str) {
    if (str == null) str = "null";
    int len = str.length();
    ensureCapacityInternal(count + len);
    str.getChars(0, len, value, count);
    count += len;
    return this;
}
```
append会直接拷贝字符到内部的字符数组中，如果字符数组长度不够，会进行扩展，实际使用的长度用count体现。具体来说，ensureCapacityInternal(count+len)会确保数组的长度足以容纳新添加的字符，str.getChars会拷贝新添加的字符到字符数组中，count+=len会增加实际使用的长度。

```java
private void ensureCapacityInternal(int minimumCapacity) {
    // overflow-conscious code
    if (minimumCapacity - value.length > 0)
        expandCapacity(minimumCapacity);
}
```
```java
void expandCapacity(int minimumCapacity) {
    int newCapacity = value.length * 2 + 2;
    if (newCapacity - minimumCapacity < 0)
        newCapacity = minimumCapacity;
    if (newCapacity < 0) {
        if (minimumCapacity < 0) // overflow
            throw new OutOfMemoryError();
        newCapacity = Integer.MAX_VALUE;
    }
    value = Arrays.copyOf(value, newCapacity);
}
```
扩展的逻辑是，分配一个足够长度的新数组，然后将原内容拷贝到这个新数组中，最后让内部的字符数组指向这个新数组，这个逻辑主要靠下面这句代码实现：

```java
value = Arrays.copyOf(value, newCapacity);
```
参数minimumCapacity表示需要的最小长度，需要多少分配多少不就行了吗？不行，因为那就跟String一样了，每append一次，都会进行一次内存分配，效率低下。这里的扩展策略，是跟当前长度相关的，当前长度乘以2，再加上2，如果这个长度不够最小需要的长度，才用minimumCapacity。

比如说，默认长度为16，长度不够时，会先扩展到16*2+2即34，然后扩展到34*2+2即70，然后是70*2+2即142，这是一种指数扩展策略。为什么要加2？大概是因为在原长度为0时也可以一样工作吧。

为什么要这么扩展呢？这是一种折中策略，一方面要减少内存分配的次数，另一方面也要避免空间浪费。在不知道最终需要多长的情况下，指数扩展是一种常见的策略，广泛应用于各种内存分配相关的计算机程序中。

那如果预先就知道大概需要多长呢？可以调用StringBuilder的另外一个构造方法：
```java
public StringBuilder(int capacity)
```

###toString

```java
public String toString() {
    // Create a copy, don't share the array
    return new String(value, 0, count);
}
```
基于内部数组新建了一个String，注意，这个String构造方法不会直接用value数组，而会新建一个，以保证String的不可变性。

###更多构造方法和append方法
String还有两个构造方法，分别接受String和CharSequence参数

```java
public StringBuilder(String str) {
    super(str.length() + 16);
    append(str);
}

public StringBuilder(CharSequence seq) {
    this(seq.length() + 16);
    append(seq);
}
```

额外多分配16个字符的空间，然后调用append将参数字符添加进来。

###插入
方法定义如下：
```java
public StringBuilder insert(int offset, String str)
```
实现如下：

```java
public AbstractStringBuilder insert(int offset, String str) {
    if ((offset < 0) || (offset > length()))
        throw new StringIndexOutOfBoundsException(offset);
    if (str == null)
        str = "null";
    int len = str.length();
    ensureCapacityInternal(count + len);
    System.arraycopy(value, offset, value, offset + len, count - offset);
    str.getChars(value, offset);
    count += len;
    return this;
}

```
在确保足够长度后，首先将原数组中offset开始的内容向后挪动n个位置，n为待插入字符串的长度，然后将待插入字符串拷贝进offset位置。
挪动位置调用了System.arraycopy方法

```java
public static native void arraycopy(Object src,  int  srcPos,
                                        Object dest, int destPos,
                                        int length);
```


```java
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

