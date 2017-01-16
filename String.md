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


