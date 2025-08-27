# Linked List Cycle
* 問題: https://leetcode.com/problems/linked-list-cycle/
* 言語: Java

## Step1

### 思考
問題の言っている意味は理解できたが、解法がすぐに思いつかず５分経過してしまい解説動画を確認した。
かろうじて思いついたのは「現在指しているノードがすでに存在しているか確かめる」という漠然とした方針だったが、
実装まで間に合わなかった。

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        
        ListNode fast = head;
        ListNode slow = head;

        while (fast != null) {
            if (fast.next != null) {
                fast = fast.next.next;
            } else {
                return false;
            }
            slow = slow.next;

            if (fast == slow) {
                return true;
            } 
        }

        return false;
    }
}
```

## Step2

### 参考

- https://github.com/hiroki-horiguchi-dev/leetcode/blob/linkedlist/linkedlist/141_Linked_List_Cycle/141.md
  

最初に解説動画を見てしまったので、step1の時点でフロイドの検出法を実装した。
他の方法で実装できないか探していたところ、hiroki-horighchiさんのコードレビューを読みSetで実装できることがわかった。
Setに訪れたNodeを格納していき、その中に現在のNodeが含まれているか確認するという解法だと理解した。
step1で漠然と立てた方針に1番近い解法だと思う。

#### なぜstep1で思い浮かばなかったのか

HashSetやSet、LinkedListに関する理解が浅かったため。
データに対して何をしたいのか、それに適したデータ構造を選ぶという発想が足りていなかった。

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
import java.util.Set;
import java.util.HashSet;
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> headList = new HashSet<>();//通り過ぎたnodeを重複不可能のsetに格納
        ListNode currentNode = head;//今いるnodeをheadとする

        while (currentNode != null) {
            if (headList.contains(currentNode)) {
                return true;
            }
            headList.add(currentNode);
            currentNode = currentNode.next;
        }

        return false;
    }
    
}
```

## Step3

より効率の良い方法としてstep1で実装したフロイドのアルゴリズム（O(1) 空間）があるが、学習段階ではまず HashSet を用いた「データ構造を選んで問題を解く」練習を優先した。

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
import java.util.Set;
import java.util.HashSet;
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> visitedList = new HashSet<>();
        ListNode currentNode = head;

        while (currentNode != null) {
            if (visitedList.contains(currentNode)) {
                return true;
            }
            visitedList.add(currentNode);
            currentNode = currentNode.next;
        }
        return false;
    }
    
}
```

## 次にやること
Hash Table系、LinkedListの再実装を行い改めてレビュー依頼を出す。