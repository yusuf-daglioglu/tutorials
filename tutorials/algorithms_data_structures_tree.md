# ALGORITHMS DATA STRUCTURES TREE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 real usa case examples of tree

- Store in memory the `XML`, `YAML`, `JSON` models.
- `file system`
- Company organizational structure: departments and subdepartments, positions (team-lead/manager,director etc)
- `family tree (⟷ genealogy ⟷ pedigree chart ⟷ soyağacı ⟷ seçere)`
- `evolutionary tree (⟷ evrim ağacı)`
- `Table of Contents` of a document
- some search algorithms (like `binary search`) works in a special trees like `binary search tree`.
- we can store and find words from dictionary tries.

## 📌 graph vs tree

`tree` is a fork of `graph`.

Differences:

- a disconnected `node` can be exist on `graph` but not on `tree`.
- on `tree` the `nodes` can not be connected except parent and child. But on `graph` any `node` can connect to any `node`.
- on `tree` a `child` can not connect multiple `parent`. But on `graph` there is a unlimited connections to any `node`.

## 📌 tree vs "node with list"

Instead of using a `tree` data structure of a library, we can create a class with a list. That would be same as `tree`. But in order to use the features (like search functionality) of third party libraries we should prefer the `tree` data structure.

## 📌 ağaç (⟷ tree)

It is a fork of `graph`.

In `tree` there is only 1 way to access 1 `node`. Some terms:

__Yaprak (⟷ Leaf)__: The `node` which does not have a `child`.

__Kardeş (⟷ Sibling)__: The `nodes` which have the same `parent`.

__Ata (⟷ Ancestor)__: The `ancestors` of `Node`-`A` are the all `nodes` from the root to `Node`-`A`.

__Ayrıt (⟷ edge)__: Each connection between `nodes`.

__descendant (⟷ torun)__: All child `nodes` below a specific `node`.

__level (⟷ düzey ⟷ seviye)__: The level of `Node`-`A` is distance from the root `node` to `Node`-`A`.

__height (⟷ yükseklik)__: The height of a `node`, is the distance from it's farthest grandchildren.

__derinlik (⟷ depth)__: the depth of a `tree` is the distance between the root `node` and the farthest `node` to the root.

__breadth__: kelime anlamı: genişlik/en. The number of `nodes` which does not have another `child`.

## 📌 tree types

Some of `tree` types: https://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees `title: "Types of binary trees".`

Most of `tree` types are extended from `binary tree`. Therefor in most of term `binary` keyword does not using as a prefix or suffix. Example: Instead of `full binary tree` term, people use `full tree` term.

## 📌 Binary Tree (⟷ İkili Ağaç)

- In a `binary tree` each `node` can have only maximum 2 and minimum zero child.
- Each `child` called __sol çocuk (⟷ left child)__ ve __sağ çocuk (⟷ right child)__.

## 📌 full binary tree (⟷ proper binary tree ⟷ plane binary tree ⟷ strict binary tree ⟷ strictly binary tree)

- It is a fork of `binary tree`.
- Each `node` may have zero or 2 `child`s.

## 📌 Perfect binary tree

- It is a fork of `binary tree`.
- If it is visualized the `edge` `nodes` should be in same level.
- Example:

```text
      4
     / \
    /   \
   2     5
  / \   / \
 1   3 4   6
```

- Every `perfect tree` is is complete `tree`. The opposite is not always correct.

## 📌 Complete Binary Tree

- In some articles, we can see the `perfect` term instead of `complete`. In this case on the same article "__almost complete binary tree (⟷ nearly binary complete)__" should be used instead of `complete binary tree`.
- `Complete Binary Tree` is a `binary tree` fork.
- Maximum level (distance) between leafs (the last `edge` `nodes`) can be 1.
- Each new added `node` should be added from left to right.
- If one level is not full, we can not add any leaf to another level.
- example:

```text
      4
     / \
    /   \
   2     5
  / \   / \
 1   3 4


      4
     / \
    /   \
   2     5
  / \   / \
 1
```

## 📌 Height Balanced Binary Tree

- In some articles called as `Balanced tree` or `Balanced binary tree`. But this is not correct definition.
- For each `node` this rule should e correct: `Node` of the right and left side should have maximum 1 level difference.
- Examples:

```text
      4
     / \
    /   \
   2     5
  / \   / \
 1   3     6

        4
       / \
      /   \
     /     \
    /       \
   2         5
  / \       /  \
                6

        1
       / \
      /   \
     /     \
    /       \
   1         1
  / \       / \
 1             1

        1
       / \
      /   \
     /     \
    /       \
   1         1
  / \       / \
 1         1
          /
          1
```

