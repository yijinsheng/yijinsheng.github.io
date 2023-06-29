### Java ArrayList subList引发的故障
业务场景：

有n个数的List，需要对前n-1个数进行排序然后把最后的数放入到里面 输出一个新的List;
代码实现如下(非原业务代码):
```
     List<Integer> list = Lists.newArrayList();
        //初始化数组
        for (int i = 1; i < 11; i++) {
            list.add(i);
        }
        for (int i = 0; i < 10; i++) {
            List<Integer> listNew  = new ArrayList<>();
                listNew = list.subList(0, list.size() - 1);
                listNew.sort(new Ordering<Integer>() {
                                 @Override
                                 public int compare(Integer integer, Integer t1) {
                                     return integer - t1;
                                 }
                             }

                );
                listNew.add(list.get(list.size() - 1));
            System.out.println("after loop " + i + " listNew size ： " + listNew.size());
            System.out.println("after loop " + i + " handle list size ：  " + list.size());

```
### 导致问题
每次代码迭代 sublist后在add 会导致原数组list的size不断地变大，改变了原数组的大小，对后面使用list的地方造成了不可预知的影响。

### 问题原因
通过查看ArrayList的源码发现在调用sublist函数时并非返回一个新的List 返回了一个SubList的对象具体代码如下：

```
public List<E> subList(int fromIndex, int toIndex) {
        subListRangeCheck(fromIndex, toIndex, size);
        return new SubList(this, 0, fromIndex, toIndex);
    }
```
SubList为ArrayList的一个内部类，分析Sublist部分源码如下：

```
private class SubList extends AbstractList<E> implements RandomAccess {
        private final AbstractList<E> parent;
        private final int parentOffset;
        private final int offset;
        int size;

        SubList(AbstractList<E> parent,
                int offset, int fromIndex, int toIndex) {
            this.parent = parent;
            this.parentOffset = fromIndex;
            this.offset = offset + fromIndex;
            this.size = toIndex - fromIndex;
            this.modCount = ArrayList.this.modCount;
        }
···
}
```
通过构造方法可以看出 subList存在了一个对父类ArrayList的引用即对原始的ArrayList的引用，并且存了相对于原数组的fromIndex 和size。接下来重点看下sublist 相关的add,set,remove方法。

当我们对一个SubList对象调用add 方法时 实际上调用的是 AbstractList的add方法，源码如下：
```
public boolean add(E e) {
        add(size(), e);
        return true;
    }
```

然后调用 SubList add方法，源码如下:

```
public void add(int index, E e) {
            rangeCheckForAdd(index);
            checkForComodification();
            parent.add(parentOffset + index, e);
            this.modCount = parent.modCount;
            this.size++;
        }

```
通过sublist的源码可以明确的看到每次对一个SubList进行add操作是，底层是在父类的ArrayList中添加的了一个元素，并把自己的size+1 。注意对于父类的ArrayList并不是在最后面加入这个元素是在子类的toIndex后面加入这个元素，保证连续性。

SubList set方法源码如下：

```
public E set(int index, E e) {
            rangeCheck(index);
            checkForComodification();
            E oldValue = ArrayList.this.elementData(offset + index);
            ArrayList.this.elementData[offset + index] = e;
            return oldValue;
        }

```
SubList remove方法源码如下

```
public E remove(int index) {
            rangeCheck(index);
            checkForComodification();
            E result = parent.remove(parentOffset + index);
            this.modCount = parent.modCount;
            this.size--;
            return result;
        }
```
通过以上源码可以看到 对于SubList 进行元素操作需要谨慎，除非你能明确的知道进行操作后的影响否则不建议使用调用subList方法产生的List。可以使用以下方法避免：

```
List<Integer> listNew  = new ArrayList<>();
listNew.addAll(list.subList(0, list.size() - 1));
```

转载于 [Java ArrayList subList(..)的坑](https://blog.csdn.net/u013254237/article/details/77504357)