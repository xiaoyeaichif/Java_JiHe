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
## 二：动态数组（ArrayList）,支持扩容。