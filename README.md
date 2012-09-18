version 0.4.0 ([source](https://github.com/aaronj1335/t-js))

t-js is freely distributable under the MIT license

overview
========
t.js is a tree-traversal library.  its only assumption is that the trees it
traverses are made up of objects with 'children' arrays:

     {
         children: [
             { },
             {
                 children: [
                     { },
                     { }
                 ]
             }
         ]
     }

 it's entirely non-recursive, including the post-order traversal and `map()`
 functions, and it works inside the browser or out.

testing
-------
unit tests are provided courtesy of
[`mocha.js`](http://visionmedia.github.com/mocha/) and
[`chai`](http://chaijs.com/).  on a unix system they can be run from the
command line with:

     $ make test

or viewed in most any system with a modern browser by opening the
`index.html` file.

documentation is generated with the `make readme` target.

usage
-----
the `t` interface is exported in either the browser or node.js.  (got this
from [`underscore.js`](http://documentcloud.github.com/underscore/)).  the
library can be installed from [npm](http://search.npmjs.org/#/t):

    $ npm install t

available functions
===================
t.bfs()
-------
perform a breadth-first search, executing the given callback at each node.

     t.bfs(node, [config], function(node, par, ctrl) {
         /* ... */
     })

- `node`:
     object where the search will start.  this could also be an array of
     objects
- `config`:
     this currently doesn't do anything for breadth-first searches
- `callback` (last argument):
     function to be executed at each node.  the arguments are:
     - `node`: the current node
     - `par`: the current node's parent
     - `ctrl`: control object.  this doesn't currently do anything.

 returns: the first `node` argument

t.dfs()
-------
perform a depth-first search, executing the given callback at each node.

     t.dfs(node, [config], function(node, par, ctrl) {
         /* ... */
     })

 in the pre-order case, `dfs()` doesn't process child nodes until after the
 callback.  so if you need to traverse an unknown tree, say a directory
 structure, you can start with just the root, and add child nodes as you go
 by appending them to `this.children` in the callback function.

- `node`:
     object where the search will start.  this could also be an array of
     objects
- `config`:
     if this is an object w/ the 'order' property set to 'post', a
     post-order traversal will be performed.  this is generally worse
     performance, but the `callback` has access to the return values of its
     child nodes.
- `callback` (last argument):
     function to be executed at each node.  the arguments are:
     - `node`: the current node
     - `par`: the current node's parent
     - `ctrl`: control object.  setting the `stop` property of this will end
     the search, setting the `cutoff` property of this will not visit any
     children of this node
     - `ret`: return values of child nodes.  this is only set if `dfs()` is
     called with the `order` property set to `post`.

 returns: the first `node` argument

t.map()
-------
given a tree, return a tree of the same structure made up of the objects
returned by the callback which is executed at each node.  think of the
`underscore`'s `_.map()` function, or python's `map()`

     t.map(node, [config], function(node, par) {
         /* ... */
     })

- `node`:
     object where the traversal will start.  this could also be an array of
     objects
- `config`:
     this is used for specifying things like pre/post order traversal
     (currently not implemented)
- `callback` (last argument):
     function to be executed at each node.  this must return an object.  the
     `map` function takes care of setting children.  the arguments are:
     - `node`: the current node
     - `par`: the current node's parent. note that this is the parent from
     the new tree that's being created.

 returns: a new tree, mapped by the callback function

t.filter()
----------
given a tree, return a tree of the same structure made up of the objects
returned by the callback which is executed at each node.  if, however, at a
given node the callback returns a falsy value, then the current node and all
of its descendents will be pruned from the output tree.

     t.filter(node, function(node, par) {
         /* ... */
     })

- `node`:
     object where the traversal will start.  this could also be an array of
     objects
- `callback` (last argument):
     function to be executed at each node.  this must return an object or a
     falsy value if the output tree should be pruned from the current node
     down.  the `filter` function takes care of setting children.  the
     arguments are:
     - `node`: the current node
     - `par`: the current node's parent. note that this is the parent from
     the new tree that's being created.

returns: a new tree, filtered by the callback function

t.stroll()
----------

_a walk through the trees..._

given two trees of similar structure, traverse both trees at the same time,
executing the given callback with the pair of corresponding nodes as
arguments.

     t.stroll(tree1, tree2, function(node1, node2) {
         /* ... */
     })

- `tree1`:
     the first tree of the traversal
- `node2`:
     the second tree of the traversal
- `callback` (last argument):
     function to be executed at each node. the arguments are:
     - `node1`: the node from the first tree
     - `node2`: the node from the second tree

t.find()
----------

given a tree and a truth test, return the first node that responds with a
truthy value

     t.find(tree, function(node, par) {
         /* ... */
     })

- `tree`:
     the tree in which to find the node
- `callback` (last argument):
     function to be executed at each node. if this function returns a truthy
     value, the traversal will stop and `find` will return the current node.
     the arguments are:
     - `node`: the current node
     - `par`: the parent of the current node

returns: the found node

_dfsPostOrder()
-----------------

this is a module-private function used by `dfs()`
credits
-------
this library is of course heavily inspired by the work of
[@jashkenas](https://twitter.com/#!/jashkenas) and others on `underscore`.
it also makes use of the hard work by folks like
[@tjholowaychuk](https://twitter.com/#!/tjholowaychuk), the jQuery team, and
[@jakeluer](https://twitter.com/#!/jakeluer).

license
-------
Copyright (C) 2011 Aaron Stacy

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.