## レビュー後の修正

### step1(フロイドの検出法)の修正

- if 文の書き方と while 条件式についてレビューをいただいた。
  - if 文を!=null で書くのではなく==null で設定し、その場合は false を返す方が素直 →if 文内にメインの処理(fast の更新)を書く必要もなくなりコードがスッキリした。
  - そうすると while 外の false と合わせることができるので while の条件式を増やして true である場合にのみ行われるループ処理となる。→ ループの条件をどちらも fast に関連する要素が null でないとする

**理解したこと**
if の指摘はメイン処理を行う前に、条件に合わないものを除外するガード的役割を果たすことになる →**ガード句**
while の条件を変更した指摘はコードの見やすさ（リファクタリング？）に直結する。確かにループ内で true、false を返す処理が分岐して書かれているよりわかりやすい。

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

        while (fast != null && fast.next!= null) {//* 条件を増やす
            /*if (fast.next == null) {
                return false;
            }
            */
            fast = fast.next.next;
            slow = slow.next;

            if (fast == slow) {
                return true;
            }
        }

        return false;
    }
}
```

###　 step2,3 の修正(HashSet)

- 命名をもっとわかりやすくする。
  - current という接頭辞は previous/next がついた変数などがあってこそ活きる。これらと対比したいときくらいに使えば良い
  - visitedList は自分の気が抜けていた。確かに訪れているのは node。命名にも気をつけなければいけない。

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
        Set<ListNode> visitedNode = new HashSet<>();//* visitedList
        ListNode node = head;//*currentNode

        while (node != null) {
            if (visitedNode.contains(node)) {
                return true;
            }
            visitedNode.add(node);
            node = node.next;
        }
        return false;
    }

}
```
