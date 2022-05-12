# LeetCode Note

## Rules For added Problems and Explanations

1. Problem Title MUST fill a **LeetCode link**

2. Problem description MUST add a **text** or **picture** **example**(if you could)

3. the Answer should include series of Steps 
   - **Approach**: key Words for solving current problems (add **VIDEO** explanation if need)
     - **Intuition** : explanation for problems and ideas of solution to be used (please **NUMBER** each sentence )
     - **Algorithm** **steps** (if need)
     - **Code**: please add the NOTES
     - **Complexity Analysis** (if need)

4. Dedicated letters and formulas: such as `O(N)` or variable: `str` , please using **`ctrl + shift + ` ` to format it** 

   - Such as:  `leftPointer` , `nums[a] > nums[b]`

   

## Array & HashMap

### 1. [Twp Sum](https://leetcode.com/problems/two-sum/)

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.You may assume that each input would have ***exactly\* one solution**, and you may not use the *same* element twice.You can return the answer in any order.

**Example 1:**

```txt
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

#### **Solution 1:**   Brute Force

**intuition**

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

#### **Solution 2:** Hash

**intuition**

 using a `HashMap` (map in C++, dict in python) , to store the numbers, the time complexity  of finding  "target - x"  can be reduced from `O(N) to O(1)` .  for each x, we first query the hash table to see if  "target - x" exists, and then insert x into the hash table to ensure that x does not match itself 

**Cost**:  time: `O(N)`, space; `O(N)`, N is the length of `nums`

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

### [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)  

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

#### **Solution 1:**  cumulative sum+hash

**intuition**

1. there`s has other time-consuming, so I prefer to use Hash Map to deal this 

2. we can follow the  cumulative sum of all nums elemens,  until we find 
   $$
   sum[i] - sum[j] = k
   $$
    the sum of elements lying between indices i and *j* is k

3. Based on these thoughts, we make use of a hashmap  which is used to store the cumulative sum item
4.  Every time we encounter a new sum, we make a new entry in the `hashmap` corresponding to that sum, If the same sum item occurs again, we increment the count corresponding to that sum in the `hashmap`
5. we counted the number of occurrences of [sum-k], that means we also get the number of times a `subarray` with sum k, because  *`sum - (sum -k) = k`*

**Complexity Analysis**

- Time complexity : *O*(*n*). The entire n**u**ms array is traversed only once.
- Space complexity : `O(n)`. `Hashmap` can contain up to n distinct entries in the worst case.

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



### [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) 

Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*

You must write an algorithm that runs in `O(n)` time

**Example 1:**

``` text
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

#### solution 1： Sorting

Sorting is valid, but time complexity is O(NLogN). could we have a better solution? yes 

#### solution 2： hash+Greedy

**Intuition**

It turns out that our initial brute force solution was on the right track, but missing a few optimizations necessary to reach *O*(*n*) time complexity.

**Algorithm**

1. This optimized algorithm contains only two changes from the brute force approach:
2.  the numbers are stored in a `HashSet` (or `Set`, in Python) to allow O(1) lookups, and we only attempt to build sequences from numbers that are not already part of a longer sequence.
3. This is accomplished by first ensuring that the number that would immediately precede the current number in a sequence is not present, as that number would necessarily be part of a longer sequence.

**Complexity Analysis**

- Time complexity : *O*(*n*).

  Although the time complexity appears to be quadratic due to the `while` loop nested within the `for` loop, closer inspection reveals it to be linear. Because the `while` loop is reached only when `currentNum` marks the beginning of a sequence (i.e. `currentNum-1` is not present in `nums`), the `while` loop can only run for n iterations throughout the entire runtime of the algorithm. This means that despite looking like O*(*n*⋅*n*) complexity, the nested loops actually run in `O(n + n) = O(n)`* time. All other computations occur in constant time, so the overall runtime is linear.

- Space complexity : *O*(*n*).

  In order to set up `O(1)` containment lookups, we allocate linear space for a hash table to store the `O(n)`  numbers in `nums`. Other than that, the space complexity is identical to that of the brute force solution.

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

### [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```



#### solution 1 : Sorting

**Intuition**

Two strings are anagrams if and only if their sorted strings are equal.

**Algorithm**

Maintain a map `ans : {String -> List}` where each key \text{K}K is a sorted string, and each value is the list of strings from the initial input that when sorted, are equal to `K`.

In Java, we will store the key as a string, eg. `code`. In Python, we will store the key as a hashable tuple, eg. `('c', 'o', 'd', 'e')`.

**Complexity Analysis**

- Time Complexity: `O(NK log K)` where *N* is the length of `strs`, and *K* is the maximum length of a string in `strs`. The outer loop has complexity *O*(*N*) as we iterate through each string. Then, we sort each string in`O(KlogK)` time.
- Space Complexity: `O(NK)` the total information content stored in`ans`.

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



#### solution 2: hash

in the solution 1, if one String element is so long,  that will be very time Consuming , so we using character counts  to be the Keys in `HashMap`

**Intuition**

Two strings are anagrams if and only if their character counts (respective number of occurrences of each character) are the same.

**Algorithm**

We can transform each string s into a character count List, the  Count List consisting of 26 non-negative integers representing the letter from  a to z . We use these counts as the keys for our hash map.

**Complexity Analysis**

- Time Complexity: `O(NK)`, where N is the length of  giving  `strs` List, and `K` is the maximum length of a element in `strs`. Counting each string is linear in the size of the string, and we count every string.
- Space Complexity: `O(NK)`, the total information content stored in `ans`.

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        ans = collections.defaultdict(list)
         
        for s in strs:
            count = [0] * 26  # a ...Z
            
            for c in s:
                count[ord(c) - ord('a')] += 1
            ans[tuple(count)].append(s)
        
        return ans.values()
```



### [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

Given an array, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

#### [Approach 1: three reverses](https://www.youtube.com/watch?v=gmu0RA5_zxs)

**Intuition**

1. we want the last k elements move to the front, we can though multi-reverse the array to get it 

2. there are three steps :

   1. | After reverse all numbers       | 7 6 5 4 3 2 1 |
      | ------------------------------- | ------------- |
      | After  reverse  first k numbers | 5 6 7 4 3 2 1 |
      | After reverse  last n-k numbers | 5 6 7 1 2 3 4 |

      

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size(); // make sure k is legal
        myReverse(nums,0,nums.size()-1);
        myReverse(nums,0,k-1);
        myReverse(nums,k,nums.size()-1);
        
    }
    
    void myReverse(vector<int>& nums, int start, int end){
        while (start < end){
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
};
```

**Complexity Analysis**

- Time complexity : `O(n)`. The entire n**u**ms array is traversed only once.
- Space complexity : `O(1)`.  uses only constant extra space. 



### [763. Partition Labels](https://leetcode.com/problems/partition-labels/)

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return *a list of integers representing the size of these parts*.

**Example 1:**

```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
```

**Example 2:**

```
Input: s = "eccbbbbdec"
Output: [10]
```



#### [Approach 1:  Maps+Greedy](https://www.youtube.com/watch?v=5NCjHqx2v-k)

**Intuition**

1. ​	two key points !
   1. one particular letter in one particular part, we split it after the last occurance of any letter we visited
   2. split String as many parts as possible 
2.  Find the last occurrence of any letter in every part/splits,  for letter `a` The first partition must include it, and also the last occurrence of `a`
3. `HashMap`:  to count the occurrence of letters 
4. Greedy Algorithm could make sure two things : 
   1. find the longest part/split
   2. find as many parts as possible

```C++
vector<int> ans;
        vector<int> lastMap(26,0);
        // record the position of every letter's last occurrence
        for(int i = 0; i < s.length();  ++i){
            lastMap[s[i] - 'a'] = i;
        }
        
        //  let anchor and j be the start and end of the current partition.
        // [archor,j] is our wanted Labeled Partition 
        int j = 0, archor = 0;
        for( int i = 0 ; i < s.length();  ++i){
            //extending the current partition [anchor, j] as longer as possible.
           j = max(j, lastMap[s[i] - 'a']);
           //  we at end of current partion
        // the last occurrence with longest partion showing up
           if( i == j){
               ans.push_back(i - archor + 1);
               archor = i + 1;
           }
        }
        
        return ans;
```

