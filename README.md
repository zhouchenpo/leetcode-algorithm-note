# leetcode刷题笔记

## 4/19

今天太忙啦，没时间写笔记。明天搭好Hexo以后在blog里写好了。

## 4/17

+ 迭代与递归法反转链表

## 递归与迭代法反转链表

递归法可以先写递归到最后的情况，即只有一个`null`或者只有一个元素，即返回本身。然后试着考虑多一个元素的情况。后面的元素都已经完成了反转，则只需要把第一个元素接在反转前的第二个后面再接上`null`。即

```Java
head.next.next = head;
head.next = null;
```

完整代码如下

```Java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}
```

再考虑迭代。从头开始将每一个next关系反过来。此时需要一个临时应用储存next node，否则反转后下一个node丢失。

```Java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```

---

## 4/16

+ Arrays.sort()+lambda的应用
  
+ 迭代法合并两个有序链表

+ 迭代法遍历二叉树

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
