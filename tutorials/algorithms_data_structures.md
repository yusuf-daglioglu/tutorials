# ALGORITHMS DATA STRUCTURES

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Java Collections

- `Iterable` (interface)
  - `Collections` (interface)
    - `List` (interface)
      - `ArrayList` (class)
      - `LinkedList` (class) (also implements Queue)
      - `Vector` (class)
    - `Queue` (interface)
      - `PriorityQueue` (class)
      - `Deque` (interface)
    - `Set` (interface)
      - `HashSet` (class)
        - `LinkedHashSet` (class)
      - `SortedSet` (interface)
        - `TreeSet`

The well-known classes and interfaces are listed in this source: https://i.stack.imgur.com/q4y2a.jpg

## 📌 LinkedList vs ArrayList

Both are extended from `List`. Therefore both of them have the same APIs. There is no difference between usage. However, due to their algorithms, their speed and memory usage are different. `LinkedList` holds the reference of the next element. Therefore the new elements between the existing `list` can be added or removed very quickly. On the other hand, to add or remove elements from `ArrayList` is much more expensive. The most significant differences:

| List Type    | Read (get by index) | Add/Remove at Middle              | Add/Remove at Beginning | Add at End |
|--------------|---------------------|-----------------------------------|-------------------------|------------|
| `ArrayList`  | `O(1)`              | always is `O(n)` (shift elements) | `O(n)` (shift elements) | `O(1)`     |
| `LinkedList` | `O(n)`              | worst is `O(n)` (find + update)   | `O(1)`                  | `O(1)`     |

Note for above table: `ArrayList` will internally increase the array size. This is only done when it gets full. On those cases the speed is `O(n)`.

- `LinkedList` use more memory.
- The `LinkedList` of Java is `Doubly-linked list`.
- But the `ArrayList` stores a `native array` inside itself.

## 📌 Iterator

All implementations of `Iterator` interface should support to remove any element at the iteration step. This interface is designed, so we can iterate any type of data structure with the same API.

The `Iterator` interface can be seen as below:

```java
package java.util;

import java.util.function.Consumer;

public interface Iterator<E> {
    boolean hasNext();

    E next();

    default void remove() {
        throw new UnsupportedOperationException("remove");
    }

    // @since 1.8
    default void forEachRemaining(Consumer<? super E> action) {
        Objects.requireNonNull(action);
        while (hasNext())
            action.accept(next());
    }
}
```

## 📌 Set

- `Set` is an interface and it implements the `Collection`.

## 📌 Set vs List

- `Set` does not stores duplicate objects. List can have multiple same objects.
- Depending on the implementation `Set` can be ordered (sorted). But `List` is always ordered (sorted).

## 📌 Set implementations

- __HashSet__
  - Implements `Set`.
  - It use `hashmap` under the hood. Stores the each element inside this `hashmap`.
  - Order is not guaranteed.

- __LinkedHashSet__
  - Implements `HashSet`.
  - The main difference comparing to `HashSet` is that it stores the iteration order. The elements can be iterated same as the insertion order.
  - In order to store the order, `HashSet` stores an extra memory then `HashSet`.

- __TreeSet__
  - Implements `Set`.
  - It stores the iteration order.
  - In order to iterate the elements, it uses `compareTo` method.

## 📌 unmodifiable collections

```java
List<Character> immutableList = Collections.unmodifiableList(list);
```

The `immutableList` still has all APIs of `List`, but it throws `unsupportedOperationException` when we call the methods which manipulates the values. `immutableList` is read-only.

## 📌 data structures

## 📌 Linked List (⟷ bağlı liste) vs Double Linked List vs Circular Linked List (⟷ Dairesel bağlı liste)

- A `Linked List` should be `doubly` or `singly`.
- `Singly linked list` holds only the reference of the next element.
- `Doubly linked list` hold both next and previous element.
- `Circular linked list` stores the reference of the initial node (element) at the last node (element).

## 📌 real use case examples of double linked list