**Complexity Analysis**

- Time Complexity: `*O*(*N*)`, where *`N`* is the length of *`S`*.
- Space Complexity: `*O*(1)` . For  keep data structure `lastMap` which is not more than 26 characters.

### [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

Given an unsorted integer array `nums`, return the smallest missing positive integer.

You must implement an algorithm that runs in `O(n)` time and uses constant extra space.

**Example 1:**

```
Input: nums = [1,2,0]
Output: 3
```

**Example 2:**

```
Input: nums = [3,4,-1,1]
Output: 2
```



#### [Approach 1: self-Hash](https://www.youtube.com/watch?v=8g78yfzMlao)

**Intuition**：

1. It's easy to think of **sorting** array to find the first missing positive value, but sorting would bring `N*LogN`  time Complexity, we're allow to run in `O(n)` time  and constant extra space (Cannot use `HashSet`), **that's why it's a Hard level problem**
2. no matter what our input array is, no matter what the smallest missing positive value is,  the answer integer n should belong to the set `from 1 to length(array) + 1`, `which n between [1, length(array) + 1]`
3. if we have a Hash Set to memory each value. The we could  just brute force to go through all of these values,  try each of them is right
4. we can using some tricks to rebulid the giving array as our Hash Set, so it's not need extra space： 
   1. Write our own hash function
   2. the rule is: map the value `nums[i]` to  index of `i-1` 
   3. three cases we have to handle : negative, zero, positive 

```C++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        // 1. set each negative integer to 0
        for(int i = 0 ;i < nums.size(); ++i){
            if ( nums[i] < 0 ) nums[i] = 0;
        }

        for(int i = 0; i < nums.size(); ++i){
            // 2. mark the positive integer into negetive integer
            // 3. mark the 0 into -1*(nums.size()+1)
            // 3. make sure val in [1,size+1) 
            int val = abs(nums[i]);
            if ( 1 <= val && val <= nums.size()){
                if(nums[val - 1] > 0){
                    nums[val - 1] = nums[val - 1] * -1;
                } else if ( nums[val - 1] == 0){
                    nums[val - 1] =  -1 * (nums.size() + 1);
                }
            }
        }
        // after self-hash, all positvie number and 0 had been hash to negative
        for( int i = 1; i < nums.size() + 1; ++i){
            // if find a positive one, that means we find the missing smallest positive integer ,which's escaped our hash operation 
            if( nums[i - 1] >= 0)
                return i;
        }
        return nums.size() + 1;
    }
};
```



## Backtracking & Recursion & Memory Search

### Step of Backtracking

Please Remember the Step of Backtracking

```C++
1. exit of backTracking  
2. make choice in the backTracking tree
3. backTrack to next level of the tree
4. go back to the previous level 
```



### [17. Letter Combinations of a Phone Number](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

#### Approach : map+Backtracking 

**Intuition**

1. NOTICE that **any order**  of  **combinations** from the problem description , that's a big clue/hint/sign of **Backtracking**
2. for the first number, there are `3` options, and these `3` options has there own 3 options for the second number and so on
3. The combinations from the first to the last will expand into a **recursive tree**.
4. just imagine that **backtracking tree** start from one of numbers, for example,  Take the `"234"` for example, look at the backtracking  tree:![image-20220511234214130](./LeetCodeNote.assets/image-20220511234214130.png)

5. When the index **reaches the end of digits**, we get a combination, and add it to the result, end the current recursion. Finally we will get all the combinations.



```C++
class Solution { 
public:
    vector<string> res;
    string combination;
    vector<string> letterCombinations(string digits) {
        
        if (!digits.size()) return res;
         map<char,string> mp = {
            {'2',"abc"},
            {'3',"def"},
            {'4',"ghi"},
            {'5',"jkl"},
            {'6',"mno"},
            {'7',"pqrs"},
            {'8',"tuv"},
            {'9',"wxyz"}};
        // backtracking starts from the first element
        backTrack(digits,0,mp);
        return res;
        
    
    }
    
    void backTrack(string digits, int curIndex, map<char,string>& mp){
        // 1. exit for backTracking  
        // curIndex reached the end of string digits
        if(curIndex == digits.size()){
            res.push_back(combination);
            return;
        }  
        // get current number
        char digit = digits[curIndex];
        // tranvers all letters of the digit represented
        for(auto& letter: mp[digit]){
            // 2. make choice in the backTracking tree for combination
            combination.push_back(letter);
            // 3. backTrack to next level of the tree
            backTrack(digits,curIndex + 1,mp);
            // 4. go back to the previous level 
            combination.pop_back();
        }
    }
};
```


**complexity analysis**

There's huge time and space required for backtracking. because we want to generate all of combination, that means that we need to enumerate each possible results

1. **Time Complexity**: `O(3^M*4^N)` , M = amount of numbers which has three letters(2,3,4,5,6),   N = amount of numbers which has four letters(7,9) 
2. **Space Complexity**: `O(3^M*4^N)` : YES lots of **stack** space required 



### [22.  Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

**Example 1:**

```C++
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```C++
Input: n = 1
Output: ["()"]
```



#### [Approach : backtracking](https://www.youtube.com/watch?v=s9fokUqJ76A)

**Intuition** :

1. NOTICE that  **combinations**  from the problem description , that's a big clue/hint/sign of using **Backtracking** algorithm
2. let's add `(` or `)` when we know it will remain a **valid** sequence. **we need to make sure open brackets can match the close brackets** 
3. we **can't have a close parentheses come before a open parenthesis: `eg: ) (`**
4. We can do this by keeping track of the number of  open`(` and colse`)` parenthesis we have placed so far.
   1. the Count of  colseing  parenthesis  <  the count of opening parenthesis

```C++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        // we need a tmp string "" to store the combination
        backtrack(res,"",0,0,n);
        return res;
    }

    void backtrack(vector<string> &res, string str,int openCount, int closeCount, int n){
        // it means all brackets used up, then we get one combination 
        if(openCount == n and closeCount == n){
            res.push_back(str) ;
            return ;
        } 
        // here we keep tracking the amount of "(" and ")" by using two variables
        // leftIndex < n : means  we can added a "(" 
        if(openCount < n) backtrack(res,str+'(', openCount+1,closeCount,n);
        // rightIndex < left: means there's has a "(",so we can added a ")" 
        if(closeCount < openCount)backtrack(res,str+')', openCount, closeCount+1,n);
    }
};
```

**Complexity Analysis**:

Our complexity analysis rests on understanding how many elements there are in `generateParenthesis(n)`. This analysis is outside the scope of this article, but it turns out this is the `n`-th Catalan number $ \dfrac{1}{n+1}\binom{2n}{n}$, which is bounded asymptotically(渐进的) by $\dfrac{4^n}{n\sqrt{n}}$  

- Time Complexity : O($\dfrac{4^n}{\sqrt{n}}$). Each valid sequence has at most `n` steps during the backtracking procedure.
- Space Complexity : O($\dfrac{4^n}{\sqrt{n}}$), as described above, and using `O(n)` space to store the sequence.



## Binary Search

### [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of* `target` *if it is in* `nums`*, or* `-1` *if it is not in* `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

