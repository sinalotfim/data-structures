#### Heap
- It **is** a **Complete Tree**.
- It **has** a **Heap Property**.
- Heap Property
    - The root node has the **largest or smallest value**.
    - Max Heap Property
        - the value of every node is **greater than or equal to its children**.
    - Min Heap Property
        - the value of every node is **smaller than or equal to its children**.
- Heaps
    - Sorting (HeapSort)
    - Graph algorithms (shortest path)
    - Priority Queues
    - Finding the kth smallest/largest value

| Operation | Approximation |
| :--- | :---: |
| Lookup | $O(\log n)$ |
| Insert | $O(\log n)$ |
| Delete | $O(\log n)$ |

---
#### Q: Create a Heap is composed of the following methods:
- [x] [insert](#a-insert)
- [x] [remove](#a-remove)
- [x] [isMaxHeap](#a-ismaxheap)
- [x] [max](#a-max)

---
#### A: Structure of a Binary Tree
```Java
public class Heap {
    private int[] items;
    private int size;

    public Heap(int capacity) {
        this.items = new int[capacity];
    }

    private int parentIndex(int index) {
        return (index - 1) / 2;
    }

    private int leftChildIndex(int index) {
        return (index * 2) + 1;
    }

    private int rightChildIndex(int index) {
        return (index * 2) + 2;
    }

    private boolean hasLeftChild(int index) {
        return leftChildIndex(index) <= size;
    }

    private boolean hasRightChild(int index) {
        return rightChildIndex(index) <= size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == items.length;
    }
}
```
---
#### A: insert
```Java
private void swap(int first, int second) {
    var temp = items[first];
    items[first] = items[second];
    items[second] = temp;
}

private void bubbleUp() {
    var index = size - 1;
    while (index > 0 && items[index] > items[parentIndex(index)]) {
        swap(index, parentIndex(index));
        index = parentIndex(index);
    }
}

public void insert(int value) {
    if (isFull())
      throw new IllegalStateException();

    items[size++] = value;

    bubbleUp();
}
```
---
#### A: remove
```Java
private int rightChild(int index) {
    return items[rightChildIndex(index)];
}

private int leftChild(int index) {
    return items[leftChildIndex(index)];
}

private boolean isValidParent(int index) {
    if (!hasLeftChild(index))
      return true;

    var isValid = items[index] >= leftChild(index);

    if (hasRightChild(index)) isValid &= items[index] >= rightChild(index);

    return isValid;
}

private int largerChildIndex(int index) {
    if (!hasLeftChild(index))
        return index;

    if (!hasRightChild(index))
        return leftChildIndex(index);

    return (leftChild(index) > rightChild(index)) ?
            leftChildIndex(index) :
            rightChildIndex(index);
}

private void bubbleDown() {
    var index = 0;
    while (index <= size && !isValidParent(index)) {
        var largerChildIndex = largerChildIndex(index);
        swap(index, largerChildIndex);
        index = largerChildIndex;
    }
}

public int remove() {
    if (isEmpty())
        throw new IllegalStateException();

    var root = items[0];
    items[0] = items[--size];

    bubbleDown();

    return root;
}
```
---
#### A: isMaxHeap
```Java
public static boolean isMaxHeap(int[] array) {
    return isMaxHeap(array, 0);
}

private static boolean isMaxHeap(int[] array, int index) {
    var lastParentIndex = (array.length - 2) / 2;
    if (index > lastParentIndex)
        return true;

    var leftChildIndex = leftChildIndex(index);
    var rightChildIndex = rightChildIndex(index);

    var isValidParent =
        array[index] >= array[leftChildIndex] &&
        array[index] >= array[rightChildIndex];

    return isValidParent &&
        isMaxHeap(array, leftChildIndex) &&
        isMaxHeap(array, rightChildIndex);
}
```
---
#### A: max
```Java
public int max() {
    if (isEmpty())
        throw new IllegalStateException();

    return items[0];
}
```