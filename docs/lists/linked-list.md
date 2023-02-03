# Linked List

A **linked list** is a linear collection of data elements whose order is not given by their physical placement in memory. Instead, each element points to the next.

The purpose of a linked list is to provide a consistent mechanism to store and access an arbitrary amount of data.

It is a data structure consisting of a collection of *nodes* which together represent a sequence. In its most basic form, each node contains: **data**, and a **reference** (in other words, a *link*) to the next node in the sequence.

This structure allows for efficient insertion or removal of elements from any position in the sequence during iteration. More complex variants add additional links, allowing more efficient insertion or removal of nodes at arbitrary positions.

A drawback of linked lists is that access time is linear (and difficult to pipeline). Faster access, such as random access, is not feasible.

They can be used to implement several other common abstract data types, including [lists][list_wikipedia], [stacks][stack_wikipedia], [queues][queue_wikipedia] and associative arrays (dictionaries).

## Linked List Types

### Singly linked list

**Singly linked lists** contain nodes which have a data field as well as 'next' field, which points to the next node in line of nodes. Operations that can be performed on singly linked lists include insertion, deletion and traversal.

![singly_linked_list][singly_linked_list_image]

### [Doubly linked list][doubly_linked_list_wikipedia]

In a **doubly linked list**, each node contains, besides the next-node link, a second link field pointing to the 'previous' node in the sequence. The two links may be called 'forwards' and 'backwards', or 'next' and 'prev'.

![doubly_linked_list][doubly_linked_list_image]

A technique known as [XOR-linking][xor_linked_list_wikipedia] allows a doubly linked list to be implemented using a single link field in each node. However, this technique requires the ability to do bit operations on addresses, and therefore may not be available in some high-level languages.

Many modern operating systems use doubly linked lists to maintain references to active processes, threads, and other dynamic objects. A common strategy for rootkits to evade detection is to unlink themselves from these lists.

### Multiply linked list

In a **multiply linked list**, each node contains two or more link fields, each field being used to connect the same set of data records in a different order of same set (e.g., by name, by department, by date of birth, etc.).

While doubly linked lists can be seen as special cases of multiply linked list, the fact that the two and more orders are opposite to each other leads to simpler and more efficient algorithms, so they are usually treated as a separate case.

### Circular linked list

In the last node of a list, the link field often contains a `null` reference, a special value is used to indicate the lack of further nodes.

A less common convention is to make it point to the first node of the list; in that case, the list is said to be 'circular' or 'circularly linked'; otherwise, it is said to be 'open' or 'linear'.

![circular_linked_list][circular_linded_list_image]

In the case of a circular doubly linked list, the first node also points to the last node of the list.

## Tradeoffs

|Operation                | Linked list | Array | Dynamic array  | Balanced tree | Random access list | Hashed array tree |
|-------------------------|:-----------:|:-----:|:--------------:|:-------------:|:------------------:|:-----------------:|
| Indexing                | O(n)        | O(1)  | O(1)           | O(log n)      | O(log n)           | O(1)              |
| Insert/delete at start  | O(1)        | N/A   | O(n)           | O(log n)      | O(1)               | O(n)              |
| Insert/delete at end    | O(n)        | N/A   | O(1) amortized | O(log n)      | O(log n) updating  | O(1) amortized    |
| Insert/delete in middle | Search+O(1) | N/A   | O(n)           | O(log n)      | O(log n) updating  | O(n)              |
| Wasted space (average)  | O(n)        | 0     | O(n)           | O(n)          | O(n)               | O(√n)             |

A [dynamic array][dynamic_array_wikipedia] is a data structure that allocates all elements contiguously in memory, and keeps a count of the current number of elements. If the space reserved for the dynamic array is exceeded, it is reallocated and (possibly) copied, which is an expensive operation.

Linked lists have several advantages over dynamic arrays.

