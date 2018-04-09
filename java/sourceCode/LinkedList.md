# LinkedList
## Basic
### Node
双向链表，每个节点的数据结构见下
```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```
* 首个节点的prev为null
* 最后一个节点的next为null
### node
检索一个节点
```java
Node<E> node(int index) {
    // assert isElementIndex(index);

    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```
* 如果index在链表的前半段，则从前往后遍历
* 如果index在链表的后半段，则从后往前遍历

### linkBefore
新增节点
```java
void linkBefore(E e, Node<E> succ) {
    // assert succ != null;
    final Node<E> pred = succ.prev;
    final Node<E> newNode = new Node<>(pred, e, succ);
    succ.prev = newNode;
    if (pred == null)
        first = newNode;
    else
        pred.next = newNode;
    size++;
    modCount++;
}
```

### unLink
移除节点
```java
E unlink(Node<E> x) {
    // assert x != null;
    final E element = x.item;
    final Node<E> next = x.next;
    final Node<E> prev = x.prev;
    if (prev == null) {
        first = next;
    } else {
        prev.next = next;
        x.prev = null;
    }
    if (next == null) {
        last = prev;
    } else {
        next.prev = prev;
        x.next = null;
    }
    x.item = null;
    size--;
    modCount++;
    return element;
}
```

## Detail
* LinkedList在add操作时，如果为add(int index, E element)，时间复杂度仍然为n
* 所有涉及到链表的操作都会让modCount加1
* 在Iterator迭代器操作时，不能直接去操作链表本身，不然会发生expectedModCount和modCount不一致的情况。
而在Iterator中的所有操作前都会检查expectedModCount和modCount的值是否相等

## Remain
Spliterator