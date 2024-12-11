*What is tree data structure?*

![[Pasted image 20241211162037.png]]
This example for tree data struct displays the hierarchical nature of it.

The root of the tree is *electronics* and it's branches are *laptops*, *cell phones* and *televisions* .
And for the farthest elements we call them leaves (the most bottom row of items, *macbook, iphone*) they have no children nodes.

If you are familiar with some other data structure such as graphs.
You might ask yourself *Are trees and graphs the same thing?*

No.
As mentioned tree has hierarchy with each node/point having max one parent node and a fixed depth.
While on the other hand graphs can be more complex in regard to mentioned properties, like nodes having multiple parents or no parents and varying distances from the root.

Now back to trees.
You already are very familiar with trees if you used folders in windows OS.

To be specific with naming the elements of tree.
- Root node -> (top most node)
- Node -> (we called them branches before)
- Leaf nodes -> (most bottom nodes, without children)

Each node having parent / child relationship for the naming.

Also for the path from let's say root to most bottom leaf node, we call them ancestors and going form children to root and descendants for opposite direction.
Not the most important thing to remember.

As for mentioned rows, they are called levels.
Root: level 0
Nodes: level 1
Leaf nodes: level 2

And a little hint on related data structure *binary tree*  the property of it is each parent at max can have two children hence the name binary tree.