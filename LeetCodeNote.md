

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

### 560. Subarray Sum Equals K  

Given an array of integers `nums` and an integer `k`, return *the total number of subarrays whose sum equals to `k`*.

**Example 1:**

```
Input: nums = [1,1,1], k = 2
Output: 2
```

**Example 2:**

```
Input: nums = [1,2,3], k = 3
Output: 2
```

**Solution 1:**  

1. there`s has other time-consuming, so I prefer to use Hash Map to deal this 

2. we can follow the  cumulative sum of all nums elemens,  until we find 
   $$
   sum[i] - sum[j] = k
   $$
    the sum of elements lying between indices i and *j* is k

3. Based on these thoughts, we make use of a hashmap  which is used to store the cumulative sum item
4.  Every time we encounter a new sum, we make a new entry in the hashmap corresponding to that sum, If the same sum item occurs again, we increment the count corresponding to that sum in the hashmap
5. we counted the number of occurrences of [sum-k], that means we alse get the number of times a subarray with sum k, because  *sum - (sum -k) = k*

**Complexity Analysis**

- Time complexity : *O*(*n*). The entire n**u**ms array is traversed only once.
- Space complexity : O(n). Hashmap can contain up to n distinct entries in the worst case.

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int result = 0;
        int sum = 0;
        HashMap < Integer, Integer > map = new HashMap < > ();
        map.put(0,1);
        for (int i=0 ;i < nums.length; i++){
            sum += nums[i];
            
            if (map.containsKey(sum - k)){
                result += map.get(sum -k);
            }
            map.put(sum, map.getOrDefault(sum,0) + 1);
        }
        
        return result;
        
    }
}
```



### 128. Longest Consecutive Sequence TODO



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

