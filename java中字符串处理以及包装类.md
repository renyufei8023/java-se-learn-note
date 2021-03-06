# JAVA中的字符串处理以及包装类
### 字符串
把两个字符串中相同的内容给截取出来

```java
/*
     * 作业1：获取两个字符串的最大相同子串。 "asdfitcastghjfghjk" "xcitcastvbnm" 思路：
     * 1，先明确两个字符串的长短，在长串中判断短串是否存在。 2，存在，已找到，说明短串就是最大的相同的。
     * 不存在，就将短串按照长度递减的方式，获取短串中的子串并到长串中判断。 3，一旦存在，便结束查找。
     */
    public static String getMaxSubstring(String s1, String s2) {
        
        String max, min;
        // 明确哪个是长串哪个是短串。
        max = (s1.length() > s2.length()) ? s1 : s2;
        min = max.equals(s1) ? s2 : s1;
        
        // 验证max和min
        // System.out.println("max="+max);
        // System.out.println("min="+min);
        
        for (int i = 0; i < min.length(); i++) {
            for (int start = 0, end = min.length() - i; end <= min.length(); start++, end++) {
                String temp = min.substring(start, end);
                
                if(max.contains(temp)){
                    return temp;
                }
            }
        }
        
        return null;
    }
```
对字符串进行排序的话可以先把字符串转成数组，然后使用数组的`Arrays.sort()`方法对字符串数组进行排序呢，然后生成一个新的字符串。

### 包装类

场景：通过文本框获取用户输入的数字数据，可是得到的都是字符串。 如果想要对字符串中的数字进行运算，必须要将字符串转成数字。</br>
Java中提供了相应的解决的对象。 基本数据类型对象包装类：java将基本数据类型值封装成了对象。</br>
封装成对象有什么好处？因为可以提供更多的操作基本数值的功能。</br>
 byte Byte short Short int Integer long Long float Float double Double boolean Boolean char Character
      
**基本数据类型对象包装类特点： 1，用于在基本数据和字符串之间进行转换。**
    

```java

// System.out.println(Integer.MAX_VALUE);
        // System.out.println(Integer.toBinaryString(-6));//将十进制转成二进制或
        // System.out.println(Integer.toHexString(-6));//将十进制转成十六进制或
        // System.out.println(Integer.toOctalString(-6));//将十进制转成八进制或
        
        // 1，字符串--->基本数值。 基本数值 (字符串);演示Integer int (string);
        System.out.println(Integer.parseInt("123") + 2);// NumberFormatException:
        System.out.println(Integer.parseInt("a1", 16));// 可以将其他进制转成十进制。
        // 2，基本数值---->字符串呢？34+"" String.valueOf(34); Integer.toString(int);
        System.out.println(34 + 5);
        

```

```java
public static void main(String[] args) {
        
        //		int i = 4;
        //		Integer i = new Integer(4);
        //		JDK1.5以后，有了一个包装类的新特性。目的简化书写，自动装箱，
        Integer i = 4;//自动装箱。Integer i = Integer.valueOf(4);
        i = i + 5;//原理;等号右边：将i对象转成基本数值   i.intValue() + 5;//自动拆箱。加法运算后，再次装箱。
        //i = Integer.valueOf(i.intValue()+5);
        
        
        
        Integer a = new Integer(3);
        Integer b = new Integer(3);
        System.out.println(a==b);//false
        System.out.println(a.equals(b));//true
        
        System.out.println("---------------------");
        Integer x = 128;
        Integer y = 128;
        //在jdk1.5自动装箱时，如果数值在byte范围之内，不会新创建对象空间而是使用原来已有的空间。
        System.out.println(x==y);
        System.out.println(x.equals(y));
        
        
    }
```

```java
private static final String SPACE = " ";
    
    /**
     * @param args
     */
    public static void main(String[] args) {
        
        /*
         * 练习：【面试题】： "23 9 -4 18 100 7" 要求对这串数字按照从小到大排序，生成一个数值有序的字符串。 思路：
         * 1，只有排序会，排序需要数组，数组中就要有元素。
         * 2，元素在哪里？在字符串里，怎么取出来呢？要获取字符串中的内容，是不是需要String对象。
         * 3，从字符串获取到数值后存储到一个int数组中。因为要排序。 4，将排完序的数组变成字符串。
         */
        
        String numsString = "23 9 -4 18 100 7";
        numsString = sortNumberString(numsString);
        System.out.println("nums=" + numsString);
    }
    
    public static String sortNumberString(String numsString) {
        // 1,获取字符串中的数字。怎么获取？通过空格进行indexOf的索引，找到其位置，substring截取。看上去就哦了。
        // 这个方法好麻烦，这些数值之间的分割符都是 空格。通过空格对字符串分离，分出来的都是数字内容的字符串。
        // 一个字符串通过分割变成多个字符串。split();
        String[] strs = numsString.split(SPACE);
        
        // 2,不能直接对字符串进行大小排序，因为 字符串23 比 字符串9要小，是错误的，必须转成整数值才可以比较。
        // 将字符串数组转成int数组。
        int[] nums = parseIntArray(strs);
        
        // 3,对数组排序。
        Arrays.sort(nums);
        
        // 4,将数组转成字符串。
        return toString(nums);
    }
    
    private static String toString(int[] nums) {
        
        StringBuilder sb = new StringBuilder();
        
        for (int i = 0; i < nums.length; i++) {
            if(i!=nums.length-1){
                sb.append(nums[i]+" ");
            }else{
                sb.append(nums[i]);
            }
        }
        
        return sb.toString();
    }
    
    // 将字符串数组转成int数组。
    private static int[] parseIntArray(String[] strs) {
        // 1,定义一个int数组。
        int[] arr = new int[strs.length];
        
        // 2,遍历字符串数组，把元素转成int存储到arr中。
        for (int i = 0; i < strs.length; i++) {
            
            arr[i] = Integer.parseInt(strs[i]);
        }
        return arr;
    }
```

