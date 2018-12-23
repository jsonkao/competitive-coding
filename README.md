### Table of Contents

| [Problem Patterns](#problem-patterns) |
|-----------------|
| [Arrays](#arrays) |

| [Data Structures](#data-structures) |
|-----------------|
| [Using Arrays](#using-arrays) |
| [Heaps](#heaps) |
| [Graphs](#graphs) |

| [Algorithms](#algorithms) |
|-----------------|
| [Sorting](#sorting) |
| [Rubin-Karp substring search](#rubin-karp-substring-search) |

# Problem Patterns

## Arrays

When doing dynamic programming with arrays, you typically store the best possible answer up to index `i` in another array's index `i`.

# Data Structures

## Using Arrays

_For storing ordered collections_

For LinkedLists, Stacks, Queues, use native array methods:
- Add to the end of an Array: `let newLength = fruits.push('Orange')`
- Add to the front of an Array: `fruits.unshift('Apple')`
- Remove from the end of an Array: `let last = fruits.pop(); // remove Orange (from the end)`
- Remove from the front of an Array `let first = fruits.shift(); // remove Apple from the front`

More array methods:
- Remove n items by index position: `let removedItem(s) = fruits.splice(pos, n); // this is how to remove an item`

### Sets

`new Set([ ... ])` stores unique values.

## Graphs

A graph is a set of vertices and a set of edges. Implementations are:
1. each Vertex stores an adjacency list
2. a map of { Vertex: adjacency list }

### Topological sort

Some ordering of vertices in a graph where if there is a path v_i to v_j, v_i appears before v_j in the ordering.

1. We store an indegree on each vertex, which represents how many vertices point to it.
2. We maintain a queue of vertices that have indegree = 0.
3. For every vertex in the queue, decrement its neighbors' indegrees. Add neighbors

```
topsort() throws CycleFoundException {
  Queue<Vertex> q = new Queue<Vertex>( );
  int counter = 0;

  for each Vertex v
    if( v.indegree == 0 )
      q.enqueue( v );

  while( !q.isEmpty( ) ) {

    Vertex v = q.dequeue( ); 
    v.topNum = ++counter; // Assign next number

    for each Vertex w adjacent to v
      if( --w.indegree == 0 ) 
        q.enqueue( w );
  }
  if ( counter != NUM_VERTICES )
    throw new CycleFoundException( );
}
```

### Djiktra's algorithm

```
Starting at a vertex s,
for each Vertex v
	v.dist = Infinity
	v.known = false
s.dist = 0
while (there are unknown vertices) {
	Vertex v = the unknown vertex with the smallest distance
	v.known = true
	for each adjacent Vertex w to v:
		if (!w.known) {
			d = distance from w to v
			if (v.dist + d < w.dist)
				w.dist = v.dist + d
		}
}
```

## Heaps

Heaps are complete binary trees where the parent must always be (isMax ? > : <) than its children.
- **insert:** Add to end of binary tree, then percolate up
- **remove:** Put last element in root's spot, then percolate down. If max heap, switch with larger child. If min heap, switch with smaller child.

---

# Algorithms

## Sorting

**HeapSort:** Add elements of array into a max heap is `O(N)`. Swap last element of heap with deleteMax(), then last element down to a valid index (before deleteMax’s spot). We are adding the maxes from right to left, so we get an ascending array `O(NlogN)`. 

**Insertion sort:** For p from 1 to N-1, make sure array from to 0 to p is sorted. Basically depends on the p-1 case. So basically bubbling the newly absorbed element leftwards.

**Merge sort:** sort left, sort right, merge the two. Merging has three counters: left, right, current. Inc them as appropriate while comparing left and right to determine which one goes into current. It's a good idea to Insertion sort arrays with N ≤ 20.

**Quicksort:** pivot = median(first, middle, last element). partition with counters i from 0 and j from last - 1 (backwards). Move i forward until element ≥ pivot. Move j back until element ≤ pivot. Switch i and j. Return quicksort(left) + pivot + quicksort(right).

## Rubin-Karp: search substring of length `s` in string of length `b`

Hashes windows of length `s`. Exploits the fact that `two strings equal --> hashes are equal`. Strongly reduces string matching.

Problems come from collisions, which can be reduced with a rolling hash function, which computes a hash based on the hash of a previous value in constant time.

Our hash function treats a string in some base >= the number of possible letters, say 128.
```js
// String: 'doe '
let code = c => c.charCodeAt(0);

hash('doe') = code('d') * 128^2 + code('o') * 128 + code('e') * 1
hash('oe ') = (hash('doe') - code('d') * 128^2) * 128 + code(' ')
```