[Video Explanation Link](https://www.youtube.com/watch?v=QdVrY3stDD4) （not elegant enough）



#### Approach 1:  modified Binary Search:

**Intuition**

1. the demanded complexity is O(log n), it`s a clue of using  Binary Search, and it's a Modified Binary Search problem
2. the pivot must be the smallest element, we can Divide array into sorted parts at the  pivot,  then we can using BS to Search target in two parts; and we do not need to find  this pivot  particularly;
3. Just Using Binary Search to Divide the array into two parts, one of them must be sorted, the other may be sorted or not. 
4. At this time, the sorted part is searched by binary method. 
5. The unsorted part is then divided into two parts, one of which must be sorted, the other may be sorted or Not.  
6. Repeat the algorithm like that , until we find the target 

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.size() == 0) return -1;
        
        int left = 0, right = nums.size()-1;
        
        // right = size -1, so left must be less or equal right
        while (left <= right){
            // use bit operation to avoid integer Overflow
            int mid = left + ((right - left)>>1);
            // finded the target 
            if (nums[mid] == target) return mid;
            // check which part is sorted 
            if (nums[mid] >= nums[left]){
                // jump into left part Whatever it`s sorted 
                // keep dividing the array until every parts are sorted for using BS （alreay finded pivot）
                
                if(nums[left] <= target && target < nums[mid]) right = mid - 1;
                else left = mid + 1;
                
            }else{
                if(nums[mid] < target && target <= nums[right]) left = mid + 1;
                else right = mid - 1;
            }
        }
        
        return -1;
    }
};
```

**Complexity analysis**

- Time complexity: $O(Log N)$
- Space complexity: $O(1)$ extra space. Only constant level  space required. 



### [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

[Video Explanation Link](https://www.youtube.com/watch?v=bU-q1OJ0KWw)

#### Approach: BF

**Intuition**

1. the demanded complexity is O(log n), there's noting  `O(log n)` runtime complexity except for Binary Search
2. it is a variation of regular Binary Search problem, the key is to control the index boundary of Binary Search

```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res;
        int left = binarySearchLeft(nums,target);
        int right = binarySearchRight(nums,target);
        res.push_back(left);
        res.push_back(right);
        return res;
    }
    
    int binarySearchLeft(vector<int>& nums, int target){
        // almost rigular BS
        int left = 0, right = nums.size() -1;
        int ans = -1;
        while (left <= right){
            // use bit operation to avoid integer Overflow
            int mid = left + ((right - left)>>1);
            // using bigger than or equal to move the index to leftMost target index 
            // it would`t stop at index[3]. index will keep move to left part array to find leftmost one    
            // [1,8,8,8,8,8,8]
            if( nums[mid] >= target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
            
            if(nums[mid] == target) ans = mid;
        }
        
        return ans;
    }
    // the Search logic of leftSearch and rightSearch is just same 
    int binarySearchRight(vector<int>& nums, int target){
        int left = 0, right = nums.size() -1;
        int ans = -1;
        while (left <= right){
            int mid = left + ((right - left)>>1);
            if( nums[mid] <= target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
            
            if(nums[mid] == target) ans = mid;
        }
        return ans;
    }
};
```

**Complexity analysis**

- Time complexity: $O(Log N)$
- Space complexity: $O(1)$ extra space. Only constant level space required. 



### 81.Search in Rotated Sorted Array II

#### Approach: Binary Search

the detail is that moving the left pointer and right pointer to delete the repeating elements

```C++
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int n = nums.size();
        if (n == 0) return false;
        if (n == 1) return nums[0] == target;
        int left = 0;
        int right = n-1;
        while (left <=right){
            int mid = (left+right) >>1;
            if (nums[mid]== target) return true;
            if(nums[mid] == nums[left] && nums[mid]== nums[right]){
                left ++;
                right --;
            }
            else if (nums[left] <=nums[mid]){
                if (nums[left] <= target && nums[mid] > target) right  = mid -1;
                else left = mid +1;
            }
            else {
                if (nums[right] >=target && nums[mid] < target) left = mid +1;
                else right = mid -1;
            }
        } 
        return false;
    }
};
```



###  [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.

- The first integer of each row is greater than the last integer of the previous row.

  

#### Approach: Binary Search

now it has two solution to find the target position

1. using twice binary searching :the first binary searching is to find the position of last small than or equal to the target , the second binary searching is to find the position of the target in the column 

2. `2d` matrix change to the `1d` matrix ,the details of `solution2` is to `1d` & `2d` conversion（transform）.how the abstract mid position is to transform the actual position .

```c++
class Solution {
  public:

     bool searchMatrix(vector<vector<int>>& matrix, int target) {
        // 2d to 1d
        int m = matrix.size();
        int n = matrix[0].size();

        int left = 0;
        int right = m*n-1;
        while(left <=right){
            int mid = left + ((right - left) >> 1);
            if (matrix[mid/n][mid%n] == target){
                return true;
            }
            else if (matrix[mid/n][mid%n] > target) {
                right = mid-1;
            }
            else left = mid+1;
        }
        return false;
    }
};
```

### [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of **unique** elements, return *the minimum element of this array*.

You must write an algorithm that runs in `O(log n) time.`

**Example 1:**

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

#### Approach: Binary Search

**Intuition**

 - the Minimum is the division point of the input array. it means that the `nums[m+1]> nums[m]` and `nums[m-1] > nums[m]`.
 - using two pointer to cut half of the input array.
 - the input array is divised by two sort array.

``` c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int left = 0;
        int right = n-1;
        if (nums[left] <= nums[right] || n ==1) return nums[0];
        while (left <= right){
            int mid = (left+right)/2;
            if (nums[mid] < nums[mid-1]) return nums[mid];
            else if (nums[mid] > nums[mid+1] ) return nums[mid+1];
            
            else if (nums[mid] > nums[left]) left = mid+1;
            else right = mid -1;
        }
        return -1;
    }
};
```

### [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

Write an efficient algorithm that searches for a value target in an `m x n` integer matrix matrix. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/24/searchgrid2.jpg)

```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
Output: true
```

#### Approach 1: binary search+ two pointer

**Intuition**

In this problem , we should focus two details:

-   it is not absolutely ordered .it is relatively ordered.
-   how to find the division point in the `2d` matrix
    So the points are :
-   The top left element is the relative (abstract) middle value of `2d` matrix .
-   abstract binary searching is to move column and line step by step

``` c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        if (m == 0)return false; 
        int n = matrix[0].size();
        int left =0;
        int right = n-1;
        while(left <m && right >=0){
            if(matrix[left][right] == target) return true;
            else if(matrix[left][right] > target) right --;
            else left ++;
            
            if (left >=m) return false;
            if (right <0) return false;
        }
        return false;
    }
};
```

### [4. Median of Two Sorted Arrays](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

#### Approach : Binary Search

**Intuition**

1. First of all, we make sure `first input array` is always greater than or equal to `second input array`
2. The idea is to imagine the combined sorted array, with a total size of `size_total = nums1.size() + nums2.size()`
3. Now we divide the combined array into two parts.
   1. if `odd` then first part has size of `size_total/2` and second part has size of `size_total/2+1`
   2. if `even` then both first part and second part will have `size_total/2` elements

4. now we define first part as the first size_total/2 elements of combined sorted array
5. Our binary search is to find **the number of elements from the `second input array` that are in `first part`**
6. This value ranges between [0, size of `second input array`]



```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int len1 = nums1.size(), len2 = nums2.size();
        // final res between in [0, halfSizeOfCombined]
        int halfSizeOfCombined = (len1+len2 + 1) >>1; 
        // if nums1.length > nums2.length --> swap the input, because we need a longer array for the seconed array
        if( len1 > len2) return findMedianSortedArrays(nums2, nums1);
        int low = 0, high = len1-1; 
        
        // Binary Search to find the min1_index(middle point of nums1)
        while(low <= high){
            int mid1 = (low+high)>>1;
            int mid2 = halfSizeOfCombined - mid1;
            if( nums1[mid1] < nums2[mid2-1])
                low = mid1 + 1;
            else 
                high = mid1 - 1;
        }
        // the nums1's mid = low, SO the nums2's mid = the Subtrack
        int mid1_index = low ;
        int mid2_index = halfSizeOfCombined - mid1_index;
        //calculate the oddMid and evenMid (for the merged big array)
        double oddMid = max( mid1_index>0? nums1[mid1_index-1]: INT_MIN,
                          mid2_index>0? nums2[mid2_index-1]: INT_MIN);
        double evenMid = min( mid1_index < len1?  nums1[mid1_index]: INT_MAX,
                           mid2_index < len2? nums2[mid2_index]:INT_MAX);
        if( (len1+len2) & 1 == 1) return oddMid;
        return (oddMid+evenMid)/2;
        
    }
};
```

**Complexity Analysis**

- Time complexity : `O(Log(M+N))`. because it's Binary Search for the combined two array
- Space complexity : `O(1)`.

## Fast Slow  Pointers: Floyd's Cycle-Finding 

### [457. Circular Array Loop](https://leetcode.com/problems/circular-array-loop/)- infrequently

You are playing a game involving a **circular** array of non-zero integers `nums`. Each `nums[i]` denotes the number of indices forward/backward you must move if you are located at index `i`:

- If `nums[i]` is positive, move `nums[i]` steps **forward**, and
- If `nums[i]` is negative, move `nums[i]` steps **backward**.

**Example 1:**

```
Input: nums = [2,-1,1,2,2]
Output: true
Explanation:
There is a cycle from index 0 -> 2 -> 3 -> 0 -> ...
The cycle's length is 3.
```

**Example 2:**

```
Input: nums = [-1,2]
Output: false
Explanation:
The sequence from index 1 -> 1 -> 1 -> ... is not a cycle because the sequence's length is 1.
By definition the sequence's length must be strictly greater than 1 to be a cycle.
```







### [234 Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

Given the `head` of a singly linked list, return `true` if it is a palindrome.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
Input: head = [1,2,2,1]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
Input: head = [1,2]
Output: false
```



