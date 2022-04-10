

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

#### **Solution 1:**  

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



### 128. Longest Consecutive Sequence 

Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*

You must write an algorithm that runs in `O(n)` time

**Example 1:**

``` text
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

#### solution 1

Sorting is valid, but time complexity is O(NLogN). could we have a better solution? yes 

#### solution 2

**Intuition**

It turns out that our initial brute force solution was on the right track, but missing a few optimizations necessary to reach *O*(*n*) time complexity.

**Algorithm**

This optimized algorithm contains only two changes from the brute force approach: 1. the numbers are stored in a `HashSet` (or `Set`, in Python) to allow O(1) lookups, and we only attempt to build sequences from numbers that are not already part of a longer sequence.

2. This is accomplished by first ensuring that the number that would immediately precede the current number in a sequence is not present, as that number would necessarily be part of a longer sequence.

**Complexity Analysis**

- Time complexity : *O*(*n*).

  Although the time complexity appears to be quadratic due to the `while` loop nested within the `for` loop, closer inspection reveals it to be linear. Because the `while` loop is reached only when `currentNum` marks the beginning of a sequence (i.e. `currentNum-1` is not present in `nums`), the `while` loop can only run for n iterations throughout the entire runtime of the algorithm. This means that despite looking like O*(*n*⋅*n*) complexity, the nested loops actually run in O(n + n) = O(n)* time. All other computations occur in constant time, so the overall runtime is linear.

- Space complexity : *O*(*n*).

  In order to set up O(1) containment lookups, we allocate linear space for a hash table to store the O(n)  numbers in `nums`. Other than that, the space complexity is identical to that of the brute force solution.

```Java
class Solution {
    public int longestConsecutive(int[] nums) {
        
        // we need a HastSet to Save time 
        Set<Integer> num_set = new HashSet<Integer>();
        for(int num : nums){
            num_set.add(num);
        }
        
        int longestStreak = 0;
        
        for (int num: num_set){
            // check if its the start of a sequence 
            // if not that means curretn num is not the smallest of the longest Consecutive (the begin one)
            if ( !num_set.contains(num-1)){
                int currentNum = num;
                int currentStreak  = 1;
                while( num_set.contains(currentNum+1)){
                    currentNum += 1;
                    currentStreak += 1;
                }
                
                longestStreak = Math.max(longestStreak,currentStreak);
            }
            
        }
        return longestStreak;
    }
}
```

### 49. Group Anagrams

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

```Java

```

#### solution 1 

**Intuition**

Two strings are anagrams if and only if their sorted strings are equal.

**Algorithm**

Maintain a map `ans : {String -> List}` where each key \text{K}K is a sorted string, and each value is the list of strings from the initial input that when sorted, are equal to \text{K}K.

In Java, we will store the key as a string, eg. `code`. In Python, we will store the key as a hashable tuple, eg. `('c', 'o', 'd', 'e')`.

**Complexity Analysis**

- Time Complexity: O(NK log K) where *N* is the length of `strs`, and *K* is the maximum length of a string in `strs`. The outer loop has complexity *O*(*N*) as we iterate through each string. Then, we sort each string inO*(*K*log*K*) time.
- Space Complexity: O(NK) the total information content stored in`ans`.

```Java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0) return new ArrayList();
        
        Map<String, List> ans = new HashMap<String, List>();
        for (String s:strs){
            
            char[] currentString = s.toCharArray();
            Arrays.sort(currentString);
            String key = String.valueOf(currentString);
            // if currentString not exist, just creat the new List to save s
            if ( !ans.containsKey(key)) ans.put(key,new ArrayList());
            ans.get(key).add(s);
        }
        
        return new ArrayList(ans.values());
    }
}
```



### 19. **Remove Nth Node From End of List**

#### solution 1 

1. get the size of the linked list by walking through entire list.
2. Do `size - n - 1` to see how many steps we need to take before retrive the node we about to remove
3. Walk that many steps and remove the node.
4. Return the head of the list.

```python
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int size = length_of(head);
        if (n == size) {
            return head.next;
        } else {
            int steps = size - n - 1;
            ListNode node = head;
            while (steps-- > 0) {
                node = node.next;
            }
            node.next = node.next.next; // remove node
            return head;
        }
    }

    private int length_of(ListNode head) {
        ListNode n = head;
        int size = 0;
        while (n != null) {
            size++;
            n = n.next;
        }
        return size;
    }
}
```

#### solution 2

fast & slow pointer

1. We will have two pointers which will iterate over the linked list say fast and slow both initalized with head means pointing to the first node.

2. Now we will move the fast pointer to the number specified i.e, if we need to remove the last 2nd node then our fast node must be at last node and slow node must be just before deleting node and if we observe there the difference between both pointer will be of that N node.

   So, Idea is to move the fast pointer N times ahead than slow and then after move both pointer by one till fast reaches last node.

3. Once fast pointer reaches last node we will update the next filed of the node at which slow is pointing.

4. After hat we will free up the space of that deleted node bu making it reference to None to avoid dangling pointer.

5. Then we will return the head pointer.

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        LengthOfList = 0
        fast = slow = head
        for i in range (n):
            fast = fast.next
        if not fast:
            return head.next
        else:
            while(fast.next):
                fast = fast.next
                slow = slow.next
            DeletedNodePointer = slow.next
            slow.next = slow.next.next
            DeletedNodePointer.next = None #Removing dangling pointer
            return (head)
```

