The algorithm is based on two ideas:

1) if the current node has no left node it means that the current node, at this stage of the execution, IS the leftmost node
2) the right node of the predecessor must point to the successor

Let's imagine a simple tree:
```
    5
   / \
  3   7
 / \
2   4 
```

The inorder traversal of that tree would be: 2, 3, 4, 5, 7. Before the start 2) does not hold true. The predecessor points to the successor only in cases of 3 -> 4 and 5 -> 7. During the execution the algorithm would create missing links of 2 -> 3, 4 -> 5 and perform a clean up after traversing them.

The step by step execution of the algorithm is:

- set 5 as current, set 3 as prev and then iterate to 4 where we would create a right link to 5
- set 3 as current, set 2 as prev and create a link from 2 to 3
- set 2 as current. since 2 has no left node we process 2 and set its right node as current. so now current is 3
- current is 3, prev is 2 but now algorithms spots a link to current which means that we have already processed 2 AND that means that 3 is a direct successor to 2 so we can process it. after that we can set 4 as current
- now current is 4, it has no left node that means we can process it. after that we set its right node as current which is 5
- 5 is current, prev is 3, we iterate until right equals to 5. that means we have processed 4 and we can process its successor (5)
- we set 7 as current after processing 5. 7 has left nodes so we can process it as well and set its right node as current
- current equals null, alrorithm stops

```
function morris(root) {
    let cur = root;

    while (cur) {
        if (!cur.left) {
            process(cur);
            cur = cur.right;
        } else {
            let prev = cur.left;

            while (prev.right !== null && prev.right !== cur) {
                prev = prev.right;
            }

            if (prev.right === null) {
                prev.right = cur;
                cur = cur.left;
            } else {
                prev.right = null;
                process(cur);
                cur = cur.right;
            }
        }
    }
}
```