#### [Approach 1:  using Fast&Slow Pointer to reverse list](https://www.youtube.com/watch?v=yOzXms1J6Nk) 

**Intuition**

1. we can using an array to store the elements in singly linked list, Use indices as pointers, one from left to right and one from right to left, and then check if the element values are equal. it`s a easy solution, but it need extra memory ! not good enough
2. actually, this problem is a hybrid version from reverse Singly linked list  and fast&slow pointers 
3. let's Break the problem into two parts:  
   1. find the mid position of Linked list and divide list into 2 parts at mod position  
   2. reverse the left parts list  and  two parts all start from head to check every Node' value is equal or not 

4. Actually, we can reverse list and find the mid pointer at same time ! 

```python
 class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        
        slow = head 
        fast = head 
        
        
        # make sure ther are enough node for us shift at least
        
        # find the midPoint 
        while fast and fast.next :     
            slow = slow.next
            fast = fast.next.next
            
        #reverse the seconed half
        prev = None 
        while slow:
            cur = slow.next
            slow.next = prev
            prev = slow
            slow = cur
        
        # check the value of two parts
        left, right = head, prev 
        while right :
            if left.val != right.val:
                return False 
            left = left.next
            right = right.next
        
        return True
            
```

**Complexity analysis**

- Time complexity: $O(N)$
- Space complexity: $O(1)$ extra space. Only constant space required. 



### [202. Happy Number](https://leetcode.com/problems/happy-number/)

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.

Return `true` *if* `n` *is a happy number, and* `false` *if not*.

**Example 1:**

```
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```



#### [Approach 1:  Fast&Slow Pointer](https://www.youtube.com/watch?v=ljz85bxOYJ0) : Floyd's Cycle-Finding Algorithm

**Intuition**

1. Let's start with the number 19,  look at whole conduction chain, it's a *implicit* **`LinkedList`** .   *Implicit* means it don't have actual `LinkedNode's` and pointers, but the data still formed a `LinkedList` structure.  

   ```
   19 -> 82 -> 68 -> 100 -> 1 
   ```

2. Although *implicit* `LinkedList`  has no real next pointers, but we can using our computational function instead of the real next pointer。 

3. Now the problem turns into how to judge the linked list has a cycle

4. 1.  Floyd Cycle-Finding Algorithm ( `Fast Slow pointers`)： If `n` *is* a happy number, there is no cycle, then  eventually  the pointer cat get to 1
   2.  using hash set to record whether the element has been visited

```Python
class Solution:
    def isHappy(self, n: int) -> bool:
        
        def myNext(n):
            res = 0
            while n:
                num = n % 10 # mod the every digits to get one's places value
                res +=  num**2 
                n = n // 10  # get the next digit
            return res
        
        slow = myNext(n)
        fast = myNext(myNext(n))
        while slow != fast:
            slow = myNext(slow)
            fast = myNext(myNext(fast))
        
        return slow == 1
```

**Complexity analysis**

- Time complexity: $O(Log N)$
  - the computatiom of myNext(n) Complexity has a litte hard, but it  is almost $O(Log N)$
  - If there is no cycle, then the fast runner will get to 1, and the slow runner will get halfway to 1； Because there were 2 runners instead of 1, we know that at worst, the cost was O($2 \cdot \log n$) = O($\log n$)
- Space complexity: $O(1)$ extra space. Only constant space required. 





### [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return *this repeated number*.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3
```

#### Approach 1:  hashMap  or Sorted  

**Intuition**

 It's very easy to using hashMap or Sorted this array to solve this problem, but these solving are not elegant enough, and  it need to consume more time or space 

#### [Approach 2: Marking visited value within the array](https://www.youtube.com/watch?v=wjYnzkAhcNk)

**Intuition**

1. Since all values of the array are between `[1..n]` and the array size is `n+1`， that`s means we can using the value be  the index of giving array. 
2. the array is loop linked array, so we can using nums[i] be the index to Skip search the Duplicate Number
3. while scanning the array from left to right, we set the `nums[n]` to its negative value.
4. when the Duplicate value occurs for the second times, the Duplicate value already been negative



```C++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {  
        for (int i = 0; i < nums.size(); ++i){
            // get the SkipIndex will go faster
            int skipIndex = abs(nums[i]);
            // if nums[skipIndex] < 0, that means the skipIndex as a value already visited in array
            if (nums[skipIndex] < 0){
                return skipIndex;
            }
            // using - to mark every element 
            nums[skipIndex] = -nums[skipIndex];  
        }
        return -1;  
    }
};
```





## Linked List

### [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) 

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

#### **Solution 1:**  Linked Like Sum+two Pointers

**EXPLANATION**

- WE have to **Traverse Both Lists** & add **sum to new list**.
- **Sum is equivalent to val1 + val2 + carry** from previous Operation.
- The **resulting node** will be **sum%10.**
- **Carry is updated** by **sum/10** for next `Opeartion`.

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

### [19. **Remove Nth Node From End of List**](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

#### solution 1 : ArrayCount

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

#### solution 2: Fast-slow pointer (Folyd's Cycle Finding)

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



## Math Tricks

### [7. Reverse Integer](https://leetcode.com/problems/reverse-integer/solution/)

Given a signed 32-bit integer `x`, return `x` *with its digits reversed*. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-231, 231 - 1]`, then return `0`.

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned).**

**Example 1:**

```
Input: x = 123
Output: 321
```

#### Approach : Pop and Push Digits  & Check before Overflow

**intuition**

1.  We can build up the reverse integer one digit at a time.
2. Reversing an integer can be done similarly to reversing a string.
3. we want to repeatedly `pop` the last digit of `x` and `push` it to the back of our `result` . in the end, `result` will be the reverse of the `x`
4. In Fact, we don't really need a real stack. We can using math operation to instead of  a real stack
5. However, this approach is dangerous, because the statement  `temp = result*10 + pop` may can cause overflow

```C++
class Solution {
public:
    int reverse(int x) {
        int res = 0;
        while (x != 0){
            // make sure res avoid causing a overflow
            // becuase if res < INT_MIN / 10, that means the next shifted res < INT_MIN 
            // similr to reach the upper eage case 
            if (res < INT_MIN / 10 || res > INT_MAX / 10){
                return 0;
            }
            // pop : % could be a pop operation
            // push : (res*10 + poped val) is kind of push operation
            res = res * 10 + x % 10;
            // move to next digit
            x /= 10;
        }

        return res;
    }
};
```

**Complexity Analysis**

- Time Complexity: `O(log(x)).` There are roughly $ \log_{10}(x) $  digits in `x`.
- Space Complexity: `O(1)` . constant space required





### [13. Roman to Integer](https://leetcode.cn/problems/roman-to-integer/)

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

**Example 1:**

```
Input: s = "III"
Output: 3
Explanation: III = 3.
```

**Example 2:**

```
Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```



#### Approach: Follow the rules

**Intuition**

1. Using a tiny `HashMap` to store these Symbol and Value
2. Here I basically check if the next number is larger then the current one. If so, I subtract the current number and move to the next.
3. As soon as there is only one number left, that one is simply added to the result .

```python
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        mp = {'I':1, 'V':5 , 'X':10, 'L':50, 'C':100,  'D':500, 'M':1000}
        
        res = 0
        # scan s
        for i in range(len(s)):
            # if the current integer less than next integer,just subtract the current integer
            if  i+1 < len(s) and mp[s[i]] < mp[s[i+1]]:
                res -= mp[s[i]]
            else:
                res += mp[s[i]]

        return res    