Insertion or deletion of an element at a specific point of a list, assuming that we have indexed a pointer to the node (before the one to be removed, or before the insertion point) already, is a constant-time
operation (otherwise without this reference it is O(n)), whereas insertion in a dynamic array at random locations will require moving half of the elements on average, and all the elements in the worst case. While one can "delete" an element from an array in constant time by somehow marking its slot as "vacant", this causes [fragmentation][fragmentation_wikipedia] that impedes the performance of iteration.

On the other hand, dynamic arrays (as well as fixed-size arrays) allow constant-time random access, while linked lists allow only sequential access to elements. Singly linked lists, in fact, can be easily traversed in only one direction. This makes linked lists unsuitable for applications where it's useful to look up an element by its index quickly, such as [heapsort][heapsort_wikipedia]. Sequential access on arrays and dynamic arrays is also faster than on linked lists on many machines, because they have optimal [locality of reference][reference_locality_wikipedia] and thus make good use of data caching.

Another disadvantage of linked lists is the extra storage needed for references, which often makes them impractical for lists of small data items such as characters or boolean values, because the storage overhead for the links may exceed by a factor of two or more the size of the data. In contrast, a dynamic array requires only the space for the data itself (and a very small amount of control data). It can also be slow, and with a naïve allocator, wasteful, to allocate memory separately for each new element, a problem generally solved using [memory pools][memory_pool_wikipedia].

A [balanced tree][self_balanced_binary_tree_wikipeida] has similar memory access patterns and space overhead to a linked list while permitting much more efficient indexing, taking O(log n) time instead of O(n) for a random access. However, insertion and deletion operations are more expensive due to the overhead of tree manipulations to maintain balance. Schemes exist for trees to automatically maintain themselves in a balanced state: [AVL trees][avl_tree_wikipedia] or [red-black trees][red_black_tree_wikipedia].

### Singly linked linear lists vs. other lists

While doubly linked and circular lists have advantages over singly linked linear lists, linear lists offer some advantages that make them preferable in some situations.

A singly linked linear list is a recursive data structure, because it contains a pointer to a smaller object of the same type. For that reason, many operations on singly linked linear lists (such as [merging][merge_algorithm_wikipedia] two lists, or enumerating the elements in reverse order) often have very simple recursive algorithms, much simpler than any solution using iteration. While those recursive solutions can be adapted for doubly linked and circularly linked lists, the procedures generally need extra arguments and more complicated base cases.

Linear singly linked lists also allow [tail-sharing][tail_sharing_wikipedia], the use of a common final portion of sub-list as the terminal portion of two different lists.

### Doubly linked vs. singly linked

Double-linked lists require more space per node (unless one uses [XOR-linking][xor_linked_list_wikipedia]), and their elementary operations are more expensive; but they are often easier to manipulate because they allow fast and easy sequential access to the list in both directions. In a doubly linked list, one can insert or delete a node in a constant number of operations given only that node's address. To do the same in a singly linked list, one must have the address of the pointer to that node, which is either the handle for the whole list (in case of the first node) or the link field in the previous node. Some algorithms require access in both directions. On the other hand, doubly linked lists do not allow tail-sharing and cannot be used as [persistent data structures](https://en.wikipedia.org/wiki/Persistent_data_structure)

### Circularly linked vs. linearly linked

A circularly linked list may be a natural option to represent arrays that are naturally circular, e.g. the corners of a polygon, a pool of buffers that are used and released in FIFO ("first in, first out") order, or a set of processes that should be time-shared in [round-robin order][round_robin_scheduling_wikipedia]. In these applications, a pointer to any node serves as a handle to the whole list.

With a circular list, a pointer to the last node gives easy access also to the first node, by following one link. Thus, in applications that require access to both ends of the list (e.g., in the
implementation of a queue), a circular structure allows one to handle the structure by a single pointer, instead of two.

The simplest representation for an empty circular list (when such a thing makes sense) is a null pointer, indicating that the list has no nodes. Without this choice, many algorithms have to test
for this special case, and handle it separately. By contrast, the use of null to denote an empty linear list is more natural and often creates fewer special cases.

