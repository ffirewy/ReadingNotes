# ArrayList
## Basic
内部维护size和elementData数组，size为实际使用大小，elementData为实际数组大小，每次在add时，都会判断size + 1是否超过了elementData，如果超过，则将elementData的大小增大到原来的1.5倍。
### batchRemove
```java
private boolean batchRemove(Collection<?> c, boolean complement) {
    final Object[] elementData = this.elementData;
    int r = 0, w = 0;
    boolean modified = false;
    try {
        for (; r < size; r++)
            if (c.contains(elementData[r]) == complement)
                elementData[w++] = elementData[r];
    } finally {
        // Preserve behavioral compatibility with AbstractCollection,
        // even if c.contains() throws.
        if (r != size) {
            System.arraycopy(elementData, r,
                             elementData, w,
                             size - r);
            w += size - r;
        }
        if (w != size) {
            // clear to let GC do its work
            for (int i = w; i < size; i++)
                elementData[i] = null;
            modCount += size - w;
            size = w;
            modified = true;
        }
    }
    return modified;
}
```
## Detail
* ArrayList在插入时，如果使用add(E element)，则时间复杂度为O(1)，并不是O(n)
* SubList并不是新建一个数组，可以理解为对原链表某一段进行直接操作

## Remain
Spliterator