```

**Complexity Analysis**:

too easy! It's not necessary to fill that



## Sliding Window

### [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```



#### Approach 1: Brute Force

**Intuition**

Check all the substring one by one to see if it has no duplicate character.

**Algorithm**

1. iterate（Brute tranvers） through all the possible substrings of the given string `s`
2. To check one of the subStrings has duplicate characters or not, we can use a set

**Complexity Analysis**

Time complexity : `O(n^3)`.

#### Approach 2: Sliding Window.

**Intuition**

1. The naive approach is very straightforward. But it is too slow. So how can we optimize it?

2. if there exist a  `subString  S(i,j)`  respond to  the index `i`  to `j-1` and it is already checked to have no duplicate characters, we only need to check current character `S[j]` is already in `subString S[i,j]`  or not. 

3. It`s easy to think about to use a Hash Set as a Sliding window,(Sliding  window reduce the Checking time cost to  `O(1)` )

**Algorithm**

1. Sliding window has two pointer, Left pointer .right pointer, 
2. using this two point to maintain a `window [i,j) (left-closed, right-open).` 
3. and hash Set can be the container of Sliding window

```C++
class Solution{
public :
    int lengthOfLongestSubstring(string s) {
         // key=charters,value = the number of key
        unordered_map<char,int> mp;
        int res = 0, left = 0, right = 0;
        
        for( ;right < s.size(); ++right){
            mp[s[right]]++;
            // if find duplicate character, left move one step, window shrinked 
            while( mp[s[right]] > 1){
                mp[s[left]]--;
                left++;
            }
            // after clean all duplicate  characters ,then cal the legth
            res = max(res, right-left+1);
        }
      	
        return res;
    }
};
```

**Complexity Analysis**

- Time complexity : `O(2n) = O(n)`. In the worst case each character will be visited twice by i and j.
- Space complexity : `O(min(m, n))`, Same as the previous approach. We need space for the sliding window, where k is the size of the `Set`. The size of the Set is upper bounded by the size of the string *n* and the size of the `charset` / alphabet `m`.



### [209.  Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a **contiguous subarray** `[numsl, numsl+1, ..., numsr-1, numsr]` of which the sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```tex
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

#### Approach 1 Brute force : 

**Intuition**

Do as directed in question. Find the sum for all the possible subarrays and update the ans when we get a better subarray that fulfill the requirements$ (sum≥ target).$

Time complexity: `O(n^3).`

#### Approach 2: Sliding Window.

**Intuition**

1. Until now, we have kept the starting index of subarray fixed, and found the last position. Instead, we could move the starting index of the current subarray as soon as we know that no better could be done with this index as the starting index. 

2. We could keep Sliding Window to cumulate the item sum , and make optimal moves so as to keep the sum greater than target as well as maintain the lowest size possible.

**Algorithm**

- **Algorithm**
  - Initialize $ left$ pointer to 0 and $sum$ to 0
  - Iterate over the $nums$
    - Add $nums[i]$ to $sum$ ： add num to Window
    - move windows to the end by giving rules 

```c++
int minSubArrayLen(int target, vector<int> &nums){
    int ans = INT_MAX;
    int left = 0, windowSum = 0;
    for(int right =0; right < nums.size(); ++i){
        windowSum += nums[right];
        while (windowSum >= target){ 
            ans = min(ans,right-left+1);
            windowSum -= nums[left++];
        }
    }
    return (ans != INT_MAX) ? ans : 0; 
}
```

**Complexity analysis**

- Time complexity: $O(N)$
  - Each element can be visited at most twice, once by the right $pointer(i)$ and (at most) once by the $ left pointer$.
- Space complexity: $O(1)$ extra space. Only constant space required. 



### [76. Minimum Window Substring](https://leetcode-cn.com/problems/minimum-window-substring/)

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2:**

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window
```

#### Approach:  sliding window + hash+ String Match+ Greedy

**Intuition** 

1. A **substring** is a contiguous sequence of characters within the string.
2. We slide the window on `s(longer String)` expanding the window by moving the `right pointer.` When the window contains all the characters required by `t(shorter string)`, if it can be shrunk, we shrink the window until we get the smallest window.

**Algorithm** :

1. At the same time initialize the `left` and `right` pointer  point to index 0

2. The `right pointer` move forward to the right until it matches `t`

3. At this point, push the `left pointer` again to shrunk the window until it does not match

4. `Right pointer` ends, then all is  end

5. How to match `window` and `t` ?  

   1. through The hash table counts the number of characters that appear (two counters: `window` `need`) 

6. How to compare all matching substrings?

   1. Greedy: use  `[start:end]` to save the shortest substring, and `min_len` to save the length of the shortest substring

   

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        
        window,need = dict(),dict()
        for item in t:
            need[item] =need.get(item,0) + 1

        left = 0
        res = ''
        match,minlen = 0, float('INF')

        for right in range(len(s)):
            if s[right] in need:
                window[s[right]] = window.get(s[right],0) + 1
                if window[s[right]] == need[s[right]]: match += 1
            
            while len(need) == match:
                if minlen > right-left:
                    res = s[left:right+1]
                    minlen = right-left
                
                if s[left] in need:
                    window[s[left]] -= 1
                    if window[s[left]] < need[s[left]]:
                        match -= 1
                left += 1
        
        return res
```



**Complexity analysis**

- Time complexity: $O(N)$
  - In the worst case, the `left` and `right` pointers traverse each element of `s` once. 
  - In the hash table, each element in `s` is inserted and deleted once, and each element in `t` is inserted once. 
  - Each check to see if it is feasible will traverse the hash table of the entire `t`. 
  - The size of the hash table is related to the size of the character set. If the size of the character set is `C`, the asymptotic time complexity is `O(C⋅∣s∣+∣t∣).`.
- Space complexity: 
  - Two hash tables are used here as extra spaces. Each hash table will not store key-value pairs that exceed the character set size at most. We set the character set size to `C`, and the progressive space complexity is `O(C)`

### [713  Subarray Product Less  Than K](https://leetcode.com/problems/subarray-product-less-than-k/)

Given an array of integers `nums` and an integer `k`, return *the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than* `k`.

**Example 1:**

```
Input: nums = [10,5,2,6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are:
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```

**Example 2:**

```
Input: nums = [1,2,3], k = 0
Output: 0
```

#### Approach 1:  window+multiply

**intuition**

1. window is the interval `[left,right]`, product is the element-wise product of window

```c
int numSubarrayProductLessThanK(int* nums, int numsSize, int k){
    int left = 0, right = 0, cnt = 0, product = 1;
    while(right < numsSize){
        product *= nums[right];
        // subarray shrink until meet the < k requirement
        while(product >= k && left <= right){
            product /= nums[left++];
        } 
        // counting the subarrays starting with index left
        //Say now we have {1,2,3} and add {4} into it. Apparently, the new subarray introduced here are:
		//{1,2,3,4}, {2,3,4}, {3,4}, {4}, which is the number of elements in the new list.
		//If we also remove some at the left, say we we remove 1, then subarrays are:
		//{2,3,4}, {3,4}, {4}. It is easy to get the result is j - i + 1.
        cnt += right - left + 1;
        right++;
    }
    return cnt;
}
```

- Time complexity: $O(N)$
- Space complexity: $O(1)$ 

## String & SubString & Subsequence & Palindrome

### [8.  String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/solution/)

Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function).

The algorithm for `myAtoi(string s)` is as follows:

1. Read in and ignore any leading whitespace.
2. Check if the next character (if not already at the end of the string) is `'-'` or `'+'`. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.
3. Read in next the characters until the next non-digit character or the end of the input is reached. The rest of the string is ignored.
4. Convert these digits into an integer (i.e. `"123" -> 123`, `"0032" -> 32`). If no digits were read, then the integer is `0`. Change the sign as necessary (from step 2).
5. If the integer is out of the 32-bit signed integer range `[-231, 231 - 1]`, then clamp the integer so that it remains in the range. Specifically, integers less than `-231` should be clamped to `-231`, and integers greater than `231 - 1` should be clamped to `231 - 1`.
6. Return the integer as the final result.

**Example 1:**

```
Input: s = "42"
Output: 42
```

**Example 2:**

```
Input: s = "   -42"
Output: -42
```

**Example 3:**

```
Input: s = "4193 with words"
Output: 4193
```

#### Approach 1: Follow the Rules

**Intuition**

1. Given the rules outlined by the problem's description, we can iterate over the input string and use the given rules to validate it.
2. First read through the problem statement **very carefully**. Let's see what are all the possible characters in the input string:
   - Whitespaces **(' ')**
   - Digits **('0', '1', '2', '3', '4', '5', '6', '7', '8', '9')**
   - A sign **('+' or '-')**
   - Anything else (alphabetic characters, symbols, special characters, etc.)

3. we need to process these four cases just up above

```python
class Solution(object):
    def myAtoi(self, str):
        """
        :type str: str
        :rtype: int
        """
        str = str.strip()
        if not str: return 0
        res = ''
        flag = 1
        for i in range(len(str)):
            # check the valadation of first element (avoid space or letter)
            if  i == 0:
                # non-numeric case
                if str[i] not in {'+','-'} and not str[i].isdigit():
                    return 0
                # sign case
                elif str[i] == '+':
                    flag = 1
                # sign case 
                elif str[i] == '-':
                    flag = -1
                else:
                    res += str[i]
            elif not str[i].isdigit():
                break
            else:
                res += str[i]
        
        if not res:return 0
        # make sure the result doesn't cause overflow for integer
        return min(2**31-1, max(-2**31,int(res)*flag))
```

**Complexity Analysis**

If `N` is the number of characters in the input string.

- Time complexity: `O(N)`

  We visit each character in the input at most once and for each character we spend a constant amount of time.

- Space complexity: `O(1)`

  We have used only constant space to store the sign and the result.

#### [Approach 2: Deterministic Finite Automaton (DFA)](https://leetcode.com/problems/string-to-integer-atoi/solution/) or State machines

**Intuition**

1. we will present an approach that uses DFA which is a more generalized approach that can also be applied to similar problems that would otherwise require writing many nested if else conditions which could become very complex.

2. What's DFA? 

   1. Theory of Computing is the study of theoretical machines and problems which can be solved using these machines. These machines are called **state machines**. A state machine reads some input and changes the states based on those inputs.

   ![image-20220511190634163](./LeetCodeNote.assets/image-20220511190634163.png)

**Algorithm**

1. Initialize three variables:
   - `currentState` (to represent current state) as `q0`
   - `result` (to store result till now) as `0`
   - `sign` (to represent the sign of the final result) as `1`

2. For each character of the input string, if the current state is not `qd`:

- If the current state is `q0`:
  - Stay in the same state if whitespaces occur.
  - If a sign occurs, change the sign variable to `-1` if it's a negative sign and change the state to `q1`.
  - If a digit occurs, append the current digit to the resulting number (clamp result if needed) and change the state to `q2`.
  - If anything else occurs, then stop building the number and transition to state `qd`.

BalaBalabalalaxxdedXXXXX

 **fuck the state machine, just copy the code**

```python
INT_MAX = 2 ** 31 - 1
INT_MIN = -2 ** 31 

class  Automatton:
    def __init__(self):
        self.state = 'start'
        self.sign = 1
        self.ans = 0
        self.table = {
            'start':['start','signed','in_number','end'],
            'signed': ['end','end','in_number','end'],
            'in_number': ['end','end','in_number','end'],
            'end':['end','end','end','end'],
        }
    
    def get_col(self,c):
        if c.isspace():
            return 0
        if c == '+' or c == '-':
            return 1
        if c.isdigit():
            return 2
        return 3

    def get(self,c):
        self.state = self.table[self.state][self.get_col(c)]
        if self.state == 'in_number':
            self.ans = self.ans*10 + int(c)
            self.ans = min(self.ans,INT_MAX) if self.sign==1 else min(self.ans,-INT_MIN)
        elif self.state == 'signed':
            self.sign = 1 if c == '+' else -1

class Solution:
    def myAtoi(self, str: str) -> int:
        auto = Automatton()
        for c in str:
            auto.get(c)
        return auto.sign * auto.ans
```

**Complexity Analysis**

If `N` is the number of characters in the input string.

- Time complexity: `O(N)`

  We iterate over the input string exactly once, and each state transition only requires constant time.

- Space complexity: `O(1)`

  We have used only constant space to store the state, sign, and result.

### [5.  Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

Given a string `s`, return *the longest palindromic substring* in `s`.

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

#### Approach : Expand Around Center+Greedy 

**Intuition** 

1. We observe that a palindrome mirrors around its center.
2. There  two kind of centers for Palindromic Substring:
   1. **odd center** :example `aba` (is a Palindromic Substring), the center is `b`
   2. **even center** : like: `abba`， the center isn't a letter, it's a gap between two b,**we can chop it off into two same parts at this position of gap**

3. Go through the string,  and start to match the Palindromic substring  from the every odd or even center.
4. And same time Find the longest Palindromic substring with `greedy algorithm`

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # edge cases!
        if not s: return ''
        if len(s) < 2: return s
        res = ''
        
        for i in range(len(s)):
            # find the odd Center String Start from letter 
            odd = self.matchCenter(s,i,i)
            # find even center string start from gap
            even = self.matchCenter(s,i,i+1)
            
            # greedy!
            tmpString =  odd if len(odd) > len(even) else even
            if len(tmpString) > len(res):
                res = tmpString
        return res
    
    def matchCenter(self,s,left,right):
        if not s: return ''
        #Extend the palindrome string to the left Side and right side from the center
        while left >=0 and right <len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return s[left+1:right]
```

**Complexity Analysis**

- Time complexity : `O(n^2)` Since expanding a palindrome around its center could take `O(n)` time, the overall complexity is `O(n^2)`
- Space complexity : `O(1)`



## Sorting

### [148 Sort List](https://leetcode.com/problems/sort-list/)

Given the `head` of a linked list, return *the list after sorting it in **ascending order***.

- The number of nodes in the list is in the range `[0, 5 * 104]`.
- `-105 <= Node.val <= 105`

#### solution 1 :  Copy To array->Sort Array

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

Time Complexity: `O(NlogN)`
Space Complexity: `O(N)`

#### solution 2: merge Sort

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

Time Complexity: `O(NlogN)`
Space Complexity: O(N)



## Two Pointers

### [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` *after placing the final result in the first* `k` *slots of* `nums`.

Do **not** allocate extra space for another array. You must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

**Example 1:**

