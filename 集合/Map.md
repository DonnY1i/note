## 映射
映射用来存放键/值对

#### HashMap和TreeMap
散列映射对键进行散列，树映射用键的整体顺序对元素进行排序，并将其组织成搜索树。

基本映射操作
```
put(k,v);
get(k);
getOrDefault(k,0);
putIfAbsent(k,v);
```
#### 映射视图
集合框架不认为映射本身是一个集合。不过可以得到映射的视图——实现了Collection接口或某个子接口的对象。有3种视图：键集、值集合以及键/值对集。
```
Set<K> keySet();
Collection<V> values();
Set<Map.Entry<K,V>>entrySet();
//返回Map.Entry对象(映射中的键/值对)的一个集视图
```
在键集视图上调用迭代器的remove()方法，实际上会从映射中删除这个键和与它有关的值。不过不能向键集视图增加元素。条目集视图也有同样的限制。
#### WeakHashMap
当对键的唯一引用来自散列条目时，这一数据结构将与垃圾回收器协同工作一起删除键/值对WeakHashMap使用弱引用保存键。如果某个对象只能由WeakReference引用，垃圾回收器仍然回收它。

#### LinkedHashSet和LinkedHashMap
用来记住插入元素项的顺序。当条目插入到表中时，就会并入到双向链表中。链接散列映射将用访问顺序，而不是插入顺序，对散列条目进行迭代。