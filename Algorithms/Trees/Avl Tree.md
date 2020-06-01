# AVL TREE
**AVL** Tree allows heights of left and right child to differ at most +- 1.

## AVL Trees are balanced
Worst case is when right subtree has height 1 more than left for every node.  
```
h < 2/lgN
N(h) = 1 + N(h-1) + N(h-2)
```
## Left rotation
```mermaid
graph TD
X[X] --> a(a)
  X --> y(y)
  y --> b(b)
  y --> Y(Y)
```
**Rotate x**
- right child(**y**) must be non null
- makes **y** new root with **x** as it's left child
- **y**'s left child as **x**'s right child   

```mermaid
graph TD
y[y] --> X(X)
  y --> Y(Y)
  X --> a(a)
  X --> b(b)
```
## Right rotation
```mermaid
graph TD
y[y] --> X(X)
  y --> Y(Y)
  X --> a(a)
  X --> b(b)
```
**Rotate y**
- left child(**x**) must be non null
- makes **x** new root with **y** as it's right child
- **x**'s right child as **y**'s left child   


```mermaid
graph TD
X[X] --> a(a)
  X --> y(y)
  y --> b(b)
  y --> Y(Y)
```

## Left-right rotation
A node has been inserted into the right subtree of the left child

```mermaid
graph TD
C[C] --> A(A)
  C --> NULL(NULL)
  A --> NULL2(NULL)
  A --> B(B)
  B --> A

```
- Left rotation on the tree **A**
- Right rotation of **B**
```mermaid
graph TD
B[B] --> A(A)
  B --> C(C)
```

## Right-left rotation
A node has been inserted into the left subtree of the right child
```mermaid
graph TD
A[A] --> NULL(NULL)
  A --> C(C)
  C --> B(B)
  C --> NULL2(NULL)

```
