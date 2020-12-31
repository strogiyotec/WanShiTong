# Red black tree
Properties:   
1. Every node is either black or red
2. The root is black
3. Every leaf is black
4. Red node has black children
5. Every path contains the same amount of black notes

## Insert when z.p if the left child   
Do it until z.p color is RED
1.  z's uncle is black
   - Color z.p and uncle as black
   - Color z.p.p as red
   - Move z point to z.p.p
2.  z's uncle is is red and z is a right child of parent
   - Make z point to z.p and left rotate z
3. z's uncle is red and z is a left child
   - Change z.p color to black and z.p.p to red
   - Do right rotation of z.p.p

## Insert when z.p if the right child   
Do it until z.p color is RED
1.  z's uncle is black
   - Color z.p and uncle as black
   - Color z.p.p as red
   - Move z point to z.p.p
2.  z's uncle is is red and z is a left child of parent
   - Make z point to z.p and right rotate z
3. z's uncle is red and z is a right child
   - Change z.p color to black and z.p.p to red
   - Do left rotation of z.p.p

# DELETE 

1. Delete node from RBT in the same way as in BinaryTree
2. x is the node which will replace deleted node, if node is not replaced then x is just a black node
3. If color of deleted node was red then no other actions are required otherwise there are four cases
Special case when deleted node had two children   
1. Find successor of node z and assign it to y
2. If y.p != z then make transplant y with y.right , then y.right = z.right and y.right.p = y
Cases if deleted node was black repeat until x is not the root and until x.s color is black (x is the left child)
1. If x's sibling(w) is RED then
    - Make sibling as BLACK
	- Make x.p BLACK
	- LEFT ROTATE x.p
2. **w** is black and both children are black
    - Make w red and move x pointer to x.p
3. If **w** is black and left child is red 
    - Make w.left black
	- Make w red
	- Right rotate w 
	  Now w points to w.left and w.right is red(previous w) which means we are in case four
4. If **w** is black and right child is red
    - Make w color to be the same as x.p
	- Make x.p color as Black
	- Make w.right as Black
	- Left rotate x.p 
	- Make x point to root

## References
1.  [Red black trees](https://yuyuan.org/RedBlackTreeTutorial/)
