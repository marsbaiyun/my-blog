---
date: 2018-08-21 16:39:57
title: leet code 刷题
tags: [技术,算法,简单]
category: 学习
toc: true
mathjax: true
---

### 1.两数之和(Two Sum)

给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

**示例**
    
    给定 nums = [2, 7, 11, 15], target = 9
    
    因为 nums[0] + nums[1] = 2 + 7 = 9
    所以返回 [0, 1]
    

**我的解法**

    
    class Solution {
        public int[] twoSum(int[] nums, int target) {
            int[] result = null;
            for(int i = 0;i < nums.length;i ++){
                if(result != null && result.length == 2){
                    break;
                }
                for(int j = 0;j < nums.length;j ++) {
                    if(j == i){
                        continue;
                    }
                    if((nums[i] + nums[j]) == target){
                        result = new int[]{i, j};
                        break;
                    }
                }
            }
            return result;
        }
    }
    
**优解**
    
遍历一次，在进行迭代并将元素插入到表中的同时，我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。如果它存在，那我们已经找到了对应解，并立即将其返回。

    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }

**复杂度分析**

时间复杂度：$O(n)$， 我们只遍历了包含有n个元素的列表一次。在表中进行的每次查找只花费$O(1)$的时间。

空间复杂度：$O(n)$， 所需的额外空间取决于哈希表中存储的元素数量，该表最多需要存储n个元素。


***
### 2. 两数相加(Add Two Numbers)

给定两个非空链表来表示两个非负整数。位数按照逆序方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

**示例**

    输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
    输出：7 -> 0 -> 8
    原因：342 + 465 = 807

**我的解法**
    
    /**
     * Definition for singly-linked list.
     * public class ListNode {
     *     int val;
     *     ListNode next;
     *     ListNode(int x) { val = x; }
     * }
     */
    class Solution {
        public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
            ListNode resultNode = null;
            ListNode tmpNode = null;
            int sum = 0;
            int tmp;
            do {
                if(l1 != null){
                    sum += l1.val;
                    l1 = l1.next;
                }
                if(l2 != null){
                    sum += l2.val;
                    l2 = l2.next;
                }
                ListNode nextNode = new ListNode(sum%10);
                tmp = sum/10;
                sum = tmp;
                if(tmpNode == null){
                    resultNode = nextNode;
                }else {
                    tmpNode.next = nextNode;
                }
                tmpNode = nextNode;
            }
            while (l1 != null || l2 != null);
            if(tmp != 0){
                tmpNode.next = new ListNode(tmp);
            }
            return resultNode;
        }
    }
    
**优解**
    
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode p = l1, q = l2, curr = dummyHead;
        int carry = 0;
        while (p != null || q != null) {
            int x = (p != null) ? p.val : 0;
            int y = (q != null) ? q.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            if (p != null) p = p.next;
            if (q != null) q = q.next;
        }
        if (carry > 0) {
            curr.next = new ListNode(carry);
        }
        return dummyHead.next;
    }

**复杂度分析**

时间复杂度：$O(\max(m, n))$，假设 m 和 n 分别表示 l1 和 l2 的长度，上面的算法最多重复 $\max(m, n)$ 次。

空间复杂度：$O(\max(m, n))$， 新列表的长度最多为 $\max(m,n)$。

***

### 3. 无重复字符的最长子串(Longest Substring Without Repeating Characters)

给定一个字符串，找出不含有重复字符的最长子串的长度。

**示例 1**

    输入: "abcabcbb"
    输出: 3 
    解释: 无重复字符的最长子串是 "abc"，其长度为 3。
    
**示例 2**

    输入: "bbbbb"
    输出: 1
    解释: 无重复字符的最长子串是 "b"，其长度为 1。
    
**示例 3**

    输入: "pwwkew"
    输出: 3
    解释: 无重复字符的最长子串是 "wke"，其长度为 3。
         请注意，答案必须是一个子串，"pwke" 是一个子序列 而不是子串。
         
