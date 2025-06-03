# Java集合Map接口的实现函数使用。

## 一：HashMap的使用(支持随机访问)

### 1：底层结构（哈希表的形式）

- 数组 + 链表/红黑树

### 2：扩容机制

- 初始化时，容量为0。当有第一个元素加入之后，容量扩张到16。

- 当HashMap的容量达到75%（默认）的时候，会进行扩容。这里指的是一旦HashMap的容量到达给定容量的0.75时就会进行扩容。比如给定容量为16，那么一旦hashMap中有12个元素，再加一个元素，那么就会扩容。
  
- 链表的扩容，当链表的长度达到8的时候，此时会检查数组的长度是不是大于等于64，如果不是，会优先对数组进行扩容。一旦数组的长度大于等于64，那么此时链表会变成红黑树结构。需要注意的是，一旦红黑树中中的元素小于等于6的时候，会蜕化链表结构。

### 3：常见的成员函数

#### 3.1：添加元素（增加元素）

1)：V put(K key, V value) 添加元素（增加元素）

- **​无论键是否存在都会执行插入**

- **键存在时：**
  - 用新值**覆盖**旧值
  - **​返回被覆盖的旧值**
- **键不存在时：**
  - 新建映射关系
  - 返回 null（也就是返回旧值）

**​示例​：**

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);    // 返回 null（新增）
map.put("apple", 2);    // 返回 1（覆盖）
```

2)：V putIfAbsent(K key, V value)

- **只有当键不存在时候才会执行插入**

- 键存在时：
  - **不修改**现有键对应的值
  - **​返回已经存在的值**
- 键不存在时
  - 新建映射关系
  - 返回 null

**​示例​：**

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);         // 返回 null（新增）
map.putIfAbsent("apple", 2); // 返回 1（未覆盖）
map.get("apple");            // 仍然是 1
```

#### 3.2：移除元素（删除元素）

1): void remove(Object key) 根据键删除元素

**示例**:

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
// 使用get方法获取apple对应的值
System.out.println(map.get("apple")); // 输出为1
// 使用remove方法删除apple对应的值
map.remove("apple");
System.out.println(map.get("apple")); // 输出为null
```

2): boolean remove(Object key, Object value) 根据键值对删除元素

**示例**:

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
// 使用get方法获取apple对应的值
System.out.println(map.get("apple")); // 输出为1
// 使用remove方法删除apple对应的值
var hasAchieve = map.remove("apple",1);
System.out.println(hasAchieve); // 输出为null
```

3): void clear()

**示例**:

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
// 使用get方法获取apple对应的值
System.out.println(map.size()); // 输出为1
map.clear();
System.out.println(map.size()); // 输出为0
```

#### 3.3：设置元素（修改元素）

1): put函数就有修改值的作用，putIfAbsent函数也有修改值的作用。

2): V replace(K key, V value) 根据键值对修改元素

- **​功能**​：替换指定 key 的 value（仅当 key 存在时）
- **返回值**：
  - 如果 key 存在：返回被替换的旧值
  - value：如果 key 不存在：返回 null（也就是返回旧值）
  
**示例**:

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 10);
Integer oldValue = map.replace("apple", 20); // 返回 10
System.out.println(map.get("apple")); // 输出 20
Integer result = map.replace("banana", 30); // 返回 null（key不存在）
```


#### 3.4：获取元素（查看元素）

1): V get(Object key) 根据键获取值（键不存在返回null）
  
**示例**:

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
// 使用get方法获取apple对应的值
System.out.println(map.get("apple")); // 输出为1
```

2): V getOrDefault(Object key, V defaultValue) 根据键获取值（键不存在返回默认值，健存在则返回对应的值）

**注意**：这个方法在统计词频的时候用的比较多。
**示例**:

```java
// 统计词频
        String [] str = {"hello","world","hello","hello","hello","world","world","world"};
        // 使用哈希表统计str中词频
        HashMap<String, Integer> hashMap1 = new HashMap<>();
        for(int i = 0;i < str.length;i++){
            String key = str[i];
            // 这里还利用到put函数的相同key覆盖
            hashMap1.put(key,hashMap1.getOrDefault(key,0)+1);
        }
        System.out.println(hashMap1); // 输出为：{world=4, hello=4}

