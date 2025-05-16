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