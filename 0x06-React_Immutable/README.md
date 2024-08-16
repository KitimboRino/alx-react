
# Immutable Objects and Immutability in JavaScript
06. React Immutable
This project covers the concept of immutability in Javascript

## Introduction

Immutability is a core concept in modern JavaScript development that helps improve the reliability, efficiency, and predictability of code. This guide explores what immutability is, the advantages it offers, how to use the `Immutable.js` library, and specific concepts such as Lists, Maps, merging strategies, and lazy sequences.

## Table of Contents

1. [Who, What, When, Where, and Why of Immutable Objects](#who-what-when-where-and-why-of-immutable-objects)
2. [How to Use the Immutable.js Library to Bring Immutability to JavaScript](#how-to-use-the-immutablejs-library-to-bring-immutability-to-javascript)
3. [The Differences Between List and Map](#the-differences-between-list-and-map)
4. [How to Use Merge, Concat, and Deep Merging](#how-to-use-merge-concat-and-deep-merging)
5. [What a Lazy Seq Is](#what-a-lazy-seq-is)

## Who, What, When, Where, and Why of Immutable Objects

### What Are Immutable Objects?

Immutable objects are data structures that cannot be changed once they are created. Any modification to an immutable object results in the creation of a new object, leaving the original object untouched.

### Why Use Immutable Objects?

1. **Predictability**: Since immutable objects never change, they make it easier to understand and reason about your code.
2. **Performance**: Immutable objects can optimize performance by reducing unnecessary re-renders or recalculations in frameworks like React.
3. **Debugging**: With immutable data, you avoid unintended side effects, making debugging easier.
4. **Concurrency**: Immutability is beneficial in concurrent environments as it avoids conflicts and race conditions between threads.

### When to Use Immutable Objects?

Immutable objects are particularly useful in scenarios where:

- State changes need to be managed predictably.
- Data needs to be shared between different parts of an application.
- Performance optimizations are necessary to prevent costly operations.

### Where Are Immutable Objects Used?

Immutable objects are widely used in functional programming, state management libraries like Redux, and UI frameworks like React where state immutability is crucial for efficient rendering.

### Who Should Use Immutable Objects?

Developers working on complex JavaScript applications, especially those involving state management, should consider using immutable objects to improve code reliability and maintainability.

## How to Use the Immutable.js Library to Bring Immutability to JavaScript

### Introduction to Immutable.js

`Immutable.js` is a library that provides immutable data structures for JavaScript. It offers various persistent immutable data structures like List, Map, Set, and more, along with methods to manipulate them.

### Installation

You can install `Immutable.js` using npm or yarn:

```bash
npm install immutable
```

```bash
yarn add immutable
```

### Basic Usage

Here's an example of how to use `Immutable.js` to create and manipulate an immutable List:

```javascript
import { List } from 'immutable';

// Creating an immutable list
const list1 = List([1, 2, 3]);
const list2 = list1.push(4);

// list1 remains unchanged
console.log(list1.toString()); // List [ 1, 2, 3 ]

// list2 is a new list with the added element
console.log(list2.toString()); // List [ 1, 2, 3, 4 ]
```

## The Differences Between List and Map

### List

- **Structure**: Ordered collection of elements.
- **Access**: Elements can be accessed by index.
- **Use Case**: Ideal for scenarios where order matters, such as maintaining a sequence of events or a collection of items.

### Map

- **Structure**: Collection of key-value pairs.
- **Access**: Values are accessed using keys.
- **Use Case**: Suitable for scenarios where you need to associate values with unique keys, such as storing configurations or user data.

### Example

```javascript
import { List, Map } from 'immutable';

// Creating a List
const myList = List([1, 2, 3]);

// Creating a Map
const myMap = Map({ a: 1, b: 2, c: 3 });

// Accessing elements
console.log(myList.get(0)); // 1
console.log(myMap.get('a')); // 1
```

## How to Use Merge, Concat, and Deep Merging

### Merge

`merge` combines two Maps by overriding the values of the existing keys.

```javascript
import { Map } from 'immutable';

const map1 = Map({ a: 1, b: 2 });
const map2 = Map({ b: 3, c: 4 });

const mergedMap = map1.merge(map2);
console.log(mergedMap.toString()); // Map { "a": 1, "b": 3, "c": 4 }
```

### Concat

`concat` appends elements to a List or Map without modifying the original structure.

```javascript
import { List, Map } from 'immutable';

const list1 = List([1, 2, 3]);
const list2 = List([4, 5]);

const concatenatedList = list1.concat(list2);
console.log(concatenatedList.toString()); // List [ 1, 2, 3, 4, 5 ]

const map1 = Map({ a: 1, b: 2 });
const map2 = Map({ c: 3 });

const concatenatedMap = map1.concat(map2);
console.log(concatenatedMap.toString()); // Map { "a": 1, "b": 2, "c": 3 }
```

### Deep Merging

`mergeDeep` recursively merges nested Maps or Lists.

```javascript
import { Map } from 'immutable';

const map1 = Map({ a: 1, b: Map({ c: 2, d: 3 }) });
const map2 = Map({ b: Map({ c: 4 }), e: 5 });

const deepMergedMap = map1.mergeDeep(map2);
console.log(deepMergedMap.toString()); // Map { "a": 1, "b": Map { "c": 4, "d": 3 }, "e": 5 }
```

## What a Lazy Seq Is

### Definition

A lazy sequence (Seq) in `Immutable.js` is a special type of data structure that evaluates elements on-demand rather than upfront. It allows for efficient data processing and chaining of operations without the overhead of creating intermediate collections.

### Advantages

- **Performance**: Avoids unnecessary computations by evaluating elements only when needed.
- **Memory Efficiency**: Does not create intermediate collections, saving memory.
- **Flexibility**: Supports chaining of operations, enabling complex transformations.

### Example

```javascript
import { Seq } from 'immutable';

const mySeq = Seq([1, 2, 3, 4, 5])
    .map(x => x * 2)
    .filter(x => x > 5);

console.log(mySeq.toArray()); // [6, 8, 10]
```

## Conclusion

Immutability in JavaScript brings numerous benefits, from improved code predictability to enhanced performance. By leveraging libraries like `Immutable.js`, developers can introduce immutable data structures into their applications, ensuring a more robust and maintainable codebase. Understanding the differences between Lists and Maps, merging techniques, and lazy sequences will enable you to utilize `Immutable.js` effectively in your projects.