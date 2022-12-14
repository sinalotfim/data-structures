#### Queue
- A line of items. 
- First-In First-Out (**FIFO**).
- Sharing a resource amongst many consumers.
- Queue operations
    - `enqueue`: add an item to the back of the queue.
    - `dequeue`: remove an item from the front of the queue
    - `peek`: get the item at the front of the queue without removing item.

| Operation | Approximation |
| :--- | :---: |
| Lookup | $O(1)$ |
| Insert | $O(1)$ |
| Delete | $O(1)$ |

---
#### Q: Create a queue is composed of the following methods:
- [x] [enqueue](#a-enqueue)
- [x] [dequeue](#a-dequeue)
- [x] [peek](#a-peek)
- [x] [isEmpty](#a-isempty)
- [x] [size](#a-size)

---
#### A: Structure of a stack
```Java
public class ArrayQueue {
    private int[] items;
    private int rear;
    private int front;
    private int count;

    public ArrayQueue(int capacity) {
        items = new int[capacity];
    }

    @Override
    public String toString() {
        return Arrays.toString(items);
    }
}
```
---
#### A: enqueue
```Java
public boolean isFull() {
    return count == items.length;
}

public void enqueue(int item) {
    if (isFull()) 
        throw new IllegalStateException();

    items[rear] = item;
    rear = (rear + 1) % items.length;
    count++;
}
```
---
#### A: dequeue
```Java
public int dequeue() {
    if (isEmpty()) 
        throw new IllegalStateException();

    var item = items[front];
    items[front] = 0;
    front = (front + 1) % items.length;
    count--;

    return item;
}
```
---
#### A: peek
```Java 
public int peek() {
    if (isEmpty()) 
        throw new IllegalStateException();
        
    return items[front];
}
```
---
#### A: isEmpty
```Java
public boolean isEmpty() {
    return count == 0;
}
```
---
#### A: size
```Java
public int size() {
    return count;
}
```
