---
title: "Collection Functions"
keywords: node collection function api
sidebar: api_sidebar
permalink: collection-functions.html
toc: false
comments: false
---

Following functions are used to process collections of child or descendant nodes.

###  each

Applies a callback function to child nodes of the current node.  Returns the 
current node.

<details markdown="1">
<summary><code><strong>each</strong>(<code>callback</code>: (node: XMLBuilder, index: number) => void, <code>self</code>?: boolean, <code>recursive</code>?: boolean, <code>thisArg</code>?: any)</code></summary>
<br/>

* `callback` - a callback function which receives each child node as its first argument and the node index as its second argument
* `self` - whether to visit the current node along with child nodes (optional)
* `recursive` - whether to visit all descendant nodes in tree-order or only the immediate child nodes (optional)
* `thisArg` - value to use as this when executing callback (optional)

```js
const { create } = require('xmlbuilder2');

const root = create().ele("root");
root.ele("a").up()
    .ele("b").up()
    .ele("c").up();
const names = [];
root.each(n => names.push(n.node.nodeName));
console.log(names); // ["a", "b", "c"]
```

</details>

___

###  map

Produces an array of values by transforming each child node with the given callback function.

<details markdown="1">
<summary><code><strong>map</strong>(<code>callback</code>: (node: XMLBuilder, index: number) => any, <code>self</code>?: boolean, <code>recursive</code>?: boolean, <code>thisArg</code>?: any)</code></summary>
<br/>

* `callback` - a callback function which receives each child node as its first argument and the node index as its second argument
* `self` - whether to visit the current node along with child nodes (optional)
* `recursive` - whether to visit all descendant nodes in tree-order or only the immediate child nodes (optional)
* `thisArg` - value to use as this when executing callback (optional)

```js
const { create } = require('xmlbuilder2');

const root = create().ele("root");
root.ele("a").up()
    .ele("b").up()
    .ele("c").up();
const names = root.map(n => n.node.nodeName);
console.log(names); // ["a", "b", "c"]
```

</details>

___

###  reduce

Reduces child nodes into a single value by applying the given callback function.

<details markdown="1">
<summary><code><strong>reduce</strong>(<code>callback</code>: (value: any, node: XMLBuilder, index: number) => any, <code>initialValue</code>: any, <code>self</code>?: boolean, <code>recursive</code>?: boolean, <code>thisArg</code>?: any)</code></summary>
<br/>

* `callback` - a callback function which receives the current value as its first argument, each child node as its second argument and the node index as its third argument
* `initialValue` - initial value
* `self` - whether to visit the current node along with child nodes (optional)
* `recursive` - whether to visit all descendant nodes in tree-order or only the immediate child nodes (optional)
* `thisArg` - value to use as this when executing callback (optional)

```js
const { create } = require('xmlbuilder2');

const root = create().ele("root");
root.ele("a").up()
    .ele("b").up()
    .ele("c").up();
const names = root.reduce((val, n) => val + n.node.nodeName, "");
console.log(names); // "abc"
```

</details>

___

###  find

Returns the first child node satisfying the given predicate, or `undefined` if there are no child nodes that satisfy the predicate.

{% capture cb_note %}
  `find` returns as soon as a node satisfies the predicate, without necessarily visiting all child nodes.
{% endcapture %}
{% include note.html content=cb_note %}

<details markdown="1">
<summary><code><strong>find</strong>(<code>predicate</code>: (node: XMLBuilder, index: number) => boolean, <code>self</code>?: boolean, <code>recursive</code>?: boolean, <code>thisArg</code>?: any)</code></summary>
<br/>

* `predicate` - a predicate function which receives each child node as its first argument and the node index as its second argument and returns a boolean value indicating if the current node is the node sought
* `self` - whether to visit the current node along with child nodes (optional)
* `recursive` - whether to visit all descendant nodes in tree-order or only the immediate child nodes (optional)
* `thisArg` - value to use as this when executing callback (optional)

