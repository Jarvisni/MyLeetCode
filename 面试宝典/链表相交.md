1、第一想法即循环判断两个链表是否有指针相同：  
```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(!headA || !headB)  return nullptr;
        ListNode* pA=headA;
        ListNode* pB=headB;
        for(;pA!=nullptr;pA=pA->next)
            for(pB=headB;pB!=nullptr;pB=pB->next)
            {
                if(pA==pB)  return pA;
            }
        return nullptr;
    }
};
```
但是这样子效率不太好，改进如下：  
2、大诗人解法  
```C++
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode la = headA;
        ListNode lb = headB;
        while(la != lb){
            //到达链表末尾时，重新走另一条链表的路
            la = la == null ? headB : la.next;
            lb = lb == null ? headA : lb.next;
        }
        return la;
    }
}

//为什么能相遇
//设长链表A长度为LA，短链表长度LB；
//由于速度相同，则在长链表A走完LA长度时，短链表B已经反过头在A上走了LA-LB的长度，剩余要走的长度为LA-(LA-LB) = LB；
//之后长链表A要反过头在B上走，剩余要走的长度也是LB；
//也就是说目前两个链表“对齐”了。因此，接下来遇到的第一个相同节点便是两个链表的交点。
//如果两个链表不存在交点
//就会一直执行到两个链表的末尾，la,lb都为null,也会跳出循环，返回null。
```

作者：pi-qia-cheng  
链接：https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/solution/ni-de-ming-zi-ni-bian-cheng-wo-wo-bian-c-q56d/  
来源：力扣（LeetCode）  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。    
  
  
  
体会：第一次做这个时很晕的一点是竟然采用两个指针去找了结尾来表示，并且循环判断中写了！=end这样意思的判断，深以为戒。面试经历确实可以使人发现自己能力的欠缺很大。痛定思痛认真刷leetcode。  
更醒悟的一点是：简历始终只能是敲门砖，无非是前往下一个站台的路上能够采用的交通方式不同罢了，到了站点都是一样的。往哪走，何去何从，能否留下还是能前往下一个地方。能力技能才是核心竞争力。  
