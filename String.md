# String
在java中使用`"renyufei"`与`new String("renyufei")` 这两个是不相等的，因为String重写了`equals`方法，

```java
     System.out.println("-----多个引用只向同一个字符串-------");
		String s1 = "renyufei";
		String string2 = "renyufei";
		System.out.println(s1 == string2);
		
		System.out.println("-----两个内容相同。创建方式不同，是不相等的-----");
		String string3 = "renyufei";
		String string4 = new String("renyufei");
		System.out.println(string3 == string4);
		
		//s1,string2,string3在内存中是同一个对象，string4是有创建了一个新的对象
		//原因是因为String重写了equals方法，建立字符串自己的判断相同的依据。通过字符串对象的内容来判断
```
###获取字符串长度
int length()
###获取字符位置
int indexOf(ch,fromIndex);
###获取指定位置上的字符
char charAt(int)
###获取部分字符串
String substring(int start,int end);//包含头不包含尾
###获取最后面的某个字符的位置
lastIndexOf() //如果没有回返回-1
###字符串是否已指定字符串开头，结尾
boolean startsWith(string)
boolean endsWith(string)
###字符串是否包含另一个字符串
boolean contains(string);
int indexOf(string)//如果返回-1，表示不存在。
###字符串中另一个字符串出现的位置
int indexOf(string)
###将字符串中指定的字符串替换成另一个字符串
String replace(oldstring , newstring)
###字符串如何比较大小
使用compareTo()只要想让对象具备比较大小的功能只需实现Comparable接口。
###将字符串转换成一个字符数组或者字节数组
toCharArray()
getBytes()
###将字符串转成大写货小写的字符串
toUpperCase()
toLowerCase()
###将字符串按照指定的方式分割成多个字符串

```java
string = "lisi,wangwu,zhaoliu";
		String[] names = string.split(",");
		for (int i = 0; i < names.length; i++) {
			System.out.println(names[i]);
		}
```
###对字符串进行排序
思路就是选择冒泡排序，使用for循环嵌套，循环中进行元素的大小比较，满足天骄交换位置，代码如下

```java
  public static void main(String[] args) {
		String[] strings = {"abc","nba","ccav","renyufei"};
		printArray(strings);
		sortString(strings);
		printArray(strings);
	}

	private static void printArray(String[] strings) {
		// TODO Auto-generated method stub
		for (int i = 0; i < strings.length; i++) {
			System.out.println(strings[i]+" ");
		}
		System.out.println();
	}	

	private static void sortString(String[] strings) {
		// TODO Auto-generated method stub
		for (int i = 0; i < strings.length - 1; i++) {
			for (int j = i+1; j < strings.length; j++) {
				if (strings[i].compareTo(strings[j]) > 0) {
					swap(strings,i,j);
				}
			}
		}
	}

	private static void swap(String[] strings, int i, int j) {
		// TODO Auto-generated method stub
		String temp = strings[i];
		strings[i] = strings[j];
		strings[j] = temp;
	}
```

```java
 /**
     * @param args
     */
    public static void main(String[] args) {
        
        /*
         *
         * 案例二：
         * "witcasteritcasttyuiitcastodfghjitcast"有几个itcast
         *
         * 思路：
         * 1，无非就是在一个字符串中查找另一个字符串。indexOf。
         * 2，查找到第一次出现的指定字符串后，如何查找第二个呢？
         * 3，无需在从头开始，只要从第一次出现的位置+要找的字符串的长度的位置开始向后查找下一个第一次出现的位置即可。
         * 4，当返回的位置是-1时，查找结束。
         */
        String str = "witcasteritcasttyuiitcastodfghjitcast";
        String key = "itcast";
        
        int count = getKeyCount(str,key);
        System.out.println("count="+count);
        /*
         int x = str.indexOf(key,0);//从头开始找。
         System.out.println("x="+x);
         
         int y = str.indexOf(key,x+key.length());//从指定起始位开始找。
         System.out.println("y="+y);
         
         int z = str.indexOf(key,y+key.length());//从指定起始位开始找。
         System.out.println("z="+z);
         
         int a = str.indexOf(key,z+key.length());//从指定起始位开始找。
         System.out.println("a="+a);
         
         int b = str.indexOf(key,a+key.length());//从指定起始位开始找。
         System.out.println("b="+b);
         */
    }
    
    /**
     * 获取key在str中出现次数。
     * @param str
     * @param key
     * @return
     */
    public static int getKeyCount(String str, String key) {
        
        //1,定义变量。记录每一次找到的key的位置。
        int index = 0;
        //2,定义变量，记录出现的次数。
        int count = 0;
        
        //3,定义循环。只要索引到的位置不是-1，继续查找。
        while((index = str.indexOf(key,index))!=-1){
            
            //每循环一次，就要明确下一次查找的起始位置。
            index = index + key.length();
            
            //每查找一次，count自增。
            count++;
        }
        return count;
    }

```

