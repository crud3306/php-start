

基础概念
-----------
度：指的是一个节点拥有子节点的个数。如二叉树的节点的最大度为2。  

深度：数的层数，根节点为第一层，依次类推。  

叶子节点：度为0的节点，即没有子节点的节点。  



树、二叉树、满二叉树、完全二叉树 概念分清  
------------
https://www.cnblogs.com/myjavascript/articles/4092746.html

1）自由树  
自由树是一个连通的，无回路的无向图。   

2）二叉树  
二叉树是每个节点最多有两个子树的树结构。通常子树被称作“左子树”（left subtree）和“右子树”（right subtree）。 

更通俗的理解：二叉树的每个结点至多只有二棵子树(不存在度大于2的结点)，二叉树的子树有左右之分，次序不能颠倒。  

3) 满二叉树
一棵深度为k，且有2^k-1个节点的树是满二叉树。    
另一种定义：除了叶结点外每一个结点都有左右子叶且叶子结点都处在最底层的二叉树。  

4）完全二叉树  
完全二叉树是由满二叉树而引出来的。对于深度为K的，有n个结点的二叉树，当且仅当其每一个结点都与深度为K的满二叉树中编号从1至n的结点一一对应时称之为完全二叉树。  

更通俗的理解：若设二叉树的深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第h 层所有的结点都连续集中在最左边，这就是完全二叉树。  
直白一点说：除最后一层外，每一层上的节点数均达到最大值；在最后一层上只缺少右边的若干结点。


5）平衡二叉树  
平衡二叉树：树的左右子树的高度差不超过1的数，空树也是平衡二叉树的一种  


6）哈夫曼树
哈夫曼树：带权路径长度达到最小的二叉树，也叫做最优二叉树。不关心树的结构，只要求带权值的路径达到最小值，哈夫曼树可能是完全二叉树也可能是满二叉树。  



最小堆/最大堆  
------------
https://blog.csdn.net/hrn1216/article/details/51465270  




