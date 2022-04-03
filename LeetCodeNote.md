

# LeetCode Note

## Array & HashMap

### 1. [Twp Sum](https://leetcode-cn.com/problems/two-sum/)

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.You may assume that each input would have ***exactly\* one solution**, and you may not use the *same* element twice.You can return the answer in any order.

**Example 1:**

```txt
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

#### 

#### **Solution 1:**  

Do not using hashMap,   Force to sort and using two points, the time complexity is high(O^2), And it cost O(1) Space 

**Cost :**   time O(N^2), space: O(1), N is the length of nums

```kotlin
class Solution {
    fun twoSum(nums: IntArray, target: Int): IntArray {
        val size = nums.size
        for ( i in 0..size){
            for ( j in i+1 until size){
                if ( nums[i] + nums[j] == target){
                    return intArrayOf(i,j)
                }
            }
        }
        return IntArray(0)
    }
}
```

#### **Solution 2:**

 using a HashMap (map in C++, dict in python) , to store the numbers, the time complexity  of finding  "target - x"  can be reduced from O(N) to O(1) .  for each x, we first query the hash table to see if  "target - x" exists, and then insert x into the hash table to ensure that x does not match itself 

**Cost**:  time: O(N), space; O(N), N is the length of nums

```kotlin
class Solution {
    fun twoSum(nums: IntArray, target: Int): IntArray {
        var hash : HashMap<Int, Int> = HashMap()
        for ( i in nums.indices){
            if ( hash.containsKey( target - nums[i])){
                return intArrayOf(hash[target - nums[i]]!!, i)
            }
            hash[nums[i]] = i 
        }
        return IntArray(0)
    }
}
```

## Linked List

### 2. Add Two Numbers 

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

#### **Solution 1:**  

**EXPLANATION**

- WE have to **Traverse Both Lists** & add **sum to new list**.
- **Sum is equivalent to val1 + val2 + carry** from previous Operation.
- THe **resulting node** will be **sum%10.**
- **Carry is updated** by **sum/10** for next Opeartion.

Cost :  **Time Complexity** **O(n).** ,**Space Compelxity** **O(max(l1,l2))**

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        int carry = 0;
        // traverse two linked list 
        while (l1 != null || l2 != null || carry == 1){
            int sum = 0;
            if( l1 != null){
                sum +=  l1.val;
                l1 = l1.next;
            }
            
            if(l2 != null){
                sum += l2.val;
                l2 = l2.next;
            }
            sum += carry;
            carry = sum / 10; // divide 
            ListNode node = new ListNode(sum % 10); // moduloing it 
            cur.next = node;
            cur = cur.next;
        }
        return dummy.next;
    }
}
```

