# Logarithms and Trees

Trees in computer science can be useful for things like binary search and quickly moving through a list. Since trees can be so important to algorithms, let's take a quick look at some of the semantics of trees:

1) Height: Trees must inherently have height. A single node with no connections to other nodes is not a tree--it's a point. By joining points to other points, we can make things like graphs and trees, which describe the relationships between points. In particular, trees tend to describe hierarchical relationships, ancestry, or taxonomy, because they inherently establish dominance relationships between the nodes, while graphs are freer in the types of relationships they describe. 

2) Leaf nodes: Leaf nodes are also called terminal nodes because they have no child nodes. Terminal nodes help us describe the type of tree we're working with; for instance, a tree which allows each node to have up to two child nodes is called a binary tree, while a tree which allows each node to have up to three child nodes is called a ternary tree, and so on (where "and so on" means continue using your Latin numerical prefixes). 

With just this base knowledge, we might already intuit that logarithms are important to trees. In case you've forgotten from math class, a logarithm describes an algebraic, exponential relationship where the exponent is the variable that's important to us (e.g. 2<sup>x</sup> = 4). The logarithmic way of writing the same expression is log<sub>2</sub>4 = x (and the logarithmic expression emphasizes that `x` is the value we seek, since we've isolated it). 

Remember, in a binary tree, each node may have `d=2` children. If we want to know how many child nodes a binary tree will have, max, at a certain height, we can use a logarithm to express that value: log<sub>2</sub>n = h. At `h=1`, `n=2`. At `h=2`, `n=4`. 

In a ternary tree, each node may have `d=3` children, and we can express this with the expression log<sub>3</sub>n = h. At `h=1`, `n=3`. At `h=2`, `n=9`, and so on. 

For trees in general, we can describe this relationship as log<sub>d</sub>n = h, where `d = the number of leaf nodes`. 

The important thing to note is that very short tres can have many leaves, which is the main reason why binary trees prove fundamental to the design of fast data structures.