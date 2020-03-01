# 两数相加


给出两个**非空**的链表用来表示两个非负的整数。其中，它们各自的位数是按照**逆序**的方式存储的，并且它们的每个节点只能存储**一位**数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
**示例：**
```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

# 解题思路

首先理解题意，我们要计算2个链表的相加，[链表](https://baike.baidu.com/item/%E9%93%BE%E8%A1%A8)的定义我这儿就不在啰嗦了，大家不明白的可以看定义，题目里还有一个重要的信息是，要计算的链表的数字是倒序的，比如：我们要计算123 + 234 的话，输入的顺序应该是[3]->[2]->[1] + [4]->[3]->[2]，这样的一个好处就是省去了我们把链表反转那一步，因为我们相加必须的从个位开始，给定的链表已经是从个位开始了。
剩下我们要考虑的情况就是进位问题和输入的2个链表的长度不一致的问题了。

* 进位问题：假设输入的是[5]+[5]，那么我们应该输出的是[0]->[1]；
* 长度不一致问题：假设输入的是[5]->[9] + [5]，那么我们应该输出的是[0]->[0]->[1]。

完整代码如下：
```java

public class UuidTest {

    public static void main(String[] args){

        ListNode l1 = new ListNode(5);
        l1.next = new ListNode(9);

        ListNode l2 = new ListNode(5);

        ListNode ret = addTwoNumbers(l1, l2);
        while (ret != null){
            System.out.print(ret.val);

            ret = ret.next;
            if(ret != null){
                System.out.print("->");
            }
        }
        System.out.println("");
    }

    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        //首先定义根节点
        ListNode root = new ListNode(0);

        //再定义前一个节点，开始等于根节点
        ListNode prev = root;

        //最后定义下一个节点
        ListNode next;

        //是否有进位
        boolean isCarry = false;

        //如果传入的列表还有下一个节点 或 存在进位
        while(l1 != null || l2 != null || isCarry){

            //设置下一个节点的值为0
            int num = 0;
            if(l1 != null) {
                //如果传入相加的左边节点不为空，节点值加该节点的值
                num += l1.val;
            }
            if(l2 != null){
                //如果传入相加的右边节点不为空，节点值加该节点的值
                num += l2.val;
            }
            if(isCarry){
                //如果存在进位，节点值加1
                num++;
            }

            //计算是否本次相加存在进位
            isCarry = false;
            if(num > 9){
                isCarry = true;
                //节点值要模10取余数
                num = num % 10;
            }

            //创建下一个节点，并将前一个节点指向下一个节点
            next = new ListNode(num);
            prev.next = next;
            prev = next;

            //传入相加的左边节点指向下一个节点
            if(l1 != null) {
                l1 = l1.next;
            }

            //传入相加的右边节点指向下一个节点
            if(l2 != null) {
                l2 = l2.next;
            }
        }

        //返回根节点的下一个节点，因为根节点是一个虚节点，真正的值在它下一个节点里
        return root.next;
    }
}

```