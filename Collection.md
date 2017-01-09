# Java中的collection
1. 集合中存储其实都是对象的地址。
2. 集合中可以存储基本数值吗？不行，但是jdk1.5以后可以这么写，但是存储的还是对象(基本数据类型包装类对象)。
3. 存储时提升了Object。取出时要使用元素的特有内容，必须向下转型。

`collection`的`add`方法其实存储的是对象的引用
如果添加的是基本数据类型，编译器会自动进行装箱操作


###List
1. 使用list解决插入元素的问题，可以使用add方法进行追加，放到集合的最后面
2. List接口的特有方法，全都是围绕索引来定义的，
3. List获取元素的方式有两种：一种是迭代，一种 遍历+get
4. List接口是支持对元素进行curd增删改查动作的


```java
        List list = new ArrayList();
        
        //1,添加元素。
        list.add(new Student("wangcai1",21));
        list.add(new Student("wangcai2",22));
        list.add(new Student("wangcai3",23));
        
        //2,插入元素。
        //		list.add(1, new Student("xiaoqiang",25));
        
        //3,删除元素。
        //		list.remove(2);//IndexOutOfBoundsException
        //3,修改元素。
        list.set(1, new Student("xiaoming",11));
        
        //		Object obj = list.get(1);
        //		System.out.println(obj);
        for (int i = 0; i < list.size(); i++) {
            System.out.println("get("+i+"):"+list.get(i));
        }
        
        //		for (Iterator it = list.iterator(); it.hasNext();) {
        //			Student stu = (Student) it.next();
        //			System.out.println(stu);
        //		}

```

![2016112819128集合框架图+迭代器.JPG](http://7xqmjb.com1.z0.glb.clouddn.com/2016112819128集合框架图+迭代器.JPG)

###ListIterator
当我们想在迭代器`Iterator`进行迭代的时候进行增删改查操作，这时候去操作会造成程序崩溃并抛出异常`java.util.ConcurrentModificationException`,在迭代过程中，使用了集合的方法对元素进行操作。导致迭代器并不知道集合中的变化，容易引发数据的不确定性。
```
解决:在迭代时，不要使用集合的方法操作元素。
         *  那么想要在迭代时对元素操作咋办？可以使用迭代器的方法操作。
         *  可是很遗憾：迭代器的方式只有 hasNext() ,next(),remove();
         *  Iterator有一个子接口ListIterator可以完成该问题的解决。如何获取该子接口对象呢？
         *  通过List接口中的listIterator()就可以获取，
         *  记住：该列表迭代器只有List接口有。而且这个迭代器可以完成在迭代过程中的增删改查动作。

```

```java
ArrayList list = new ArrayList();
		list.add("hehe");
		list.add("dayde");
		list.add("ni");
		list.add("shibus");
		list.add("shabi");
		
		ListIterator iterator = list.listIterator();
		
		while (iterator.hasNext()) {
			Object object = (Object) iterator.next();
			if ("hehe".equals(object)) {
//				iterator.add("mabide");
				iterator.set("caosini");
			}
			
			System.out.println(object);
		}
```

###使用LinkedList实现链表或者队列

```java
public class LinkedListTest {
    
    /**
     * @param args
     */
    public static void main(String[] args) {
        /*
         * 面试题：用LinkedList模拟一个堆栈或者队列数据结构。
         * 创建一个堆栈或者队列数据结构对象，该对象中使用LinkedList来完成的。
         *
         * 自定义堆栈结构。作业。
         */
        //创建一个队列对象。
        Queue queue = new Queue();
        //往队列中添加元素。
        queue.myAdd("itcast1");
        queue.myAdd("itcast2");
        queue.myAdd("itcast3");
        queue.myAdd("itcast4");
        
        while(!queue.isNull()){
            System.out.println(queue.myGet());
        }
    }
}
/**
 * 定义一个队列数据结构。Queue
 */
class Queue{
    //封装了一个链表数据结构。
    private LinkedList link;
    /*
     * 队列初始化时，对链表对象初始化。
     */
    Queue(){
        link = new LinkedList();
    }
    
    /**
     * 队列的添加元素功能。
     */
    public void myAdd(Object obj){
        //内部使用的就是链表的方法。
        link.addFirst(obj);
    }
    
    /**
     * 队列的获取方法。
     */
    public Object myGet(){
        return link.removeLast();
    }
    
    /**
     * 判断队列中元素是否空，没有元素就为true。
     */
    public boolean isNull(){
        return link.isEmpty();
    }
}

```

###HashSet
```java
public static void main(String[] args) {
        
        Set set = new HashSet();
        
        /*
         //		去除了字符串中的重复元素。
         set.add("nba");
         set.add("java");
         set.add("haha");
         set.add("itcast");
         set.add("haha");
         set.add("java");
         set.add("java");
         set.add("java");
         set.add("itcast");*/
        
        /*
         *
         * 为什么学生对象没有保证唯一性呢？
         * 通过对哈希表的分析。
         * 存储元素时，先调用了元素对象的hashCode()方法，而每个学生对象都是新建立的对象，
         * 所以hashCode值都不相同，也就不需要判断equals了。
         * 想要按照需求同姓名同年龄来保证学生对象的唯一性咋办？
         * 不能使用Object中hashCode方法，需要重新定义hashCode的算法内容。
         * 简单说：重写hashCode方法。
         */
        set.add(new Student("lisi1",21));
        set.add(new Student("lisi2",22));
        set.add(new Student("lisi1",21));
        set.add(new Student("lisi2",22));
        set.add(new Student("lisi1",21));
        
        "abc1".hashCode();
        
        for (Iterator it = set.iterator(); it.hasNext();) {
            System.out.println(it.next());
        }
        
    }

```

