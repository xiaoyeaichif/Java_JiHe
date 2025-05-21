# Java常见Set的使用

## 一：HashSet（是Table + List（或者红黑树）的结构）
1:定义：HashSet是一个无序的集合，它不保证集合内元素的顺序，并且不允许有重复元素。
2：常见的成员函数
- add：添加元素（增）
```JAVA
/*
*   不区分set中对象的类型，所以可以添加任何类型的元素
*   HashSet set = new HashSet<>();
*   或者 HashSet<String> set = new HashSet<>();
*   HashSet<Integer> set = new HashSet<>();
*/
    Set set = new HashSet<>();
    boolean p1 = set.add("hello");
    System.out.println("添加结果: " + p1); // 添加结果: true
    boolean p2 = set.add("world");
    System.out.println("添加结果: " + p2); // 添加结果: true
    boolean p3 = set.add("hello");
    System.out.println("添加结果: " + p3); // 添加添加结果: false，表示Set集合是不允许添加相同的元素的
    System.out.println("HashSet: "+set);  // HashSet: [world, hello]
```

- remove：删除元素（删）
```Java
        Set set = new HashSet<>();
        boolean p1 = set.add("hello");
        System.out.println("添加结果: " + p1);
        boolean p2 = set.add("world");
        System.out.println("添加结果: " + p2);
        boolean p3 = set.add("hello");
        System.out.println("添加结果: " + p3);
        System.out.println("HashSet: "+set); // HashSet: [world, hello]
        // 删除指定对象
        boolean temp = set.remove("world");
        System.out.println("HashSet: "+set); // HashSet: [hello]，表示删除了world这个对象
```

- contains：判断元素是否存在（查）
```Java
        Set set = new HashSet<>();
        boolean p1 = set.add("hello");
        System.out.println("添加结果: " + p1);
        boolean p2 = set.add("world");
        System.out.println("添加结果: " + p2);
        boolean p3 = set.add("hello");
        System.out.println("添加结果: " + p3);
        System.out.println("HashSet: "+set);
        // 删除指定对象
        boolean temp = set.remove("world");
        System.out.println("HashSet: "+set);
        // p判断元素是否存在
        boolean p = set.contains("hello");
        System.out.println("是否存在: "+p); // 返回true表示存在，false表示不存在
```

- size：获取元素个数
```Java
    Set set1 = new HashSet<>();
    set1.add("nihao");
    int size = set1.size();
    System.out.println("size = "+size); // 输出为1
```

- isEmpty：判断集合是否为空
```Java
    Set set1 = new HashSet<>();
    set1.add("nihao");
    int size = set1.size();
    System.out.println("size = "+size);
    boolean p4 = set1.isEmpty();
    System.out.println("set1是否为空:"+p4);
```

3: 扩容机制分析
- HashSet底层使用的是HashMap，而HashMap的扩容机制与负载因子LoadFactor和table有关，另外负载因子的大小为0.75，而table的初始值被设置为DEFAULT_INITIAL_CAPACITY，并且该值的大小为16，也就是说一开始创建一个Set的容量为16。下面是Idea的验证。
``` Java
// add函数再进行插入操作的时候，会调用到HashMap的putVal函数，然后来执行扩容的操作。
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        // 如果初始table为null或者table的容量为0，则进行扩容操作
        if ((tab = table) == null || (n = tab.length) == 0)
            // 1：进入resize函数进行扩容，这个操作以后，初始化的Set的容量就会变成16，并且有个threshold = 12，这个值等于LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY，只要集合中有12个元素，就会进行扩容操作。将容量扩大为原来的两倍也就是32.
            // 2：为什么到原容量的0.75就进行扩容？因为当Set中的元素容量为12时，如果后续有大量的插入操作，很容易把Set填满，所以只是提前预判的操作；个人感觉是因为效率的问题进行考虑的。
            n = (tab = resize()).length;
        // 现在容器已经扩容到16了，开始进行哈希操作，看元素应该插入table（下标从0-15）的哪一个位置，进而将该Node插入到对应的位置
        // 需要注意这是没有发生哈希冲突的时候
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        // 一旦发生哈希冲突，则需要将冲突的Node插入到链表中
        else {
            Node<K,V> e; K k;
            // 如果第一个节点就和 key 匹配（hash 和 equals 均相同），记为 e，用于后面覆盖值。
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            // 执行红黑树数据的插入工作
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            // 链表的大小并未到达8，不需要执行树化操作。需要执行的是Node节点的插入工作
            else {
                // 这是一个死循环
                for (int binCount = 0; ; ++binCount) {
                    // 如果当前的节点为空，则将新节点插入到链表的尾部
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        // 链表的元素已经>=8个，此时需要执行别的判断操作
                        // 1：如果链表元素>=8 && table.length >= 64，则将链表转化为红黑树,这样可以提升查询的效率
                        // 2：如果上述条件得不到满足，优先将table进行扩容。
                        // 3：TREEIFY_THRESHOLD默认为8
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    // 如果当前指针的位置并未到达链表的尾部，则继续向下遍历
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        // 记录操作的次数
        ++modCount;
        // 检查大小是不是超过阈值，也就是容量的0.75
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

- 扩容验证（扩容是指HashSet中只要有12个对象就进行扩容，不管是Table + List中的节点超过12，还是单独List节点超过12）
```Java
public class kuoRongTest {
    public static void main(String[] args) {
       Set hashset = new HashSet<>();
       // 验证容量到达12时就进行扩容
        for (int i = 0; i < 16; i++){
            hashset.add(i);
        }
    }
}
![alt text](image.png)
![alt text](image-1.png)
// 可以看到进行了扩容
```