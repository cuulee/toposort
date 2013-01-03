# Sorting directed acyclic graphs

## Example
Let's say, you have a list of pluginsor tasks, which depend on each other (`depends` defines plugins or tasks that should be executed before the plugin that declares the directive):

```
var plugins =
[ {name: "foo", depends: ['bar']}
, {name: "bar", depends: ["ron"]}
, {name: "john", depends: ["bar"]}
, {name: "tom", depends: ["john"]}
, {name: "ron", depends: []}
]
```

A quick analysis, will result in the following dependency tree:

```
tom
 |
john  foo
 |     |
 - - - - 
    |
   bar
    |
   ron
```

and thus the following execution flow:

```
   ron
    |
   bar
 - - - - 
 |     |
john  foo
 |
tom
```

Let's try this with `toposort`:

```js
toposort = require('toposort')

var plugins =
[ ["foo", 'bar']
, ["bar", "ron"]
, ["john", "bar"]
, ["tom", "john"]
]

// this will sort our plugins by dependecy
var results = toposort(plugins)

// now, we reverse the results to get the resulting execution flow, as above
results.reverse()

console.dir(results)
/*
Output:

[ 'ron', 'bar', 'foo', 'john', 'tom' ]

*/
```

## API

### toposort(edges)
 * edges {Array} An array of directed vertices like `[node1, node2]` (where `node1` depends on `node2`)

Returns: {Array} a list of nodes, sorted by their dependency (following edge direction as descendancy)

## Legal
MIT License