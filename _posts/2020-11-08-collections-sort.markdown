---
layout:     post
title:      "Collections.sort 你真的会用吗？那些你所不知道的坑"
subtitle:   "这是一个在测试阶段永远不会出现的bug"
date:       2020-11-08
author:     "Abel"
header-img: "img/header/2020-11-08.png"
catalog: true
tags:
  - Collections.sort
  - Android
  - 排序
---

# 引子

这段代码产生什么效果？

```
val list: MutableList<Data> = CopyOnWriteArrayList()
list.add(Data(2))
list.add(Data(1))
Collection.sort(list))
print(list)
```

一般情况下，这段代码不会产生任何问题，你甚至会怀疑我在侮辱你的智商。

但我知道你手头没有 Android 7.0 的机子对不对。

如果你使用 Android 7.x 或更低版本系统，``Collection.sort()`` 下的调用栈会抛出 ``UnsupportedOperationException`` 异常。不支持的操作，这是什么鬼东西？

```
java.lang.UnsupportedOperationException
	at java.util.concurrent.CopyOnWriteArrayList$CowIterator.set(CopyOnWriteArrayList.java:818)
	at java.util.Collections.sort(Collections.java:247)
```

# 公众号复活了

哈喽，我又双叒叕把公众号更新起来啦。忙碌了一年，懂王都要坐不稳了，公众号终于又玩起来了。

前段时间遇到的一个坑，这个问题比较隐蔽。恰好周末有空，抓紧时间捣鼓一下搞搞分享。

# 事情是这样的

众所周知 ``LinkedList`` ，``ArrayList`` 之类的链表在并发时是不安全的。在并发时候的时候，可能出现意外的数组越界一类的错误。

顺理成章的，我替换成了 ``CopyOnWriteArrayList`` 这一多线程读写安全的链表。由于我需要排序，因此使用了``Collection.sort()`` 执行排序。看起来没有问题，但偏偏出现了文章开头的一幕。

这是怎么回事？

# 一分钟了解 CopyOnWriteArrayList

这得从 ``CopyOnWriteArrayList`` 的实现说起。

大家是否了解过 ``CopyOnWriteArrayList`` ？不了解也没关系，简单给大家介绍下。

顾名思义：「在写入的时候做拷贝的数组链表」。``CopyOnWriteArrayList`` 是一个遍历安全（Iterator Safety）的乐观锁链表，在进行读操作时不上锁，因此可以支持大并发的读取。而在写操作时，为了不影响正在并发的读操作，它会拷贝一份原来的数组，写入完毕后，替换原有数组的指针。

也就是说，当一个数组生成后，就再也不会发生更改。如果需要变更，则会生成一份副本，修改副本后在指针上替换原有的本体。因此，当迭代器持有了一个数组进行遍历，就保证了该数组不会发生变更。

此外，在 List 迭代器 ``ListIterator`` 中，除了常见的 ``next``，``hasNext`` 之类的读操作，还包含``remove``，``set`` 等一系列写操作。源码设计者认为，为了保证当前持有的数组不被改变，因此调用迭代器中这些写函数会抛出异常。有道理嘛，要么读要么写，并存总得有些牺牲。

接下来就是要命的地方了。在不同版本的中 ``Collection.sort()`` 的实现并不一致。

# 新版本的正确操作

Api 25（Android 7.1）以上，``Collection.sort(List<T> list)`` 直接调用了所提供 list 的 ``sort()`` 函数。然后就完事儿了，没了。

```
public static <T> void sort(List<T> list, Comparator<? super T> c) {
    list.sort(c);
}
```

# 老版本的坑爹实现

Api 25 及以下就屁事多了：

```
public static <T> void sort(List<T> list, Comparator<? super T> c) {
    Object[] a = list.toArray();
    Arrays.sort(a, (Comparator) c);
    ListIterator<T> i = list.listIterator();
    for (int j = 0; j < a.length; j++) {
        i.next();
        i.set((T) a[j]);
    }
}
```

完蛋了。``ListIterator`` 接口拿到就用，也不管是什么实现直接调用 ``set`` 函数。最终那些 Android 7.1 及以下的用户全都得凉凉。

> 面向接口编程是上等好事，但在更换实现时则要格外注意。

# 怎么办？

问题缘由了解了，怎么处理？

很简单，当需要给 ``CopyOnWriteArrayList `` 排序时，不要使用 ``Collection.sort(List<T> list)``。直接调 ``List.sort()`` 就好了。

# 饭后甜品

再来拓展一下知识怎么样？

考察题：如何安全删除链表最后一个元素？

给定如下函数定义，使其能够安全删除最后一个元素：

```
void removeLast(CopyOnWriteArrayList<Data> list) {
    
    // write the fucking code...
    
    System.out.println(list);
}
```


正确操作如下：

```
void removeLast(CopyOnWriteArrayList<Data> list) {
    Iterator<Data> iterator = list.iterator();
    while (iterator.hasNext()) {
        Data d = iterator.next();
        if (!iterator.hasNext()) {
            list.remove(d);
        }
    }
    System.out.println(list);
}
```

其一，题目给到了 ``CopyOnWriteArrayList`` ，即暗示存在并发。因此只能使用迭代器获取而不能 ``list.get(list.size())`` ;

其二，我们已经知道 ``iterator`` 不能执行包含 ``iterator.remove()`` 之类的写操作，因此只能在迭代到最后一个时，在原始 list 身上执行 ``remove``。