## Linked List operations

```pseudocode
struct Node<T>
{
  T data, // The data being stored in the node
  Node next // A reference to the next node, null for last node
}
```

```pseudocode
struct LinkedList
{
  Node head // points to first node of list; null for empty list
}
```

```pseudocode
node = list.first_node
while (node != null)
{
  // do something with node.data
  node = node.next;
}
```

### Node Insertion

```pseudocode
// insert new_node after node
func insert_after(Node node, Node new_node)
{
  new_node.next = node.next;
  node.next = new_node;
}
```

```pseudocode
func insert_beginning(LinkedList list, Node<T> new_node)
{
  // insert node before current first node
  new_node.next = list.first_node
  list.first_node = new_node
}
```

![linked_list_add_node][linked_list_add_node_image]

### Node Removal

```pseudocode
// remove node past this one
func remove_after(Node node)
{
  old_node = node.next;
  node.next = node.next.next;
  destroy old_node;
}
```

```pseudocode
// remove first node
func remove_beginning(LinkedList list)
{
  old_node = list.first_node
  list.first_node = list.first_node.next // point past deleted node
  destroy old_node
}
```

![linked_list_remove_node][linked_list_remove_node_image]

<!-- links -->
[list_wikipedia]: https://en.wikipedia.org/wiki/List_(abstract_data_type)
[stack_wikipedia]: https://en.wikipedia.org/wiki/Stack_(abstract_data_type)
[queue_wikipedia]: https://en.wikipedia.org/wiki/Queue_(abstract_data_type)
[doubly_linked_list_wikipedia]: https://en.wikipedia.org/wiki/Doubly_linked_list
[xor_linked_list_wikipedia]: https://en.wikipedia.org/wiki/XOR_linked_list
[dynamic_array_wikipedia]: https://en.wikipedia.org/wiki/Dynamic_array
[heapsort_wikipedia]: https://en.wikipedia.org/wiki/Heapsort
[reference_locality_wikipedia]: https://en.wikipedia.org/wiki/Locality_of_reference
[memory_pool_wikipedia]: https://en.wikipedia.org/wiki/Memory_pool
[red_black_tree_wikipedia]: https://en.wikipedia.org/wiki/Red-black_tree
[avl_tree_wikipedia]: https://en.wikipedia.org/wiki/AVL_tree
[self_balanced_binary_tree_wikipeida]: https://en.wikipedia.org/wiki/Self-balancing_binary_search_tree
[merge_algorithm_wikipedia]: https://en.wikipedia.org/wiki/Merge_algorithm
[tail_sharing_wikipedia]: https://en.wikipedia.org/wiki/Tail-sharing

<!-- images -->

[singly_linked_list_image]: https://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Singly-linked-list.svg/408px-Singly-linked-list.svg.png "A singly linked list whose nodes contain two fields: an integer value and a link to the next node"
[doubly_linked_list_image]: https://upload.wikimedia.org/wikipedia/commons/thumb/5/5e/Doubly-linked-list.svg/610px-Doubly-linked-list.svg.png "A doubly linked list whose nodes contain three fields: an integer value, the link forward to the next node, and the link backward to the previous node"
[circular_linded_list_image]: https://upload.wikimedia.org/wikipedia/commons/thumb/d/df/Circularly-linked-list.svg/350px-Circularly-linked-list.svg.png "A circular linked list"
[linked_list_add_node_image]: https://upload.wikimedia.org/wikipedia/commons/thumb/4/4b/CPT-LinkedLists-addingnode.svg/474px-CPT-LinkedLists-addingnode.svg.png
[linked_list_remove_node_image]: https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/CPT-LinkedLists-deletingnode.svg/380px-CPT-LinkedLists-deletingnode.svg.png
[fragmentation_wikipedia]: https://en.wikipedia.org/wiki/Fragmentation_(computer)
[round_robin_scheduling_wikipedia]: https://en.wikipedia.org/wiki/Round-robin_scheduling