```js
const { create } = require('xmlbuilder2');

const root = create().ele("root");
root.ele("a").up()
    .ele("b").up()
    .ele("c").up();
const bNode = root.find(n => n.node.nodeName === "b");
console.log(bNode.node.nodeName); // "b"
```

</details>

___

###  filter

Produces an array of child nodes which pass the given predicate test.

<details markdown="1">
<summary><code><strong>filter</strong>(<code>predicate</code>: (node: XMLBuilder, index: number) => boolean, <code>self</code>?: boolean, <code>recursive</code>?: boolean, <code>thisArg</code>?: any)</code></summary>
<br/>

* `predicate` - a predicate function which receives each child node as its first argument and the node index as its second argument and returns a boolean value indicating if the current node is the node sought
* `self` - whether to visit the current node along with child nodes (optional)
* `recursive` - whether to visit all descendant nodes in tree-order or only the immediate child nodes (optional)
* `thisArg` - value to use as this when executing callback (optional)

```js
const { create } = require('xmlbuilder2');

const root = create().ele("root");
root.ele("node1").up()
    .txt("text")
    .ele("node2").up()
    .txt("more text");
const textNodes = root.filter(n => n.node.nodeType === 3); // contains "text" and "more text" nodes
```

</details>

___

###  every

Returns `true` if all child nodes pass the given predicate test.

<details markdown="1">
<summary><code><strong>every</strong>(<code>predicate</code>: (node: XMLBuilder, index: number) => boolean, <code>self</code>?: boolean, <code>recursive</code>?: boolean, <code>thisArg</code>?: any)</code></summary>
<br/>

* `predicate` - a predicate function which receives each child node as its first argument and the node index as its second argument and returns a boolean value indicating if the current node is the node sought
* `self` - whether to visit the current node along with child nodes (optional)
* `recursive` - whether to visit all descendant nodes in tree-order or only the immediate child nodes (optional)
* `thisArg` - value to use as this when executing callback (optional)

```js
const { create } = require('xmlbuilder2');

const root = create().ele("root");
root.ele("node1").up()
    .ele("node2").up()
root.every(n => n.node.nodeName.startsWith("n")); // true
```

</details>

___

###  some

Returns `true` if any of the child nodes pass the given predicate test.

{% capture cb_note %}
  `some` returns as soon as a node satisfies the predicate, without necessarily visiting all child nodes.
{% endcapture %}
{% include note.html content=cb_note %}

<details markdown="1">
<summary><code><strong>some</strong>(<code>predicate</code>: (node: XMLBuilder, index: number) => boolean, <code>self</code>?: boolean, <code>recursive</code>?: boolean, <code>thisArg</code>?: any)</code></summary>
<br/>

* `predicate` - a predicate function which receives each child node as its first argument and the node index as its second argument and returns a boolean value indicating if the current node is the node sought
* `self` - whether to visit the current node along with child nodes (optional)
* `recursive` - whether to visit all descendant nodes in tree-order or only the immediate child nodes (optional)
* `thisArg` - value to use as this when executing callback (optional)

```js
const { create } = require('xmlbuilder2');

const root = create().ele("root");
root.ele("node1").up()
    .ele("child").up()
    .ele("node2").up()
root.some(n => n.node.nodeName.startsWith("n")); // true
```

</details>

___

###  toArray

Produces an array of child nodes.

<details markdown="1">
<summary><code><strong>toArray</strong>(<code>self</code>?: boolean, <code>recursive</code>?: boolean)</code></summary>
<br/>

* `self` - whether to visit the current node along with child nodes (optional)
* `recursive` - whether to visit all descendant nodes in tree-order or only the immediate child nodes (optional)

```js
const { create } = require('xmlbuilder2');

const root = create().ele("root");
root.ele("a").up()
    .ele("b").up()
    .ele("c").up()
const nodes = root.toArray(); // contains nodes a, b and c
```

</details>