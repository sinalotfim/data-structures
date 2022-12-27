#### Q: Create a AVL Tree is comprising the following methods:
- [x] [insert](#a-insert)
- [x] [isBalanced](#a-isbalanced)
- [x] [isPerfect](#a-isperfect)

---
#### A: Structure of a Binary Tree
```Java
public class AVLTree {
    private class AVLNode {
        private int height;
        private int value;
        private AVLNode leftChild;
        private AVLNode rightChild;

        public AVLNode(int value) {
            this.value = value;
        }

        @Override
        public String toString() {
            return "Value = " + this.value;
        }
    }

  private AVLNode root;
}
```
---
#### A: insert
```Java
// Setting height
private void setHeight(AVLNode node) {
    node.height = Math.max(height(node.leftChild), height(node.rightChild)) + 1;
}

private int height(AVLNode node) {
    return (node == null) ? -1 : node.height;
}

// Balance tree
private int balanceFactor(AVLNode node) {
    return (node == null) ? 0 : height(node.leftChild) - height(node.rightChild);
}

private boolean isLeftHeavy(AVLNode node) {
    return balanceFactor(node) > 1;
}

private boolean isRightHeavy(AVLNode node) {
    return balanceFactor(node) < -1;
}

private AVLNode rotateLeft(AVLNode root) {
    var newRoot = root.rightChild;

    root.rightChild = newRoot.leftChild;
    newRoot.leftChild = root;

    setHeight(root);
    setHeight(newRoot);

    return newRoot;
}

private AVLNode rotateRight(AVLNode root) {
    var newRoot = root.leftChild;

    root.leftChild = newRoot.rightChild;
    newRoot.rightChild = root;

    setHeight(root);
    setHeight(newRoot);

    return newRoot;
}

private AVLNode balance(AVLNode root) {
    if (isLeftHeavy(root)) {
        if (balanceFactor(root.leftChild) < 0)
            root.leftChild = rotateLeft(root.leftChild);

      return rotateRight(root);
    } else if (isRightHeavy(root)) {
        if (balanceFactor(root.rightChild) > 0)
            root.rightChild = rotateRight(root.rightChild);
        
        return rotateLeft(root);
    }

    return root;
}

// Insert
public void insert(int value) {
    root = insert(root, value);
}

private AVLNode insert(AVLNode root, int value) {
    if (root == null)
        return new AVLNode(value);

    if (value < root.value) root.leftChild = insert(root.leftChild, value);
    else root.rightChild = insert(root.rightChild, value);

    setHeight(root);

    return balance(root);
}
```
---
#### A: isBalanced
```Java
public boolean isBalanced() {
    return isBalanced(root);
}

private boolean isBalanced(Node root) {
    if (root == null)
        return true;

    var balanceFactor = height(root.leftChild) - height(root.rightChild);

    return 
        Math.abs(balanceFactor) <= 1 &&
        isBalanced(root.leftChild) &&
        isBalanced(root.rightChild);
}
```
---
#### A: isPerfect
```Java
public boolean isPerfect() {
    return size() == (Math.pow(2, height() + 1) - 1);
}
```