/*
    C++实现比这个简单一点
    vector<string>str = {"hello","world","hello","hello","hello","world","world","world"};
    // 生成哈希表
    unordered_map<string,int>hashMap;
    for(int i = 0;i < str.size();i++){
        hashMap[str[i]]++;
    }
*/
```

**示例**:

```java
HashMap<Object, Object> hashMap = new HashMap<>();
hashMap.put("yyy",1023);
// 测试getOrDefault函数
// 查找键为nihao的值，如果hashMap中不存在这个键，则返回默认值1
Object object = hashMap.getOrDefault("nihao", 1);
System.out.println(object); // 输出为1
```

3): containsKey(Object key) 判断键是否存在 

**示例**:

```java
HashMap<Object, Object> hashMap = new HashMap<>();
hashMap.put("hello",1023);
var isExist = hashMap1.containsKey("hello");
System.out.println(isExist); // true
```

4): boolean containsValue(Object value) 判断值是否存在

```java
HashMap<Object, Object> hashMap = new HashMap<>();
hashMap.put("hello",1023);
var isExist = hashMap1.containsValue(1023);
System.out.println(isExist); //true
```

### 4：操作是否线程安全

**HashMap是线程不安全的，多个线程进行操作时会出现线程安全问题！！！**

## 二：HashTable的使用（不能加入null的键值对）

### 1：底层结构

- 数组 + 链表

### 2：扩容机制

- 初始化时容量为0，当加入第一个元素时，容量扩展到11.阈值大小也是0.75

-  当容量到达11 * 0.75 = 8时，进行扩容操作，扩容的机制为2*旧容量+1。比如初始容量为11，扩容之后就是23。

- HashTable没有像HashMap一样，链表到达指定大小之后变成红黑树。

### 3：常见的成员函数


## HashTable 与 HashMap 操作接口对比

| 操作 | 方法名      | 是否重写   | 差异                      |
| -- | -------- | ------ | ----------------------- |
| 添加 | `put`    | ✅ 是    | 维护顺序链表                  |
| 删除 | `remove` | ✅ 是    | 同步更新链表                  |
| 修改 | `put`    | ✅ 是    | 顺序不变                    |
| 查询 | `get`    | ❌ 调用钩子 | 更新访问顺序（如设置 accessOrder） |

**注意**：
- HashTable不能加入null的键值对，HashMap可以加入null的键值对。

### 4：操作是否线程安全

**HashTable是线程安全的，多个线程进行操作时不会出现线程安全问题！！！**

## 三：LinkedHashMap的使用(不支持随机访问)

### 1：底层结构

- 数组 + 双向链表

### 2：扩容机制

- LinkedHashMap 的扩容机制其实是 **继承自HashMap**的，它并没有自己实现新的扩容逻辑，只是在 HashMap 的基础上增加了双向链表来维护元素的插入顺序（或访问顺序）。另外需要注意，**LinkedHashMap不会扩容时不会变成红黑树！！！！**

### 3：常见的成员函数

| 操作 | HashMap 方法 | LinkedHashMap 方法 | 是否重写 | 差异说明 |
|------|---------------|----------------------|-----------|-----------|
| 增加 | `put(K key, V value)` | `put(K key, V value)` | ✅ 重写 | 添加元素后，维护插入/访问顺序链表 |
| 删除 | `remove(Object key)` | `remove(Object key)` | ✅ 重写 | 删除节点同时移除链表中的对应位置 |
| 修改 | `put(K key, V value)` | `put(K key, V value)` | ✅ 重写 | 覆盖旧值时，顺序仍保持不变（插入顺序） |
| 查询 | `get(Object key)`     | `get(Object key)`     | ❌ 未重写（但调用顺序相关钩子） | 若设置 accessOrder=true，会在内部更新访问顺序 |

### 4：操作是否线程安全

- **线程不安全**

## 四：TreeMap的使用（最特殊的，底层实现采用红黑树实现）

### 1：底层结构

- 使用的是红黑树，和上述三种Map的底层结构是不一样的！

**🧬 TreeMap 的结构特点：**

| 特性      | 描述                          |
| ------- | --------------------------- |
| 底层结构    | 红黑树（自平衡的二叉搜索树）              |
| 插入操作    | 会自动将新节点插入到红黑树正确位置，并进行旋转维持平衡 |
| 查询时间复杂度 | O(logN)                     |
| 无需扩容    | 因为树结构天然是按节点链接，不存在“容量限制”     |


### 2：扩容机制

- **❗TreeMap 没有“扩容机制”，因为它不是基于数组或哈希表实现的！**

### 3：常见的成员函数

| 操作 | HashMap 方法 | TreeMap 方法 | 是否重写 | 差异说明 |
|------|--------------|---------------|-----------|----------|
| 增加 | `put(K key, V value)` | `put(K key, V value)` | ✅ 重写（自己实现） | TreeMap 使用红黑树插入，自动排序（按 key） |
| 删除 | `remove(Object key)`  | `remove(Object key)`  | ✅ 重写（自己实现） | TreeMap 在红黑树中删除节点，需维护平衡 |
| 修改 | `put(K key, V value)` | `put(K key, V value)` | ✅ 重写（同上）     | 若 key 存在，覆盖旧值，位置不变（仍有序） |
| 查询 | `get(Object key)`     | `get(Object key)`     | ✅ 重写（自己实现） | TreeMap 通过红黑树按 key 查找，效率 O(logN) |


### 4：操作是否线程安全

- **线程不安全**

## 五：四种 Map 实现对比总览


| 特性            | HashMap     | Hashtable | LinkedHashMap | TreeMap  |
| ------------- | ----------- | --------- | ------------- | -------- |
| 底层结构          | 数组 + 链表/红黑树 | 数组 + 链表   | 数组 + 双向链表     | 红黑树      |
| 是否有序          | 否           | 否         | 是（插入顺序）       | 是（key排序） |
| 是否线程安全        | 否           | ✅ 是       | 否             | 否        |
| 是否允许 null key | ✅ 是         | ❌ 否       | ✅ 是           | ❌ 否      |
| 查询复杂度         | 平均 O(1)     | O(1)      | O(1)          | O(logN)  |
| 扩容机制          | 有           | 有         | 继承自 HashMap   | ❌ 无      |