```
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2:**

```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```



#### [Approach 1:  two Pointers](https://www.youtube.com/watch?v=DEJAZBq0FDA)+ Fast-slow permutation

**Intuition**

1.  remove duplicates ? it's more like to find how many unique value in the array
2.  it`s easy to think of `HashSet`, but this problem only allow to using constantly extra space
3. using left & right pointer:  Because the array is in ascending order, the index of equal elements are consecutive. We use the fast pointer to traverse the array and ==copy the different elements from fast to slow,== so that slow can records different elements in the array.

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        int slow = 0, fast = 1;
        
        while (fast < nums.size()){
            // find the unique value 
            if(nums[fast] != nums[slow]){
                // slow increment  (counting !)
                slow++;
                //  copy the unique to slow (left part)
                nums[slow] = nums[fast];
            }
            // fast point need shift every time
            fast++;
        }
        // index start from 0 ,so need to + 1
        return slow + 1;
    }
};
```



### [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

Given a **1-indexed** array of integers `numbers` that is already ***sorted in non-decreasing order\***, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return *the indices of the two numbers,* `index1` *and* `index2`*, **added by one** as an integer array* `[index1, index2]` *of length 2.*

The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.

Your solution must use only constant extra space.

[Video Explanation Link](https://www.youtube.com/watch?v=sAQT4ZrUfWo)

#### Approach 1:  Two Pointer  && modified Binary Search :

**Intuition**

1. there has a limit of space cost, the problem ask for constant extra space, So we must give up using Hash Table 
2. It is a sorted array, and is a big clue(Intuition) to using Two Pointer or Binary Search;
3. using two pointer to close the target we want , it`s quit logic to using 

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
         int left =0 , right = numbers.size()-1;
            while (left <= right){
                int sum = numbers[left] + numbers[right];
                if (sum == target) return {left+1, right+1};
                else if (sum < target) left++;
                else right--;
            }

            return {};
    }
};
```

**Complexity analysis**

- Time complexity: $O(Log N)$

- Space complexity: $O(1)$​ extra space. Only constant level space required.

   

### [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return *the maximum amount of water a container can store*.

**Notice** that you may not slant the container.

**Example 1:**

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

#### Approach 1:  Two Pointer + Greedy :

**Intuition**

1. We have to **maximize** the Area that can be formed between the vertical lines using the shorter line as length and the distance between the lines as the width of the rectangle forming the area.
2. The intuition of behind this approach is **that the area formed between this lines will always be limited by the shorter line** 
3. the farther the lines, the more area obtained (get) 
4. SO~ it's so obvious for us to use two Pointers ,  
   1. one at the beginning and one at the end of the array constituting the length of the lines
   2. **maintain** a variable $\text{maxarea}$ to store the maximum area obtained till now
   3. **update** $\text{maxarea}$​ and move the pointer pointing to the shorter line towards the other end by one step
   4. `maxarea = max(maxarea, min(height[left], height[right]) * (right-left))`


```C++
class Solution {
public:
    int maxArea(vector<int>& height) {
        if (height.empty()) return 0;
        int maxArea = 0;
        int left = 0, right = height.size()-1;
        
    	while (left < right){
        maxArea = max(maxArea, min(height[left], height[right]) * (right-left));
        if (height[left] <height[right]) left++;
        else right--;
    }
    	return maxArea;
    }
};
```
**Complexity Analysis**

- Time complexity: `O(n)`. Single pass
- Space complexity: `O(1)`. Constant space is used.



### [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

#### Approach 2: two pointers+Greedy

**Intuition**

For each element in the array, we find the maximum level of water it can trap after the rain, **which is equal to the minimum of maximum height of bars on both the sides minus its own height.**

1. We assume that the rainwater that each bar can receive is `Rain(i)` , the final  `ans = Sum(Rain(i))`

2. `Rain(i) =  min (leftMaxBar,rightMaxBar) - height[i]` ,

   1.  `leftMaxBar = the highest bar for left part of height[i]`
   2.  `rightMaxBar= the highest bar for right part of height[i]`
   3.  the Rain area is **limited** by the shorter Bar

3. SO this problem will turn into How to find the  `leftMaxBar`  and `rightMaxBar`

4. Using Two Pointers And Greedy to update the `leftMaxBar`  !!!

   1. `left` Pointer is keeping track of the `leftMaxBar`, so does to `right` Pointer and `rightMaxBar`
   2.  using greedy ways to make sure `leftMaxBar` and  `rightMaxBar` are maximum
   
   

```Python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height: return 0
        
        leftMaxBar = height[0]
        rightMaxBar = height[-1]
        left,right = 0, len(height)-1
        
        ans = 0
        
        while left < right:
            leftMaxBar = max(leftMaxBar,height[left])
            rightMaxBar = max(rightMaxBar, height[right])
            
            # the rain area was determined by the shorted one 
            if leftMaxBar <  rightMaxBar:
                ans += leftMaxBar - height[left]
                left += 1
            else:
                ans += rightMaxBar - height[right]
                right -= 1
        
        return ans
```

**Complexity analysis**

- Time complexity: `O(n).` Single iteration of `*O*(*n*)`.
- Space complexity: `O(1)` ，extra space. Only constant space required for some variables

#### Approach 1: Monotonic  Stack

**From Approach 1, we could Know the Problem is to how to computer  Rain(i)**

**Intuition**

1. we can use stack to keep track of the bars that are bounded by longer bars 
2. We keep a stack and iterate over the array， We add the index of the bar  value to the stack if current value is smaller than or equal to the bar value at top of stack,which means that the current bar is bounded by the previous bar in the stack
3. If we found a bar longer than that at the top, we are sure that the bar at the top of the stack is bounded by the current bar

**Algorithm**

- Use stack to store the indices of the bars.

- Iterate the array:

  - While stack is not empty and

    $\text{height[current]}>\text{height[st.top()]}$

    - It means that the stack element can be popped. Pop the top element as `top`.
    - find the Trapping Area`s  current Width and  current Height
    - Add resulting trapped water to `answer += currWidth * currHeight`

  - Push current index to top of the stack

  - Move `current` to the next position

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        stack<int> stk;
        int n = height.size();
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && height[i] > height[stk.top()]) {
                int top = stk.top();
                stk.pop();
                if (stk.empty()) {
                    break;
                }
                int left = stk.top();
                // -1 means can't count the bar's width
                int currWidth = i - left - 1;
                int currHeight = min(height[left], height[i]) - height[top];
                ans += currWidth * currHeight;
            }
            stk.push(i);
        }
        return ans;
    }
};
```

**Complexity analysis**

- Time complexity: `O(n)`
  - Single iteration of `O(n)` in which each bar can be touched at most twice(due to insertion and deletion from stack) and insertion and deletion from stack takes `O(1)` time.
- Space complexity: `O(n)`. Stack can take up to `O(n)` Space, Caused the input `height` array's length



### [15. 3Sum](https://leetcode-cn.com/problems/3sum/)

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

#### [Approach 1: Sorting+Two Pointers+De-duplication](https://www.youtube.com/watch?v=qJSPYnS35SE)

**intuition**

1. it's easy to think of using Brunt Force method to enumerate every possibe combination of Any three elemens from Giving array, But that's will consume huge of time, it's bad a solution 
2. how we can optimaize the Brunt Force enumeration  ? Using **Sorting + two Pointers !!!**
3. After Sorted array, then we can using `cur ， Left ， right`, these three pointer to enumerate the answers and with De-duplication



```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int lens = nums.size();
        vector<vector<int>> res;
        
        if (lens < 3) return res;
        
       sort(nums.begin(),nums.end());
       
        for(int cur = 0; cur < lens - 2; ++cur){
            // cause for the sorted array, there's no exist 3Sum==0, So just reture 
            if( nums[cur] > 0) return res;
            // for the dulicate elements,Skip it 
            if(cur > 0 && nums[cur]  == nums[cur-1]) 
                continue;
            
            int left = cur + 1;
            int right = lens - 1;
            // classic two pointers writing style 
            while (left < right){
                // find one case we want  
                if (nums[cur] + nums[left] + nums[right] == 0){
                    res.push_back({nums[cur],nums[left], nums[right]});
                    // check the nebior to avoid dupulation results 
                    while(left < right && nums[left] == nums[left+1])
                        left++;
                    while(left < right && nums[right] == nums[right-1])
                        right--;
                    // visited index need to shift!
                    left++;
                    right--;
                    // if 3Sum > 0, menas nums[right] is too biger, so right decreament
                }else if(nums[cur]+nums[left]+nums[right] > 0){
                    right--;
                }else{
                    left++;
                }
            }
        }
        
        return res;
    }
};
```

**Complexity analysis**

- Time complexity: `O(n^2)`
  - `Sorting's` running time is `O(NlogN)`, scan whole array takes `O(N)` time, two pointers tranvers  the array required `O(N)` time, 
    - SO final required time is : `O(NlogN)+ O(N)*O(N) = O(N^2)`