> 这道题想了半个小时除了暴力遍历法没有想到别的思路遂放弃

**推荐解决方案**
**滑动窗口**
**算法**

在暴力法中，我们会反复检查一个子字符串是否含有有重复的字符，但这是没有必要的。如果从索引$i$到$j - 1$之间的子字符串$s_{ij}$已经被检查为没有重复字符。
我们只需要检查$s[j]$对应的字符是否已经存在于子字符串$s_{ij}$中。

要检查一个字符是否已经在子字符串中，我们可以检查整个子字符串，这将产生一个复杂度为$O(n^2)$的算法，但我们可以做得更好。

通过使用 ``HashSet`` 作为滑动窗口，我们可以用$O(1)$的时间来完成对字符是否在当前的子字符串中的检查。

滑动窗口是数组/字符串问题中常用的抽象概念。 窗口通常是在数组/字符串中由开始和结束索引定义的一系列元素的集合，即 $[i, j)$（左闭，右开）。而滑动窗口是可以将两个边界向某一方向“滑动”的窗口。例如，我们将 $[i, j)$ 向右滑动 11 个元素，则它将变为 $[i+1, j+1)$（左闭，右开）。

回到我们的问题，我们使用 ``HashSet`` 将字符存储在当前窗口 $[i, j)$（最初 j = ij=i）中。 然后我们向右侧滑动索引 j，如果它不在 ``HashSet`` 中，我们会继续滑动 j。直到 $s[j]$ 已经存在于 ``HashSet`` 中。此时，我们找到的没有重复字符的最长子字符串将会以索引 i 开头。如果我们对所有的 i 这样做，就可以得到答案。

    public class Solution {
        public int lengthOfLongestSubstring(String s) {
            int n = s.length();
            Set<Character> set = new HashSet<>();
            int ans = 0, i = 0, j = 0;
            while (i < n && j < n) {
                // try to extend the range [i, j]
                if (!set.contains(s.charAt(j))){
                    set.add(s.charAt(j++));
                    ans = Math.max(ans, j - i);
                }
                else {
                    set.remove(s.charAt(i++));
                }
            }
            return ans;
        }
    }

**复杂度分析**

* 时间复杂度：$O(2n) = O(n)$，在最糟糕的情况下，每个字符将被 i 和 j 访问两次。

* 空间复杂度：$O(min(m, n))$，与之前的方法相同。滑动窗口法需要 $O(k)$ 的空间，其中 k 表示 ``Set`` 的大小。而Set的大小取决于字符串 n 的大小以及字符集/字母 m 的大小。 

**优化的滑动窗口**
**算法**

上述的方法最多需要执行 2n 个步骤。事实上，它可以被进一步优化为仅需要 n 个步骤。我们可以定义字符到索引的映射，而不是使用集合来判断一个字符是否存在。 当我们找到重复的字符时，我们可以立即跳过该窗口。

也就是说，如果 $s[j]$ 在 $[i, j)$ 范围内有与 $j'$重复的字符，我们不需要逐渐增加 i 。 我们可以直接跳过 $[𝑖，𝑗′]$ 范围内的所有元素，并将 i 变为 $j' + 1$。

    public class Solution {
        public int lengthOfLongestSubstring(String s) {
            int n = s.length(), ans = 0;
            Map<Character, Integer> map = new HashMap<>(); // current index of character
            // try to extend the range [i, j]
            for (int j = 0, i = 0; j < n; j++) {
                if (map.containsKey(s.charAt(j))) {
                    i = Math.max(map.get(s.charAt(j)), i);
                }
                ans = Math.max(ans, j - i + 1);
                map.put(s.charAt(j), j + 1);
            }
            return ans;
        }
    }

**复杂度分析**

* 时间复杂度：$O(n)$，索引 j 将会迭代 n 次。

* 空间复杂度（``HashMap``）：$O(min(m, n))$，与之前的方法相同。

***

### 题目二 - 输入有序数组

给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

***说明:***

* 返回的下标值（index1 和 index2）不是从零开始的。
* 你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

