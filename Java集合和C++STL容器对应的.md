# 一:Java 集合 vs C++ STL 对应关系表

| Java 集合                 | C++ STL 容器                        | 说明                                                 |
| ----------------------- | --------------------------------- | -------------------------------------------------- |
| `ArrayList<E>`          | `std::vector<T>`                  | 动态数组，随机访问快，适合频繁读取，插入/删除慢（特别是中间）                    |
| `LinkedList<E>`         | `std::list<T>`                    | 双向链表，插入/删除快（任意位置），访问慢                              |
|                         | `std::deque<T>`                   | Java 没有直接等价物，但 `LinkedList` 可支持双端操作，类似 deque       |
| `HashSet<E>`            | `std::unordered_set<T>`           | 无序集合，不允许重复，基于哈希表实现，查找插入 O(1)（均摊）                   |
| `TreeSet<E>`            | `std::set<T>`                     | 有序集合，不允许重复，基于红黑树/平衡树                               |
| `HashMap<K,V>`          | `std::unordered_map<K,V>`         | 无序键值映射，基于哈希表实现                                     |
| `TreeMap<K,V>`          | `std::map<K,V>`                   | 有序键值映射，基于红黑树                                       |
| `LinkedHashMap<K,V>`    | 无直接等价                             | 有顺序的哈希表，按插入顺序维护，类似 `HashMap + List`                |
| `PriorityQueue<E>`      | `std::priority_queue<T>`          | 优先队列，默认大顶堆，Java 默认小顶堆                              |
| `Stack<E>`              | `std::stack<T>`                   | 栈结构，先进后出，Java 的 `Stack` 是一个较老的类                    |
| `Queue<E>` / `Deque<E>` | `std::queue<T>` / `std::deque<T>` | 队列 / 双端队列接口，Java 多用 `LinkedList` 或 `ArrayDeque` 实现 |

# 二：Java vs C++ 常用容器增删改查性能对比

| 容器/集合                   | 增 (插入)              | 删 (删除)    | 改 (更新)    | 查 (查找)    | 说明             |
| ----------------------- | ------------------- | --------- | --------- | --------- | -------------- |
| **Java `ArrayList`**    | O(1)（尾部） / O(n)（中间） | O(n)      | O(1)（按下标） | O(1)（按下标） | 动态数组，底层数组实现    |
| **C++ `vector`**        | O(1)（尾部） / O(n)（中间） | O(n)      | O(1)（按下标） | O(1)（按下标） | 同上             |
| **Java `LinkedList`**   | O(1)（头尾） / O(n)（中间） | O(1)/O(n) | O(n)      | O(n)      | 双向链表           |
| **C++ `list`**          | 同上                  | 同上        | 同上        | 同上        | 同上             |
| **Java `HashMap`**      | O(1) 平均 / O(n) 最坏   | O(1) 平均   | O(1) 平均   | O(1) 平均   | 基于数组 + 链表/红黑树  |
| **C++ `unordered_map`** | 同上                  | 同上        | 同上        | 同上        | 类似 HashMap     |
| **Java `TreeMap`**      | O(log n)            | O(log n)  | O(log n)  | O(log n)  | 基于红黑树，按 key 有序 |
| **C++ `map`**           | 同上                  | 同上        | 同上        | 同上        | 类似 TreeMap     |
| **Java `HashSet`**      | O(1) 平均             | O(1) 平均   | 无“改”，需删再加 | O(1) 平均   | 基于 HashMap     |
| **C++ `unordered_set`** | 同上                  | 同上        | 同上        | 同上        | 同上             |
| **Java `TreeSet`**      | O(log n)            | O(log n)  | 无“改”      | O(log n)  | 基于 TreeMap     |
| **C++ `set`**           | 同上                  | 同上        | 同上        | 同上        | 同上             |

# 三：🔍 具体区别分析

1. 增（插入）

| Java              | C++                      | 区别说明                              |
| ----------------- | ------------------------ | --------------------------------- |
| `ArrayList.add()` | `vector.push_back()`     | 插入尾部效率高，插入中间要移动元素                 |
| `HashMap.put()`   | `unordered_map[key]=val` | 插入键值对，Hash 冲突处理机制略不同（Java 有红黑树优化） |
| `TreeSet.add()`   | `set.insert()`           | 都基于红黑树，插入是 log(n)                 |

2. 删（删除）

| Java                      | C++                        | 区别说明                        |
| ------------------------- | -------------------------- | --------------------------- |
| `ArrayList.remove(index)` | `vector.erase(pos)`        | 删除后要移动元素                    |
| `HashMap.remove(key)`     | `unordered_map.erase(key)` | 哈希表删除 O(1)，但 Java 在碰撞多时可能退化 |
| `TreeMap.remove(key)`     | `map.erase(key)`           | 红黑树删除 O(log n)              |

3. 改（更新）

| Java                    | C++                        | 区别说明        |
| ----------------------- | -------------------------- | ----------- |
| `ArrayList.set(index)`  | `vector[index] = val`      | 下标操作都是 O(1) |
| `HashMap.put(key, val)` | `unordered_map[key] = val` | 直接覆盖旧值      |
| `Set`/`Map` 没有直接“改”操作   | 同上                         | 一般做法是先删再加   |

4. 查（查找）
   
| Java                   | C++                            | 区别说明           |
| ---------------------- | ------------------------------ | -------------- |
| `ArrayList.get(index)` | `vector[index]`                | 随机访问 O(1)      |
| `HashMap.get(key)`     | `unordered_map.at(key)` 或 `[]` | 哈希表查找          |
| `TreeMap.get(key)`     | `map.at(key)` 或 `[]`           | 红黑树查找 O(log n) |

# 四：📌 小结建议

| 需求场景       | 推荐容器（Java）            | 推荐容器（C++）               |
| ---------- | --------------------- | ----------------------- |
| 快速随机访问     | `ArrayList`           | `vector`                |
| 快速插入删除（头尾） | `LinkedList`          | `list`                  |
| 快速 key 查找  | `HashMap`             | `unordered_map`         |
| 有序 key 查找  | `TreeMap`             | `map`                   |
| 去重集合       | `HashSet` / `TreeSet` | `unordered_set` / `set` |
| 优先队列       | `PriorityQueue`       | `priority_queue`        |