- Space complexity: `O(1)`.  constant extra memory required



## 

# Dynamic Programming

Cause Dynamic Programming is a very hard part for the algorithm interview and there has lots of  categories in DP problem. SO we need a Special Chapter for this big kind of problem. 

## Classic DP

### [139 Word Break](https://leetcode-cn.com/problems/minimum-window-substring/)

Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

 **Example 1:**

```
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".\
```

#### Approach:  DP

**Intuition**

1. Create array that keeps track of the string s so far, if the substring is possible to be built by the wordDict.

2. We define $\textit{dp}$​[i] to indicate whether the string  `s[0..i−1]` composed of the first `i` characters of the string `s` can be split by spaces into several dictionary word

3. using pointer `j` to split `s[0..i−1]`  into `s1 = s[0..j-1]` and `s2 =  s[j..i-1]` , If both strings are valid, then `s1+s2` also valid
   $$
   dp[i]= dp[j] \&\& check(s[j..i−1])
   $$
   `check(s[j..i-1])`  indicates whether the substring `s[j..i-1]` appears in the dictionary.

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        n = len(s)
        dp = [False]*(n+1)
        dp[0] = True 
		
        
        for i in range(n):
            # Loop through all indices of s after i
            for j in range(i+1,n+1):
                # If s[:i] is already buildable, and s[i:j] is in wordDict, then s[j] is buildable
                if dp[i] == True and s[i:j] in wordDict:
                    dp[j] = True 
        # Check if the entire s is buildable
        return dp[-1]
```

**Complexity analysis**

1. Time complexity: `O(n^2)`
   1. where `n` is the length of the string `s`. We have a total of `O(n)` states to calculate, and each calculation needs to enumerate `O(n)` split points. The hash table determines whether a string appears in a given string list. It takes `O(1)` time, so the total time complexity is `O(n^2)`

2. Space complexity: `O(n)`, where n is the length of the string s. 

## DP + Matching 

### [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/solution/)

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

- `'.'` Matches any single character.
- `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

**Example 1:**

```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```



#### [Approach 1: Recursion](https://www.youtube.com/watch?v=HAA8mgxlov8)

**Intuition**

1. `[ .* ]` can match any string of `S`

2. This problem can be divided into two cases:

   1. P has  `*` star:  there are two cases we need to process, **because `*` can match zero or more than once of  preceding element **

      1. `*` match  zero element (make the precding character repeated 0 times )
      2. `*` match more element (make the preceding character repeated multi-times, even infinite number of times)

   2. `P` hasn't  `*` star:  For the no start case, the match will be simpler without Stars: For example, if we meet  `.`in `p`, we just need to skip it 

3. So we can using Recursion to divide and  conquer this problem

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        
        # handle edge case
        if not p: return not s
        
        # Case 1: No * --> p[0] == s[0] or ".", it means first Match successed 
        firstMatch = bool(s and p[0] in {s[0],'.'})
        
        # Case 2:with * 
        if len(p) > 1 and p[1] == "*":
            # 1-> * makes the preceding element disappear(No matter the fistMatch successed)
            # 2-> * matches more than once of preceding element (precondition: firstMatch successed)
            return self.isMatch(s,p[2:]) or firstMatch and self.isMatch(s[1:],p)
        else:
            # no *, just match rest of String
            return firstMatch and self.isMatch(s[1:],p[1:])
        return True
        
```

[**Complexity Analysis**:](https://leetcode.com/problems/regular-expression-matching/solution/)

- Time Complexity:  Recursion will consumes lots of time, the upper Bound of  time complexity is `O((S+P)* 2^(S+P/2))`
- Space Complexity:  Recursion also requires stack memory to execute, it will also take a total of `O((S+P)* 2^(S+P/2))`space

#### Approach 2: Recursion+Memo

**Intuition**

1. we could optimize the recursion approach by introducing Memo ,How ?

2. Using memo to memory the `SubString` of `S` and `T` we already visited, then we could reduce the running time (complexity) 

   

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        
        # introduce memo
        memo = dict()
        
        # handle edge case
        if not p: return not s
        
        if (s,p) in memo:
            # using HashTable tp avoid the repeated works
            return memo[(s,p)]
        
        # Case 1: No * --> p[0] == s[0] or ".", it means first Match successed 
        firstMatch = bool(s and p[0] in {s[0],'.'})
        
        # Case 2:with * 
        if len(p) > 1 and p[1] == "*":
            # 1-> * makes the preceding element disappear
            # 2-> * matches more than once of preceding element (precondition: firstMatch successed)
            return self.isMatch(s,p[2:]) or firstMatch and self.isMatch(s[1:],p)
        else:
            # no *, just match rest of String
            return firstMatch and self.isMatch(s[1:],p[1:])
        # put the ans into Memo
        memo[(s,p)] = ans
        return ans
        
```

[**Complexity Analysis**:](https://leetcode.com/problems/regular-expression-matching/solution/)

- Time Complexity:  memo will  reduce the time consuming, the upper Bound of  time complexity is `O(SP)`
- Space Complexity:  Recursion also requires stack memory to execute, it will also take a total of `O((S+P)* 2^(T+P/2))`space, and added the memo required Space

#### Approach 3: DP

**Intuition**

1. Recursion with memo approach is not good enough in the space or time consuming  
2. In Fact, most of Recursion problem can convert to Dynamic Programming , How ? 

**Define DP table**

1. every DP need to define the  meaning of DP table

2. there are two Strings, so we need a **two-dimensional** array:
   
   `dp[i][j] = true` means  the `subString` of `S[0 to i-1]` and `P[0 to j-1]`is matched !

3. and the details of code is **similar** to Recursion approach

4. There are two direction to fill the `Dp` table :, *Top-Down Variation* 

   1.  *Bottom-Up Variation* : the final answer is `dp[0][0]`
   2.  *Top-Down Variation* : the final answer is `dp[end][end]`

5. Using *Top-Down Variation* to implement the DP table

   ```text
           After filled all grid of Dp table From Top to Bottom
           # dp(0,0) dp(0,1) ... dp(0,j)..  dp(0,len(p)+1)
           # dp(1,0)
           # ...
           # dp(len(s)+1,0) ... ...         dp(-1,-1)
   ```

   

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        
        # dp[i][j] == s[0 to i-1] match p[0 to j-1]
        
        # 1. define DP table : 2-dimention 
        dp = [[False for _ in range(len(p)+1)] for _ in range(len(s)+1)]
        
        # 2. several intionlized steps
        
        # 2.1, fill the first grid 
        dp[0][0] = True
        
        # 2.2 fill the fist row grid If could 
        for j in range(1,len(p)+1):
            
            if p[j-1] == "*":
                # if s == '',that means p mush to using * to disapper the preceding element
                dp[0][j] = dp[0][j-2]
            else:
                dp[0][j] = False
        
        for i in range(1,len(s)+1):
            for j in range(1,len(p)+1):
                # case 1: no *
                if s[i-1] == p[j-1] or p[j-1] == ".":
                    dp[i][j] = dp[i-1][j-1]
                
                # case 2: with *
                if p[j-1] == '*':
                    # preceding element matched,then j backward 2 steps
                    # '.' exist, then i backward 1 step
                    if p[j-2] == s[i-1] or p[j-2] == '.':
                        dp[i][j] = dp[i][j-2] or dp[i-1][j]
                    else:
                        # makes the preceding element disappear so j need to backward 2 steps
                        dp[i][j] = dp[i][j-2]
                
        # 3. After filled all grid of Dp table From Top to Bottom
        # dp(0,0) dp(0,1) ... dp(0,j)..  dp(0,len(p)+1)
        # dp(1,0)
        # ...
        # dp(len(s)+1,0) ... ...         dp(-1,-1)
        
        return dp[-1][-1]

```

[**Complexity Analysis**:](https://leetcode.com/problems/regular-expression-matching/solution/)

- Time Complexity:  `O(SP), S= len(s), P = len(p)`
- Space Complexity:  `O(SP), S= len(s), P = len(p)`

## DP + Puzzle