***示例:***
    
    输入: numbers = [2, 7, 11, 15], target = 9
    输出: [1,2]
    解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
    

#### 我的解法：

    
    class Solution {
        public int[] twoSum(int[] numbers, int target) {
            Map<Integer, Integer> map = new HashMap<>();
            for(int i = 0;i < numbers.length;i ++){
                int tmp = target - numbers[i];
                if(map.containsKey(tmp) && map.get(tmp) < i){
                    return new int[]{map.get(tmp)+1, i+1};
                }
                map.put(numbers[i], i);
            }
            return null;
        }
    }
    

#### 耗时最短的解法：
    
    class Solution {
        public int[] twoSum(int[] numbers, int target) {
            int[] result=new int[2];
            int i,j;
            for(i=0,j=numbers.length-1;i<j;)
            {
                if(numbers[i]+numbers[j]>target){
                    j--;
                }else if(numbers[i]+numbers[j]<target){
                    i++;
                }else break;
            }
            result[0]=i+1;
            result[1]=j+1;
            return result;
        }
    }
    
***

### 7.反转整数(Reverse Integer)

给定一个 32 位有符号整数，将整数中的数字进行反转。

**示例 1:**
    
    输入: 123
    输出: 321
    
**示例 2:**
    
    输入: -123
    输出: -321
    
**示例 3:**
    
    输入: 120
    输出: 21
    
***注意:***

假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−231,  231 − 1]。根据这个假设，如果反转后的整数溢出，则返回 0。

#### 我的解法：
    
    class Solution {
        public int reverse(int x) {
            StringBuilder sb = new StringBuilder("");
            String str = x+"";
            if(x < 0){
                sb.append("-");
                str = str.substring(1);
            }
            for(int i = str.length()-1;i >=0;i--){
                if(i == str.length()-1 && i !=0 && String.valueOf(str.charAt(i)).equals("0")){
                    continue;
                }
                sb.append(str.charAt(i));
            }
            Integer i = null;
            try {
                i = Integer.valueOf(sb.toString());
            } catch (NumberFormatException e) {
                return 0;
            }
    
            return i.intValue();
        }
    }
    
#### 耗时最短的解法：
    
    class Solution {
        public int reverse(int x) {
            int next = x;
            /*
             pop:反转数——余数
              */
            int pop = 0;
            int result = 0;
            do {
                pop = next % 10;
                next /= 10;
                // 判断是否溢出
    
                // MIN: -2147483648
                if (result < Integer.MIN_VALUE / 10 || result * 10 == Integer.MAX_VALUE && pop < -8) {
                    return 0;
                }
                // MAX:  2147483647
                if (result > Integer.MAX_VALUE /10 || result * 10 == Integer.MAX_VALUE && pop > 7) {
                    return 0;
                }
                result = result * 10 + pop;
            }
            while (next != 0);
            return result;
        }
    }
    
***

### 9.回文数(Palindrome Number)

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

***示例 1:***

    输入: 121
    输出: true

***示例 2:***

    输入: -121
    输出: false
    解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

***示例 3:***

    输入: 10
    输出: false
    解释: 从右向左读, 为 01 。因此它不是一个回文数。

***示例:***
    
  给定 nums = [2, 7, 11, 15], target = 9
  
  因为 nums[0] + nums[1] = 2 + 7 = 9
  所以返回 [0, 1]
    
***进阶:***

你能不将整数转为字符串来解决这个问题吗？

#### 我的解法：

    
    class Solution {
        public boolean isPalindrome(int x) {
            if(x < 0 || String.valueOf(x).equals("-0")){
                return false;
            }
            String str = String.valueOf(x);
            int i,j;
            for(i = 0,j = str.length()-1;i <= j;){
                if(str.charAt(i) == str.charAt(j)){
                    i++;
                    j--;
                }else {
                    return false;
                }
            }
            return true;
        }
    }
    
[官方解决方法](https://leetcode-cn.com/articles/palindrome-number/)

***