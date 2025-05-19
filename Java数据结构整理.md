# Java常见数据结构整理（Java集合）
## 一：数组Array（长度固定不支持扩容）
1：定义：Array是一种固定长度的容器，它并不是一个类，因此不像ArrayList等集合类一样拥有丰富的成员函数。
2：常见的成员函数
- length：获取数组长度
```java
int[] nums = new int[5];      // 初始化长度为 5 的数组，默认值为 0
nums[0] = 10;                 // 赋值
int x = nums[1];              // 读取
int len = nums.length;        // 获取长度（注意：这是字段，不是方法）
```

- 数组的填充
```java
// 将数组的所有元素填充为1
int[] nums = new int[5];
Array.fill(nums,1);
```

- 排序算法的使用
```java
// 数组填充
int[] nums = new int[5];
for(int i = 0; i < nums.length; i++) {
    nums[i] = (4-i) % 5;
}
Arrays.sort(nums);
```

- 判断两个数组内容是否相等
```java
int[] nums1 = {1，2，3};
int[] nums2 = {1，2，3};
boolean res = Arrays.equals(nums1,nums2);
// 判断是不是true即可
if(res){
    return true;
}else{
    return false;
}
```

- 复制原数组并且扩展数组
```java
int[] oldArr = {1, 2, 3};
int[] newArr = Arrays.copyOf(oldArr, 5);  // 长度为 5，后两个为 0
System.out.println(Arrays.toString(newArr)); // [1, 2, 3, 0, 0]
```

- 有序数组的二分查找api
```java
int[] arr = {1, 3, 5, 7};
int idx = Arrays.binarySearch(arr, 5); // 返回 2，这是数组的下标
```
## 二：动态数组（ArrayList）,初始容量为10，支持动态扩容，动态扩容为1.5倍。
1：定义：ArrayList是Java集合类中一个很重要的类，它实现了List接口，是动态数组，可以动态的增删元素。

2：常见的成员函数（只能存储泛型，int，char不能直接使用）

3：注意：默认构造器(无参构造函数)初始化时数组的容量也为0，一旦加入数据之后，容量扩大到10.之后就以1.5倍固定大小进行扩容。而有参构造器(有参构造函数)初始会给定一个大小size，那么数组的容量就是初始设置的大小size，之后超过size之后再已size的1.5倍进行扩容。
```Java
public class test {
    public static void main(String[] args) {
        // 扩容机制的验证
        List list = new ArrayList();
        //
        for(int i = 1;i <=10;i++){
            list.add(i);
        }
        // 此时容量为10
        // 添加元素11-15，此时需要扩容
        // 扩容大小为10 + (10>>1) = 10+5 = 15
        for (int i = 11;i <=15;i++){
            list.add(i);
        }
        // 这个过程又需要扩容
        // 15 + 15 / 2 = 22
        list.add(100);
        list.add(200);
        list.add(null);

        // 有参构造器的验证
        // 初始容量设置为2，一旦超过2，就进行扩容。
        List list1 = new ArrayList(2);
        for(int i = 1;i <=10;i++){
            list1.add(i);
        }

        // 需要注意的是，扩容会有拷贝操作，所以要尽可能的减少这个操作，可以给容器初始化一个合适的容量，这样就可以减少扩容的次数，从而提升系统的性能。
    }
}
```

4：扩容之后原数组会被JVM的垃圾回收器进行回收，所以原数组是不能再使用的。

5：常见的成员函数
- add：添加元素（增）
``` java
List list = new ArrayList();
list.add("1"); // 添加元素,本质上存储的是对象 list.add(new String("1"));
list.add(2); // list.add(new Interge(2));自动装箱功能
```
- remove：删除元素（删）
``` java
List list = new ArrayList();
list.add("1"); // 添加元素,本质上存储的是对象 list.add(new String("1"));
list.add(2); // list.add(new Interge(2));自动装箱功能
// 增加true对象
list.add(true); 
// 目前list = [1,2,true],删除容器中的元素
// remove(int index)：根据元素删除元素，返回被删除的元素
// remove(Object o)： 根据索引删除元素，返回boolean类型
``` 
- set：修改元素 （改）（返回被修改的对象）
``` java
List list = new ArrayList();
list.add("1"); // 添加元素,本质上存储的是对象 list.add(new String("1"));
list.add(2); // list.add(new Interge(2));自动装箱功能
// 增加true对象
list.add(true);
// 修改元素前，要记得判断容器大小是不是为空 
if(list.size()>0){
    list.set(0,"2"); // 修改第一个元素
    System.out.println("list = "+list);
}
``` 
- get：获取元素 （查）(返回值是一个对象)
``` java
List list = new ArrayList();
list.add("1"); // 添加元素,本质上存储的是对象 list.add(new String("1"));
list.add(2); // list.add(new Interge(2));自动装箱功能
// 增加true对象
list.add(true);
// 容器元素[1,2,true]
// 获取容器中的元素
// Object get(int index) 获取的是从下标，返回的是一个对象
Object obj = list.get(0); 
``` 

-  size：获取元素个数
``` java
List list = new ArrayList();
// 初始化时，list的大小为0
int size = list.size();
System.out.println("size = "+size); // 输出为0
// 添加元素，size会自动增加
list.add("1");
int size1 = list.size();
System.out.println("size1 = "+size1); //输出为1
```

## 三：LinkedList（实现是双端链表，支持增删改查，但是不支持随机访问）
1：定义：LinkedList是Java集合类中一个很重要的类，它实现了List接口，是链表，可以动态的增删元素。作为ArrayList的补充，因为ArrayList底层使用的数组，再删除一个指定元素的时候效率非常低，时间复杂度为O(n),而LinkedList底层用的链表，删除一个指定元素时间复杂度为O(1)。
2：常见的成员函数（只能存储泛型，int，char不能直接使用）
- add：添加元素（增）
- remove：删除元素（删）
- set：修改元素 （改）
- get：获取元素 （查）

## 四：Vector（Vector是一个线程安全的集合类，它实现了List接口，是动态数组，可以动态的增删元素。）
1：Vector是Java集合中一个类，它实现了List接口，底层也是用数组实现，但是Vector是线程安全的。与ArrayList和LinkedList相比，Vector的效率低于ArrayList和LinkedList。

2：成员函数与其他List基本一致

3：扩容机制，Vector默认构造器下，初始容量为10，并且之后以2倍的大小进行扩容。有参构造器下，初始容量为指定大小size，之后以size的2倍的大小进行扩容。
```Java
public class vectorTest {
    public static void main(String[] args) {
        /**
         * 线程安全的集合
         */
        // 测试扩容机制（无参构造的形式）
        List vector = new Vector();
        // 扩容机制的检验
        for(int i = 1;i <= 10;i++){
            vector.add(i);
        }
        // 超过10，再次扩容
        vector.add(100);
        // 运行完，vector的容量应该为20
    }
}
```
