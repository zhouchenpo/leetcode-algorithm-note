# leetcode刷题笔记

## 4/16

+ Arrays.sort()+lambda的应用
  
+ 迭代法合并两个有序链表

+ 迭代法遍历二叉树
  
---

## `Arrays.sort()`的应用

对于一维数组来说，`Arrays.sort()`默认快排。为了对二维数组`arr[][]`按照一定规则排序可以利用lambda表达式。

`Arrays.sort(arr,(a,b)->a[0]-b[0]);`

其中`(a,b)->a[0]-b[0]`即代表排序方式的lambda表达式。

由此可知默认的排序方式为`(a,b)->a-b`。

也可对其进行重写。

```Java
    Arrays.sort(arr,new Comparator<int[]>(){
        @Override
            public int compare(int[] a,int[] b){
                return a[0]-b[0];
            }
    }
```

---

## 迭代法合并两个有序链表

有`l1`与`l2`两个有序链表，要将其合并成一个有序返回。

可以采用双指针法。将较小的数先连接。

```Java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(-1);
        ListNode cur = res;
        while(l1!=null&&l2!=null){
            if(l1.val<=l2.val){
                cur.next = l1;
                cur=cur.next;
                l1=l1.next;
            }else{
                cur.next = l2;
                cur=cur.next;
                l2=l2.next;
            }
        }
        if (l1 == null) {
            cur.next = l2;
        } else {
            cur.next = l1;
        }
        return res.next;
    }
}
```

---

## 迭代法前序遍历二叉树

```Java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            res.add(Integer.valueOf(node.val));
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        return res;
    }
}
```

虽然需要堆栈，看起来没有递归法快，但是递归法 __系统要堆栈__ 所以迭代还是比递归快。~~（听说的）~~
