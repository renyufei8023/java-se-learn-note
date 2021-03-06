# JDBC
###什么是JDBC
使用java代码发送sql语句的技术，就是jdbc技术。
###使用jdbc的前提
- 需要登录数据库服务器
- 需要知道数据库的IP地址
- 需要知道数据库服务器的端口号
- 需要知道数据库的用户名
- 需要知道数据库密码
###数据库连接
1. 先确定连接数据库的URL，
2. 然后使用Driver注册驱动，
3. 注册驱动之后然后再去连接数据库

具体实现代码如下：
第一种连接方式：

```java
     //1.创建驱动程序类对象
		Driver driver = new com.mysql.jdbc.Driver(); //新版本
		//Driver driver = new org.gjt.mm.mysql.Driver(); //旧版本
		
		//设置用户名和密码
		Properties props = new Properties();
		props.setProperty("user", user);
		props.setProperty("password", password);
		
		//2.连接数据库，返回连接对象
		Connection conn = driver.connect(url, props);
		
		System.out.println(conn);
```
第二种方式：
使用驱动管理器类连接数据库(注册了两次，没必要)
```java
    Driver driver = new com.mysql.jdbc.Driver();
		//Driver driver2 = new com.oracle.jdbc.Driver();
		//1.注册驱动程序(可以注册多个驱动程序)
		DriverManager.registerDriver(driver);
		//DriverManager.registerDriver(driver2);
		
		//2.连接到具体的数据库
		Connection conn = DriverManager.getConnection(url, user, password);
		System.out.println(conn);
```
第三种方式:
推荐使用加载驱动程序类  来 注册驱动程序 
```java
//Driver driver = new com.mysql.jdbc.Driver();
		
		//通过得到字节码对象的方式加载静态代码块，从而注册驱动程序
		Class.forName("com.mysql.jdbc.Driver");
		
		//Driver driver2 = new com.oracle.jdbc.Driver();
		//1.注册驱动程序(可以注册多个驱动程序)
		//DriverManager.registerDriver(driver);
		//DriverManager.registerDriver(driver2);
		
		//2.连接到具体的数据库
		Connection conn = DriverManager.getConnection(url, user, password);
		System.out.println(conn);
```