```java
public static void main(String[] args) {
        /*
         *
         *
         * 案例三： "itcast_sh"要求，将该字符串按照长度由长到短打印出来。 itcast_sh itcast_s tcast_sh
         */
        
        String str = "itcast";
        printStringByLength(str);
        
    }
    
    public static void printStringByLength(String str) {
        
        // 1，通过分析，发现是for嵌套循环。
        for (int i = 0; i < str.length(); i++) {
            
            for (int start = 0, end = str.length() - i; end <= str.length(); start++, end++) {
                
                //根据start，end截取字符串。
                String temp = str.substring(start, end);
                System.out.println(temp);
            }
            
        }
        
    }

```
###StringBuffer

```java
 public static void main(String[] args) {
        /*
         * StringBuffer:
         * 1，是一个字符串缓冲区，其实就是一个容器。
         * 2，长度是可变，任意类型都行。注意：是将任意数据都转成字符串进行存储。
         * 3，容器对象提供很多对容器中数据的操作功能，比如：添加，删除，查找，修改。
         * 4，必须所有的数据最终变成一个字符串。
         * 和数组最大的不同就是：数组存储完可以单独操作每一个元素，每一个元素都是独立的。
         * 字符串缓冲区，所有存储的元素都被转成字符串，而且最后拼成了一个大的字符串。
         *
         * 可变长度数组的原理：新建数组，并复制数组元素到新数组中。
         */
        
        //1,创建一个字符串缓冲区对象。用于存储数据。
        StringBuffer sb = new StringBuffer();
        
        //2,添加数据。不断的添加数据后，要对缓冲区的最后的数据进行操作，必须转成字符串才可以。
        String str = sb.append(true).append("hehe").toString();
        //		sb.append("haha");
        
        //		sb.insert(2, "it");//插入
        
        //		sb.delete(1, 4);//删除
        
        //		sb.replace(1, 4, "cast");
        //		sb.setLength(2);
        System.out.println(sb);
        
        
        //		String s = "a"+5+'c';//原理就是以下这句
        //		s = new StringBuffer().append("a").append(5).append('c').toString();
        
        
        
    }

```

```java
 public static void main(String[] args) {
        
        /*
         * int[] arr = {34,12,89,68};
         * 将一个int[]中元素转成字符串  格式 [34,12,89,68]
         */
        int[] arr = {34,12,89,68};
        String str = toString_2(arr);
        System.out.println(str);
    }
    
    /**
     * 缓冲区的应用：无论多少数据，什么类型都不重要，只要最终变成字符串就可以StringBuffer这个容器。
     * @param arr
     * @return
     */
    public static String toString_2(int[] arr) {
        //1,创建缓冲区。
        StringBuffer sb = new StringBuffer();
        
        sb.append("[");
        for (int i = 0; i < arr.length; i++) {
            if(i!=arr.length-1){
                sb.append(arr[i]+",");
            }else{
                sb.append(arr[i]+"]");
            }
        }
        
        
        return sb.toString();
    }
    
    public static String toString(int[] arr) {
        
        //用字符串连接。
        String str = "[";
        for (int i = 0; i < arr.length; i++) {
            if(i!=arr.length-1){
                str+=arr[i]+",";
            }else{
                str+=arr[i]+"]";
            }
        }
        return str;
    }

```
StringBuilder和StringBuffer的区别。
- StringBuilder:非同步的。单线程访问效率高。
- StringBuffer：同步的，多线程访问安全。

###String内部实现
String雷内部用一个字符数组表示字符串

```java
private final char value[];
```
String有两个构造方法，可以根据char数组创建String

```java
public String(char value[])
public String(char value[], int offset, int count)
```
String会根据参数新创建一个数组，并拷贝内容，而不会直接用参数中的字符数组。