All below examples can not be singly linked list because they all have back/next feature.

- `Example 1`

`undo` and `redo` features of text editors (`word processor`s, IDEs etc...).

- `Example 2`

`next page` and `back page` buttons of the web browsers.

Notes for above 2 examples:

- `Note 1`:

Some articles explains that the 2 above examples are `stack`. But this is wrong. If we had only `back page` feature and we could not be able to go to next page, then "stack" structure would be correct.

- `Note 2`:

If back and next feature has limited capacity (example it can store only 30 pages), then we could use "queue". The new pages will be added to queue, then the 30 pages before will be removed automatically.

- `Example 3`

Deck of cards in a game

- `Example 4`

Showing the destination from one point to another in map/transportation systems.

- `Example 5`

Dynamic memory allocation of running processes. This allocation is done by virtual machines (like `JVM`) or OS's (like Linux).

- `Example 6`

Image viewer (the gallery feature for end-user) - Previous and next images are linked.

- `Example 7`

The playlist of Music Player - The songs are playing ordered. The played songs are not removed as in the queue. They still exist in the play-list.

## 📌 real use case examples of "circular linked list" and "singly linked list"

- The playlist of Music Player - It can be `circular doubly linked list` or `doubly linked list`. This depends if the end-user wants to play or not the playlist after it completes.
- Image viewer (the gallery feature for end-user) - The same details which mentioned above for the "The playlist of Music Player".
- On the multi-player games which is playing ordered (like okey, most of card games, XOX game), the users who play game are holding using `circular linked list`. The move will never return to the previous user. So it should not be `doubly`, but `singly` is enough in this case.

## 📌 Stack (⟷ yığın ⟷ yığıt ⟷ çıkın)

- We can only add or remove only from the top element.
- There is no access to the intermediate elements.
- It based on `LIFO`.
- Operations:
  - `Push`: add new element to top.
  - `Peek (⟷ top)`: read the top element (does not remove it)
  - `Pop`: remove the top element.

## 📌 real use case examples of stack

- The computer processes stores the order of the called functions in stack. On Java when we print the `stack trace` it prints that data.
- Code compilers can detect which parenthesis (or curly braces or other language specific characters) are in opening closing state. (The associations of closing or starting statements/blocks)

- Physical examples (can be used on simulations or 3D games):
  - The list of the bracelets (`türkçe`: bilezik) on a person's hand.
  - The list of the plates (`türkçe`: tabak) which are putted one above of another.
  - Lets assume that there is a car garage with one-lane road entrance. Let's assume some cars are already entered on this road. In that moment garage is full. Those cars which are waiting on the entrance road should be go back from one-lane road. The order of those cars can be store in stack.

## 📌 Kuyruk (⟷ queue)

- It based on `FIFO`.
- There is no access to the intermediate elements.
- Operations:
  - `enqueue`: add new element to rear (`türkçe`: arkadaki) of the `queue`.
  - `dequeue`: remove the first element.

## 📌 real use case examples of queue

- list of the incoming requests of a web server
- CPU scheduling
- the list of waiting customers on the call center
- market queue simulation
- list of waiting customers for the bank personal (expert). In this case in some of the banks, there is a machine which gives number on each customer. On this device the software stores the customers in a queue.
- list of waiting cars in the highway border
- consecutive triggered events. Examples: `message queue` or `event queue` platforms. Kafka does not use queue, because it does not removes any message after it consumed. But ActiveMQ uses queue.
- call log on the mobile phone. New calls are adding on the queue, and the oldest one is removing. But on the screen (GUI) the end user sees the queue reversed.

## 📌 PriorityQueue

- It extends `Queue` but it does not base on `FIFO`.
- It holds each element depending on the `compare` function.

## 📌 real use case examples of priority queue

- `OS` can give priority to specific processes.
- In some `cryptocurrency`, the money transfers (transactions) are prioritizing depending on their fee.
- On the bank the waiting customers are prioritizing depending on their total amount of money on their bank accounts.