###Statement的执行过程
![2017010939872Statment.png](http://7xqmjb.com1.z0.glb.clouddn.com/2017010939872Statment.png)

###JDBC工具类
通常我们在项目中可以创建一个工具类管理JDBC的工具类，这样就省去我们写一些重复代码了，在这个工具类里面我们可以提供两个方法，一个获取连接，一个关闭。
我们弄一个`Properties`，把数据库的url，用户名，密码都放到这里面，然后通过输入流把它给读取出来。读取的操作我们把它放到`static`代码块里面，这样的话就会只加载一次，
具体实现如下：

```java
private static String url = null;
	private static String user = null;
	private static String password = null;
	private static String driverClass = null;
	
	/**
	 * 静态代码块中（只加载一次）
	 */
	static{
		try {
			//读取db.properties文件
			Properties props = new Properties();
			/**
			 *  . 代表java命令运行的目录
			 *  在java项目下，. java命令的运行目录从项目的根目录开始
			 *  在web项目下，  . java命令的而运行目录从tomcat/bin目录开始
			 *  所以不能使用点.
			 */
			//FileInputStream in = new FileInputStream("./src/db.properties");
			
			/**
			 * 使用类路径的读取方式
			 *  / : 斜杠表示classpath的根目录
			 *     在java项目下，classpath的根目录从bin目录开始
			 *     在web项目下，classpath的根目录从WEB-INF/classes目录开始
			 */
			InputStream in = JdbcUtil.class.getResourceAsStream("/db.properties");
			
			//加载文件
			props.load(in);
			//读取信息
			url = props.getProperty("url");
			user = props.getProperty("user");
			password = props.getProperty("password");
			driverClass = props.getProperty("driverClass");
			
			
			//注册驱动程序
			Class.forName(driverClass);
		} catch (Exception e) {
			e.printStackTrace();
			System.out.println("驱程程序注册出错");
		}
	}

	/**
	 * 抽取获取连接对象的方法
	 */
	public static Connection getConnection(){
		try {
			Connection conn = DriverManager.getConnection(url, user, password);
			return conn;
		} catch (SQLException e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		}
	}
	
	
	/**
	 * 释放资源的方法
	 */
	public static void close(Connection conn,Statement stmt){
		if(stmt!=null){
			try {
				stmt.close();
			} catch (SQLException e) {
				e.printStackTrace();
				throw new RuntimeException(e);
			}
		}
		if(conn!=null){
			try {
				conn.close();
			} catch (SQLException e) {
				e.printStackTrace();
				throw new RuntimeException(e);
			}
		}
	}
	
	public static void close(Connection conn,Statement stmt,ResultSet rs){
		if(rs!=null)
			try {
				rs.close();
			} catch (SQLException e1) {
				e1.printStackTrace();
				throw new RuntimeException(e1);
			}
		if(stmt!=null){
			try {
				stmt.close();
			} catch (SQLException e) {
				e.printStackTrace();
				throw new RuntimeException(e);
			}
		}
		if(conn!=null){
			try {
				conn.close();
			} catch (SQLException e) {
				e.printStackTrace();
				throw new RuntimeException(e);
			}
		}
	}
```

###ResulSet

```java
表示数据库结果集的数据表，通常通过执行查询数据库的语句生成。

ResultSet 对象具有指向其当前数据行的光标。最初，光标被置于第一行之前。next 方法将光标移动到下一行；因为该方法在 ResultSet 对象没有下一行时返回 false，所以可以在 while 循环中使用它来迭代结果集。

默认的 ResultSet 对象不可更新，仅有一个向前移动的光标。因此，只能迭代它一次，并且只能按从第一行到最后一行的顺序进行。可以生成可滚动和/或可更新的 ResultSet 对象。以下代码片段（其中 con 为有效的 Connection 对象）演示了如何生成可滚动且不受其他更新影响的可更新结果集。有关其他选项，请参见 ResultSet 字段。


       Statement stmt = con.createStatement(
                                      ResultSet.TYPE_SCROLL_INSENSITIVE,
                                      ResultSet.CONCUR_UPDATABLE);
       ResultSet rs = stmt.executeQuery("SELECT a, b FROM TABLE2");
       // rs will be scrollable, will not show changes made by others,
       // and will be updatable

 
ResultSet 接口提供用于从当前行获取列值的 获取 方法（ getBoolean、 getLong 等）。可以使用列的索引编号或列的名称获取值。一般情况下，使用列索引较为高效。列从 1 开始编号。为了获得最大的可移植性，应该按从左到右的顺序读取每行中的结果集列，每列只能读取一次。
对于获取方法，JDBC 驱动程序尝试将底层数据转换为在获取方法中指定的 Java 类型，并返回适当的 Java 值。JDBC 规范有一个表，显示允许的从 SQL 类型到 ResultSet 获取方法所使用的 Java 类型的映射关系。

用作获取方法的输入的列名称不区分大小写。用列名称调用获取方法时，如果多个列具有这一名称，则返回第一个匹配列的值。在生成结果集的 SQL 查询中使用列名称时，将使用列名称选项。对于没有在查询中显式指定的列，最好使用列编号。如果使用列名称，则程序员应该注意保证名称唯一引用预期的列，这可以使用 SQL AS 子句确定。

在 JDBC 2.0 API（JavaTM 2 SDK 标准版 1.2 版）中，此接口添加了一组更新方法。关于获取方法参数的注释同样适用于更新方法的参数。

可以用以下两种方式使用更新方法：

更新当前行中的列值。在可滚动的 ResultSet 对象中，可以向前和向后移动光标，将其置于绝对位置或相对于当前行的位置。以下代码片段更新 ResultSet 对象 rs 第五行中的 NAME 列，然后使用方法 updateRow 更新导出 rs 的数据源表。

       rs.absolute(5); // moves the cursor to the fifth row of rs
       rs.updateString("NAME", "AINSWORTH"); // updates the 
          // NAME column of row 5 to be AINSWORTH
       rs.updateRow(); // updates the row in the data source

 
将列值插入到插入行中。可更新的 ResultSet 对象具有一个与其关联的特殊行，该行用作构建要插入的行的暂存区域 (staging area)。以下代码片段将光标移动到插入行，构建一个三列的行，并使用方法 insertRow 将其插入到 rs 和数据源表中。

       rs.moveToInsertRow(); // moves cursor to the insert row
       rs.updateString(1, "AINSWORTH"); // updates the 
          // first column of the insert row to be AINSWORTH
       rs.updateInt(2,35); // updates the second column to be 35
       rs.updateBoolean(3, true); // updates the third column to true
       rs.insertRow();
       rs.moveToCurrentRow();
```
###使用JDBC执行DDL语句(创建表)

```java
@Test
	public void test1(){
		Statement stmt = null;
		Connection conn = null;
		try {
			//1.驱动注册程序
			Class.forName("com.mysql.jdbc.Driver");
			
			//2.获取连接对象
			conn = DriverManager.getConnection(url, user, password);
			
			//3.创建Statement
			stmt = conn.createStatement();
			
			//4.准备sql
			String sql = "CREATE TABLE student(id INT PRIMARY KEY AUTO_INCREMENT,NAME VARCHAR(20),gender VARCHAR(2))";
			
			//5.发送sql语句，执行sql语句,得到返回结果
			int count = stmt.executeUpdate(sql);
			
			//6.输出
			System.out.println("影响了"+count+"行！");
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally{
			//7.关闭连接(顺序:后打开的先关闭)
			if(stmt!=null)
				try {
					stmt.close();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new RuntimeException(e);
				}
			if(conn!=null)
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new RuntimeException(e);
				}
		}		
	}
```

###使用JDBC执行DML语句
- 增加

```java
@Test
	public void testInsert(){
		Connection conn = null;
		Statement stmt = null;
		try {
			//通过工具类获取连接对象
			conn = JdbcUtil.getConnection();
			
			//3.创建Statement对象
			stmt = conn.createStatement();
			
			//4.sql语句
			String sql = "INSERT INTO student(NAME,gender) VALUES('李四','女')";
			
			//5.执行sql
			int count = stmt.executeUpdate(sql);
			
			System.out.println("影响了"+count+"行");
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally{
			//关闭资源
			/*if(stmt!=null)
				try {
					stmt.close();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new RuntimeException(e);
				}
			if(conn!=null)
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new RuntimeException(e);
				}*/
			JdbcUtil.close(conn, stmt);
		}
	}
```
- 修改

```java
@Test
	public void testUpdate(){
		Connection conn = null;
		Statement stmt = null;
		//模拟用户输入
		String name = "陈六";
		int id = 3;
		try {
			/*//1.注册驱动
			Class.forName("com.mysql.jdbc.Driver");
			
			//2.获取连接对象
			conn = DriverManager.getConnection(url, user, password);*/
			//通过工具类获取连接对象
			conn = JdbcUtil.getConnection();
			
			//3.创建Statement对象
			stmt = conn.createStatement();
			
			//4.sql语句
			String sql = "UPDATE student SET NAME='"+name+"' WHERE id="+id+"";
			
			System.out.println(sql);
			
			//5.执行sql
			int count = stmt.executeUpdate(sql);
			
			System.out.println("影响了"+count+"行");
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally{
			//关闭资源
			/*if(stmt!=null)
				try {
					stmt.close();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new RuntimeException(e);
				}
			if(conn!=null)
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new RuntimeException(e);
				}*/
			JdbcUtil.close(conn, stmt);
		}
	}
```
- 删除

```java
@Test
	public void testDelete(){
		Connection conn = null;
		Statement stmt = null;
		//模拟用户输入
		int id = 3;
		try {
			/*//1.注册驱动
			Class.forName("com.mysql.jdbc.Driver");
			
			//2.获取连接对象
			conn = DriverManager.getConnection(url, user, password);*/
			//通过工具类获取连接对象
			conn = JdbcUtil.getConnection();
			
			//3.创建Statement对象
			stmt = conn.createStatement();
			
			//4.sql语句
			String sql = "DELETE FROM student WHERE id="+id+"";
			
			System.out.println(sql);
			
			//5.执行sql
			int count = stmt.executeUpdate(sql);
			
			System.out.println("影响了"+count+"行");
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally{
			//关闭资源
			/*if(stmt!=null)
				try {
					stmt.close();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new RuntimeException(e);
				}
			if(conn!=null)
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
					throw new RuntimeException(e);
				}*/
			JdbcUtil.close(conn, stmt);
		}
	}
```
###使用JDBC执行DQL语句(查询操作)
我们一般都是使用`executeQuery`来获取`ResultSet`，`ResultSet`的话我们可以通过`列`或者是`字段名称`来获取数据
代码如下：

```java
@Test
	public void test1(){
		Connection conn = null;
		Statement stmt = null;
		try{
			//获取连接
			conn = JdbcUtil.getConnection();
			//创建Statement
			stmt = conn.createStatement();
			//准备sql
			String sql = "SELECT * FROM student";
			//执行sql
			ResultSet rs = stmt.executeQuery(sql);
			
			//移动光标
			/*boolean flag = rs.next();
			
			flag = rs.next();
			flag = rs.next();
			if(flag){
				//取出列值
				//索引
				int id = rs.getInt(1);
				String name = rs.getString(2);
				String gender = rs.getString(3);
				System.out.println(id+","+name+","+gender);
				
				//列名称
				int id = rs.getInt("id");
				String name = rs.getString("name");
				String gender = rs.getString("gender");
				System.out.println(id+","+name+","+gender);
			}*/
			
			//遍历结果
			while(rs.next()){
				int id = rs.getInt("id");
				String name = rs.getString("name");
				String gender = rs.getString("gender");
				System.out.println(id+","+name+","+gender);
			}
			
		}catch(Exception e){
			e.printStackTrace();
			throw new RuntimeException(e);
		}finally{
			JdbcUtil.close(conn, stmt);
		}
	}
```

###使用预编译执行sql语句
预编译执行相对于静态的安全性比较高，避免sql注入，提高了安全性，
- 增加

```java
@Test
	public void testInsert() {
		Connection conn = null;
		PreparedStatement stmt = null;
		try {
			//1.获取连接
			conn = JdbcUtil.getConnection();
			
			//2.准备预编译的sql
			String sql = "INSERT INTO student(NAME,gender) VALUES(?,?)"; //?表示一个参数的占位符
			
			//3.执行预编译sql语句(检查语法)
			stmt = conn.prepareStatement(sql);
			
			//4.设置参数值
			/**
			 * 参数一： 参数位置  从1开始
			 */
			stmt.setString(1, "李四");
			stmt.setString(2, "男");
			
			//5.发送参数，执行sql
			int count = stmt.executeUpdate();
			
			System.out.println("影响了"+count+"行");
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally {
			JdbcUtil.close(conn, stmt);
		}
	}
```
- 修改

```java
@Test
	public void testUpdate() {
		Connection conn = null;
		PreparedStatement stmt = null;
		try {
			//1.获取连接
			conn = JdbcUtil.getConnection();
			
			//2.准备预编译的sql
			String sql = "UPDATE student SET NAME=? WHERE id=?"; //?表示一个参数的占位符
			
			//3.执行预编译sql语句(检查语法)
			stmt = conn.prepareStatement(sql);
			
			//4.设置参数值
			/**
			 * 参数一： 参数位置  从1开始
			 */
			stmt.setString(1, "王五");
			stmt.setInt(2, 9);
			
			//5.发送参数，执行sql
			int count = stmt.executeUpdate();
			
			System.out.println("影响了"+count+"行");
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally {
			JdbcUtil.close(conn, stmt);
		}
	}
```
- 删除


```java
@Test
	public void testDelete() {
		Connection conn = null;
		PreparedStatement stmt = null;
		try {
			//1.获取连接
			conn = JdbcUtil.getConnection();
			
			//2.准备预编译的sql
			String sql = "DELETE FROM student WHERE id=?"; //?表示一个参数的占位符
			
			//3.执行预编译sql语句(检查语法)
			stmt = conn.prepareStatement(sql);
			
			//4.设置参数值
			/**
			 * 参数一： 参数位置  从1开始
			 */
			stmt.setInt(1, 9);
			
			//5.发送参数，执行sql
			int count = stmt.executeUpdate();
			
			System.out.println("影响了"+count+"行");
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally {
			JdbcUtil.close(conn, stmt);
		}
	}
```
- 查询

```java
@Test
	public void testQuery() {
		Connection conn = null;
		PreparedStatement stmt = null;
		ResultSet rs = null;
		try {
			//1.获取连接
			conn = JdbcUtil.getConnection();
			
			//2.准备预编译的sql
			String sql = "SELECT * FROM student"; 
			
			//3.预编译
			stmt = conn.prepareStatement(sql);
			
			//4.执行sql
			rs = stmt.executeQuery();
			
			//5.遍历rs
			while(rs.next()){
				int id = rs.getInt("id");
				String name = rs.getString("name");
				String gender = rs.getString("gender");
				System.out.println(id+","+name+","+gender);
			}
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally {
			//关闭资源
			JdbcUtil.close(conn,stmt,rs);
		}
	}
```
使用Statement实现sql注入：

```java
//模拟用户输入
	//private String name = "ericdfdfdfddfd' OR 1=1 -- ";
	private String name = "eric";
	//private String password = "123456dfdfddfdf";
	private String password = "123456";

	/**
	 * Statment存在sql被注入的风险
	 */
	@Test
	public void testByStatement(){
		Connection conn = null;
		Statement stmt = null;
		ResultSet rs = null;
		try {
			//获取连接
			conn = JdbcUtil.getConnection();
			
			//创建Statment
			stmt = conn.createStatement();
			
			//准备sql
			String sql = "SELECT * FROM users WHERE NAME='"+name+"' AND PASSWORD='"+password+"'";
			
			//执行sql
			rs = stmt.executeQuery(sql);
			
			if(rs.next()){
				//登录成功
				System.out.println("登录成功");
			}else{
				System.out.println("登录失败");
			}
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally {
			JdbcUtil.close(conn, stmt ,rs);
		}
		
	}
	
	/**
	 * PreparedStatement可以有效地防止sql被注入
	 */
	@Test
	public void testByPreparedStatement(){
		Connection conn = null;
		PreparedStatement stmt = null;
		ResultSet rs = null;
		try {
			//获取连接
			conn = JdbcUtil.getConnection();
			
			String sql = "SELECT * FROM users WHERE NAME=? AND PASSWORD=?";
			
			//预编译
			stmt = conn.prepareStatement(sql);
			
			//设置参数
			stmt.setString(1, name);
			stmt.setString(2, password);
			
			//执行sql
			rs = stmt.executeQuery();
			
			if(rs.next()){
				//登录成功
				System.out.println("登录成功");
			}else{
				System.out.println("登录失败");
			}
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally {
			JdbcUtil.close(conn, stmt ,rs);
		}
		
	}
```

###使用CablleStatement调用存储过程
```java
/**
	 * 调用带有输入参数的存储过程
	 * CALL pro_findById(4);
	 */
	@Test
	public void test1(){
		Connection conn = null;
		CallableStatement stmt = null;
		ResultSet rs = null;
		try {
			//获取连接
			conn = JdbcUtil.getConnection();
			
			//准备sql
			String sql = "CALL pro_findById(?)"; //可以执行预编译的sql
			
			//预编译
			stmt = conn.prepareCall(sql);
			
			//设置输入参数
			stmt.setInt(1, 6);
			
			//发送参数
			rs = stmt.executeQuery(); //注意： 所有调用存储过程的sql语句都是使用executeQuery方法执行！！！
			
			//遍历结果
			while(rs.next()){
				int id = rs.getInt("id");
				String name = rs.getString("name");
				String gender = rs.getString("gender");
				System.out.println(id+","+name+","+gender);
			}
			
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally {
			JdbcUtil.close(conn, stmt ,rs);
		}
	}
	
	/**
	 * 执行带有输出参数的存储过程
	 * CALL pro_findById2(5,@NAME);
	 */
	@Test
	public void test2(){
		Connection conn = null;
		CallableStatement stmt = null;
		ResultSet rs = null;
		try {
			//获取连接
			conn = JdbcUtil.getConnection();
			//准备sql
			String sql = "CALL pro_findById2(?,?)"; //第一个？是输入参数，第二个？是输出参数
			
			//预编译
			stmt = conn.prepareCall(sql);
			
			//设置输入参数
			stmt.setInt(1, 6);
			//设置输出参数(注册输出参数)
			/**
			 * 参数一： 参数位置
			 * 参数二： 存储过程中的输出参数的jdbc类型    VARCHAR(20)
			 */
			stmt.registerOutParameter(2, java.sql.Types.VARCHAR);
			
			//发送参数，执行
			stmt.executeQuery(); //结果不是返回到结果集中，而是返回到输出参数中
			
			//得到输出参数的值
			/**
			 * 索引值： 预编译sql中的输出参数的位置
			 */
			String result = stmt.getString(2); //getXX方法专门用于获取存储过程中的输出参数
			
			System.out.println(result);

		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException(e);
		} finally {
			JdbcUtil.close(conn, stmt ,rs);
		}
	}
```