String中大部分方法，尼日不都是操作的这个字符数组，比如
- length()方法返回的就是这个数组的长度
- substring()方法就是根据参数，调用构造方法String(char value[], int offset, int count)新建了一个字符串
- indexOf查找字符或子字符串时就是在这个数组中进行查找

String中还有一些方法，与char数组有关，
返回执行索引位置的char
```java
public char charAt(int index)
```
返回字符串对应的char数组
```java
public char[] toCharArray()
```
这是一个拷贝后的数组，不是原来的数组
将char数组中指定范围的字符拷贝入目标数组指定位置

```java
public void getChars(int srcBegin, int srcEnd, char dst[], int dstBegin)
```


###编码转换
String内部是按UTF-16BE处理字符的，对于BMP字符，使用一个char，两个字节，对于增补字符，使用两个char，四个字节。
java中使用Charset这个类表示各种编码，它有两个常用的静态方法：

```java
public static Charset defaultCharset()
public static Charset forName(String charsetName)
```
第一个返回的是系统的默认编码，一般为UTF-8。
第二个我们可以设置编码

```java
Charset charset = Charset.forName("GB18030");
```
String类提供了返回字符串安给定编码的字节表示：

```java
public byte[] getBytes()  
public byte[] getBytes(String charsetName)
public byte[] getBytes(Charset charset)
```
第一个没有传入编码，使用的是默认编码，第二个参数为编码名称，第三个为Charset。

###不可变性
String类是不可变的，对象一旦被创建，就没法在进行修改。String类声明了final，所以也不能被集成，内部的char数组也是final的，初始化就不能再改变了。有些方法可能看起来会修改，但是其实是创建新的Sting对象实现的，原来的String对象不会被修改。比如下面这个方法：

```java
    public String concat(String str) {
    int otherLen = str.length();
    if (otherLen == 0) {
        return this;
    }
    int len = value.length;
    char buf[] = Arrays.copyOf(value, len + otherLen);
    str.getChars(buf, len);
    return new String(buf, true);
}
```
如果我们需要修改String的时候最好使用StringBuilder和StringBuffer，这样可以提高效率。

###常量字符串
Java中的字符串常量比较特殊，可以直接赋值给String变量还可以像String类型的对象一样直接调用Sting的各种方法。
其实这些常量就是String类型的对象，在内存中，他们被放在一个共享的地方，这个地方成为`字符串常量池`，它保存所有的常量字符串，每个常量只会保存一份，被所有共享着使用，这就是我们刚开始说的不同的变量都是同一个字符串的时候内存地址是相同的原因。`当通过常量的形式使用一个字符串的时候，使用的就是常量池中的那个对应的String类型的对象.`
但是我们使用`new`创建出来却是不一样的。具体可以看下面的代码：

```java
public String(String original) {
    this.value = original.value;
    this.hash = original.hash;
}
```
hash是String类中的另一个实例变量，表示缓存的hashCode值。
name1和name2指向两个不同的String对象，只是这两个对象内部的value值指向相同的char数组。其内存布局大概如下所示：
![](https://dn-mhke0kuv.qbox.me/f036c966d022643c7b13.jpg)
所以，`name1==name2`是不成立的，但`name1.equals(name2)`是true。
不仅仅是比较内容是否相同，还会进行hashCode值得比较。

###hashCode
hash这个实例变量，定义如下：
```java
private int hash; // Default to 0
```
它缓存了hashCode()方法的值，也就是说，第一次调用hashCode()的时候，会把结果保存在hash这个变量中，以后再调用就直接返回保存的值。
String类的hashCode方法：

```java
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        char val[] = value;

        for (int i = 0; i < value.length; i++) {
            h = 31 * h + val[i];
        }
        hash = h;
    }
    return h;
}
```
计算公式`s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]`
s表示字符串，s[0]表示第一个字符，n表示字符串长度，s[0]*31^(n-1)表示31的n-1次方再乘以第一个字符的值。

为什么要用这个计算方法呢？这个式子中，hash值与每个字符的值有关，每个位置乘以不同的值，hash值与每个字符的位置也有关。使用31大概是因为两个原因，一方面可以产生更分散的散列，即不同字符串hash值也一般不同，另一方面计算效率比较高，31*h与32*h-h即 (h<<5)-h等价，可以用更高效率的移位和减法操作代替乘法操作。