### 148 Sort List

Given the `head` of a linked list, return *the list after sorting it in **ascending order***.

- The number of nodes in the list is in the range `[0, 5 * 104]`.
- `-105 <= Node.val <= 105`

#### solution 1 

somehow tricky. Extract all the element into an array, sort and then fill the values in the linkedlist.

```python
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        arr, itr = [], head    # take an array to store all the extracted values
        while itr:             # iterate and copy
            arr.append(itr.val)    
            itr = itr.next
        arr.sort()             # sort it
        itr, i = head, 0       # take i to traverse the array along with linked list
				
        while itr:             # fill the sorted values
            itr.val = arr[i]  
            i += 1
            itr = itr.next
        return head
```

Time Complexity: O(NlogN)
Space Complexity: O(N)

#### solution 2

mergesort, could be devided into three subproblems:

**1. Merge Sort an Array (Leetcode 912)** [Clickhere](https://leetcode.com/problems/sort-an-array/)
**2. Middle of the Linked List( Leetcode 876)** [Clickhere](https://leetcode.com/problems/middle-of-the-linked-list/)
**3. Merge Two Sorted Lists( Leetcode 21)** [Clickhere](https://leetcode.com/problems/merge-two-sorted-lists/)

```python
class Solution:
    def getMid(self, head):            # we will be using two pointers
        slow = fast = head             # what we want? Middle right, but we will stop the loop at one node before the middle
        while fast and fast.next:      # try to think about odd and even length of the list to completely understand the while loop
            fast = fast.next.next
            if fast: slow = slow.next
        mid, slow.next = slow.next, None   # assign None to the last node's next of left half, that is why we broke the loop one node before the middle.
        return mid
        
    def merge(self, left, right):
        head = prev = None    # taking prev pointers to point previous
        while left and right:  
		#Find the minimum value node and change its pointer to next accordingly
            mini = right        
            if left.val < right.val:
                mini = left
                left = left.next
            else: right = right.next
        
            if not head: head = prev = mini      # if head is None assign the head
            else: prev.next = prev = mini        # point the previous next and prev to minimum node just found
        if left: prev.next = left                # adding the remainings of left, while loop could have stopped because of right
        if right: prev.next = right              # adding the remainings of right, while loop could have stopped because of left
        return head
        
    def sortList(self, head: Optional[ListNode]):
        if not (head and head.next): return head     # if lesser than 2 element no need to divide
        mid = self.getMid(head)                      # get the middle node
        left = self.sortList(head)                   # further divide the left part list
        right = self.sortList(mid)                   # further divide the right part of the list
        return self.merge(left, right)               # merge the sorted lists
```

Time Complexity: O(NlogN)
Space Complexity: O(N)

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

