# B+ Tree
Implementing B+ tree using C++.This is a CLI Version which is easy to open,compile and run.
- [X] Import
- [x] Search 
- [X] Insert
- [X] Delete
- [X] Print
- [X] Structuring the main Function


## Usage :

1. 	Make sure you have gcc compiler installed.Open cmd or terminal in the project folder.

2. 	Run the following commands :

3. 	To compile command (Not necessary unless you change the code): 
  	g++ -o "B+Tree.exe" "B+ Tree.cpp" "display.cpp" "insertion.cpp" "remove.cpp" "search.cpp" "utilFunc.cpp"

4. 	To Run command : B+Tree.exe




## Summary- What is the project all about? 

This project is small version of database system. Where we efficiently implement the B+ Tree for fast and efficient access
of files in the disc. Your database tuples will be stored as a .txt file in DBFiles folder corresponding FILE* will be saved in the 
leaf node. Above step is done to mimic the disc-block access. If we want to make more tables then we can make that many BPTree objects !!
The code can easily be scaled to implement B+ Tree for other types of records(PDF,Image,etc) in real life environments. The import option
also enables inserting of multiple existing records into the B+ Tree at the same time.

## Assumptions in our Tree :

1.	We are making a right biased tree. By this we mean if maxLimits are even we will split them
	in such a way that right sibling has one element greater.

2.	Insertion is based on the primary key. Hence all the properties of the primary key has to be followed.
	No dublicate insertion has to be done with same primary key!

3.	We are saving the \*ptr2next explicitly while ideally it is saved as the last pointer in the pointerset. But here as we 
	are using union to save the memory and seperate the leaf and non-leaf nodes, because of this \*ptr2next is explicitly
	saved !!


## Some UseFul Properties of B+ Tree:

1. B+ Tree Unlike B Tree is defined by two order values one for leaf node and another for non-leaf node.
	Minimum 50% should hold on B+ Tree Node.
	
	a.	For Non-Leaf Nodes-
		i.	ceil(maxInternalLimit/2)<= #of children <= maxInternalLimit
		ii.	ceil(maxInternalLimit/2)-1<= #of keys <= maxInternalLimit-1
		
	b.	For Leaf Nodes-
		i.	ceil(maxLeafLimit/2)<= #of keys <= maxLeafLimit
		ii.	since Leaf node will point to the dataPtr. It will be of same size as maxLeafLimit to correspond
			to every key !!!



## Search:

1.	If x is non-leaf node, we seek for the first *i* for which **keyValue** which is greater 
	than or equal-to the key k searched for. After that search continues in the node pointed 
	by ***iptr2Tree***.

2.	If all the **keyValue** are smaller than k then, we continue to search in the node pointed
	by ***(maxInternalLimit)ptr2Tree***.

3.	If x is a leaf-node, we search if k is present in the node!




## Insertion:

There are two convention being followed for the insertion(according to the google what i found out)
where, if the current node becomes full then -

1.	First give an element to the left sibling and if that doesn't
work give an element to the right sibling and if this also doesn't work split it.

2.	Simply Split into two nodes.

**Major Drawback of 1**
	Increases I/O, especially if we	check both siblings!!!


We have followed 2nd method which was comparatively easy to implement with relatively less hustle. So, 
here is the complete algorithm for [reference](http://www.cburch.com/cs/340/reading/btree/index.html?fbclid=IwAR0QFRcpIVL19PdMtZU0-wG18f-rwGS4lNvzpEAsdaZCL7BrNRBuFffiPJ0)

Descend to the leaf node where leaf fits :

a.	If the node has empty space, insert the key/reference pair into the node and We are DONE!

b.	If the node is already full, split it into two nodes, distributing the keys evenly. 

	i.	If the node is leaf,take the copy of minimum in the second node and repeat this algorithm to 
		insert it in parent node.
		
	ii.	If the node is non-leaf, exclude the middle value during split and insert the excluded value into 
		the	parent.