## 📌 Yığın Ağacı (⟷ Heap tree ⟷ öbek ⟷ binary heap)

- It's a fork of `binary tree`.
- Every `parent` `node` should have bigger (numeric) value then the `child` and `grandchild`.
- __yığınlamak (⟷ heapify)__ is the name of the operation to make any `tree` to the `Heap tree`.

## 📌 İkili Arama Ağacı (⟷ Binary Search Tree ⟷ BST)

- It is a fork of `binary tree`.
- Each parent `node` should have bigger (numeric) value from:
  - it's left `child`
  - all `grandchild` (from/under it's left `child`)
- Each parent `node` should have lower (numeric) value from:
  - it's right `child`
  - all `grandchild` (from/under it's right `child`)
- All above definitions are not enough to make `BST`. To create a `BST` we should apply all the above rules for each new (numeric) element starting from the root element. We can not insert the new element (`node`) between any `node` which above rules are valid for that position. This should be wrong.
- In `BST` tree if we go always to left `child`, we will arrive to lower (numeric) value of the tree. If we will always go to right child we will arrive to biggest (numeric) value of the tree.

## 📌 AVL Tree

- It is a fork of `BST`.
- The name is the abbreviation of the of the inventors' names.
- The `tree` name is the combination of the first characters of the names of 3 people who invented it.
- The following rule should be valid for each `node`:

  - `balance factor (⟷ denge faktörü ⟷ balans faktörü)` of the `node` `=` `height of left child` - `height of right child`

  - `balance factor` `=` `1` or `0` or `-1`

- Example invalid `tree`:

```text
      4(2)
     / \
    /   \
   2(0)
  / \
 /   \
1(0)  3(0)
```

On the above `tree`, the `balance factors` are mentioned in parenthesis for each `node`. The balance factor of the `root` `node` is `2`. Therefor the `tree` is not `AVL`.

- Another invalid example:

```text
        A(0)
       / \
      /   \
    B(2)  C(-2)
    /       \
   C(1)     D(-1)
  /           \
 E(0)         F(0)
```

On the above tree the balance factors are mentioned in parenthesis for each `node`. The balance factor of some `nodes` are `-2` and `2`. Therefor the `tree` is not `AVL`.

- The disadvantage of the `AVL` is that it stores an additional integer value in all `nodes` for the balance factor. But the operations like search/add/remove is generally fast because the tree is always balanced.

## 📌 Trie

`Türkçe`'de `geri alma` anlamına gelen `retrieval`'dan gelmektedir.

In some articles it named as:

- __metin ağacı__
- __sözlük ağacı (⟷ dictionary tree)__
- __digital tree__
- __radix tree__
- __prefix tree__ (because we can make search by prefix)

- Example

```text
          root
         /   \
        /     \
       a       \
     /  \       others here
   al    ay
  /      | \
ali    ayş  ayn
        |     |
      ayşe    ayna
```

The above tree is only a visualization for the words. In background the tree stores only 1 character on the each `node`. Because when we travel in the tree we know the path, that means we know the word. For example on the above tree there is no a `node` which stores `ayş`. That `node` stores only the `ş` character.

- On each `node` we must store a boolean named as `isCompleteWord`. Because it does not mean that the path of the `node` is not a valid word, if there is a child under that `node`. For example "grave" is a word, but `e` `node` has child because `graveyard` is also another word. Therefor `isCompleteWord` should be `true` for the `e` `node`.

## 📌 Red-Black Tree

special tree.

## 📌 tree traversal

`traversal` (gezinme) term is different then search. When we searching a specific `node`, we can design a special path to find that `node` and we terminate whole operation when we find the target `node`. But in `traversal`, we must visit all `nodes`. But for both operations we can use same algorithms.

All below algorithms can be execute in a graph. When we execute the algorithm on the graph, as a `root` `node` we can take a random `node` (or any reference can be taken by the developer considering the business logic).

On below explanations:

- `visit` means print the value of the `node` on the screen.
- `traverse` means only `visit` and do not print the value yet.

- __Depth-first search (⟷ DFS)__
  - __Pre-order Traversal (⟷ NLR ⟷ node-left-right)__
    - Visit the `root`
    - Visit recursively the left subtree
    - Visit recursively the right subtree
  - __In-order Traversal (⟷ LRN ⟷ left-node-right)__
    - `traverse` the left `tree`
    - visit the `root`
    - `traverse` the right `tree`
  - __Post-order Traversal (⟷ LNR ⟷ left-node-root)__
    - Recursively `traverse` the current `node`'s left subtree.
    - Recursively `traverse` the current `node`'s right subtree.
    - visit the `root`
  - __Reverse pre-order traversal (⟷ NRL)__
    - Visit the current `node`.
    - Recursively `traverse` the current `node`'s right subtree.
    - Recursively `traverse` the current `node`'s left subtree.
  - __Reverse post-order (⟷ RLN)__
    - Recursively `traverse` the current `node`'s right subtree.
    - Recursively `traverse` the current `node`'s left subtree.
    - Visit the current `node`.
  - __Reverse in-order (⟷ RNL)__
    - Recursively `traverse` the current `node`'s right subtree.
    - Visit the current `node`.
    - Recursively `traverse` the current `node`'s left subtree.
- __Level-order Traversal (⟷ Breadth-first search ⟷ BFS ⟷ sığ öncelikli arama ⟷ enine arama)__
  - visit the `root`
  - visit the `nodes` from the left to right

Example:

```text
        F
      /  \
     /    \
    /      \
  B          G
/   \         \
A     D         I
    / \       /
    C   E     H
```

`Pre-order`   : F, B, A, D, C, E, G, I, H

`In-order`    : A, B, C, D, E, F, G, H, I

`Post-order`  : A, C, E, D, B, H, I, G, F

`Level-order` : F, B, G, A, D, I, C, E, H

If we execute the `in-order` algorithm in a `binary search tree`, that will make us to visit from the lower values to the higher values.

## 📌 Depth-first search (⟷ DFS) vs Breadth-first search (⟷ BFS)

- `DFS`: Derinlik öncelikli arama, bir `graph` veya ağaç yapısında derinlemesine gitmeyi tercih eder. Yani, bir düğümden başlayarak mümkün olduğunca derinlemesine gider ve geri döndüğünde diğer komşu düğümleri ziyaret eder. `DFS`, en kısa yolu bulmak için tasarlanmamıştır; daha çok bir düğüme ulaşmak için bir yol bulmak amacıyla kullanılır.

- `BFS`: Genişlik öncelikli arama, bir düğümden başlayarak o düğümün tüm komşularını ziyaret eder ve ardından bu komşuların komşularını ziyaret eder. `BFS`, en kısa yolu bulmak için daha uygundur, çünkü her seviyedeki düğümleri sırayla ziyaret eder.

Yol Kaydı: `BFS`, en kısa yolu bulmak için genellikle yol geçmişini saklar. Her düğümü ziyaret ettiğinde, o düğüme nasıl ulaşıldığını kaydeder. Bu, hedef düğüme ulaşmak için en kısa yolu bulmak için kullanılabilir.

## 📌  BFS tree

If we will execute the `BFS` algorithm on any graph and we will store/save the path, that (road) path will be a tree which we called `BFS tree`. Example:

There is a random example graph below.

Note: There is a connection between `D` and `E`.

```text
   A
  /  \
 /    \
/      \
B       C
       /  \
      D----E
```

If we execute the `BFS` algorithm on above `tree`, we will create the `BSF tree` as below:

```text
   A
  /  \
 /    \
/      \
B       C
       /  \
      D    E
```

## 📌 Expression Tree (⟷ İfade Ağaçları)

```text
          +
         / \
        /   \
       /     \
      /       \
     *         7
    / \
   +   c
  / \
 a   b
```

If we will execute below algorithm to above tree, we will got below results:

| Notation name | Result (Expression) | Algorithm name which executed to above tree |
|---------------|---------------------|---------------------------------------------|
| Infix         | (a+b)*c + 7         | in-order traversal                          |
| Prefix        | +*+abc7             | pre-order traversal                         |
| Postfix       | ab+c*7+             | post-order traversal                        |

## 📌 Infix vs Prefix vs Postfix

- Arithmetic operations like `3*8+9` are storing in memory in a different form by compiler when resolving/analyzing them.
- The human-readable format called `Infix`.
- Compilers are converting to these forms:
  - `Prefix notation (⟷ Polish Notation)`
  - `Postfix notation (⟷ RPN ⟷ reverse Polish Notation)`
- On `Prefix` and `Postfix` notations there is no need for parenthesis.
- Each notation has it's own rules like "signs should come first" etc.
