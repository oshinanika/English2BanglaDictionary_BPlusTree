# English2BanglaDictionary_BPlusTree

## BPlus Tree
for visualization of b+tree go to https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html

### Input file :
English2BanglaDictionary.txt has 17297 entries.1st line is the data size.

## Code Explanation
### Class node :
all node activity ( adding data,splitting) will be done here.each node has a [key, value] pair

### Class BplusTree :
finding data, merging data into parent node, showing the tree sturcture will be done here.



Here order=4 means how many data can be stored in one node
```
bplustree = BPlusTree(order=4)
```

Each eng word will go to insert() in Class BplusTree which will call the Node class add() to append the data with others in the current node.
```
def add(self, key, value):
        '''
        Adds a key-value pair to the node.
        '''
        #for 1st key
        if not self.keys:
            self.keys.append(key)
            self.values.append([value])
            return None
        .......

 ```

4.The for loop in add() checks all datas in a node to see where the current word will be placed(alphabetically)
```
        #for next key onward, this loop checks where to put the pey in the list
        for i, item in enumerate(self.keys):
            if key == item:
                self.values[i].append(value)
                break

            elif key < item:
                self.keys = self.keys[:i] + [key] + self.keys[i:]
                self.values = self.values[:i] + [[value]] + self.values[i:]
                break

            elif i + 1 == len(self.keys):
                self.keys.append(key)
                self.values.append([value])
                break
 ```

When a node fills up then the split() is called.
```
def split(self):
        '''
        Splits the node into two and stores them as child nodes.
        '''
        left = Node(self.order)
        right = Node(self.order)
        mid = int(self.order / 2)

        left.keys = self.keys[:mid]
        left.values = self.values[:mid]

        right.keys = self.keys[mid:]
        right.values = self.values[mid:]

        self.keys = [right.keys[0]]
        self.values = [left, right]
        self.leaf = False
```

Once there is more than 1 level in the tree the (0) level=parent, (1+) level=child. Now when each eng word will go to insert() Class BplusTree ,it will go to the find() to call Classs Node add() to append the data with others in the current node.
```
def _find(self, node, key):
        '''
        For a given node and key, returns the index where the key should be
        inserted and the list of values at that index.
        '''
        for i, item in enumerate(node.keys):
            if key < item:
                return node.values[i], i

        return node.values[i + 1], i + 1
 ```

If any other nodes are splitted, one key would be appended to the parent key which is defined by the variable "pivot" in merge().
```
def _merge(self, parent, child, index):
        '''
        For a parent and child node, extract a pivot from the child to be
        inserted into the keys of the parent. Insert the values from the child
        into the values of the parent.
        '''
        parent.values.pop(index)
        pivot = child.keys[0]

        for i, item in enumerate(parent.keys):
            if pivot < item:
                parent.keys = parent.keys[:i] + [pivot] + parent.keys[i:]
                parent.values = parent.values[:i] + child.values + parent.values[i:]
                break

            elif i + 1 == len(parent.keys):
                parent.keys += [pivot]
                parent.values += child.values
                break
  ```

At the time of retrieve(), The find() in Class BplusTree is called to get all the bangla meanings of a eng word.

## Built With

* Sublime Text 3
* Python 3.7

### Credits
* https://gist.github.com/savarin/69acd246302567395f65ad6b97ee503d
* https://github.com/atiqahammed/B-plus-tree-dictionary-searching
