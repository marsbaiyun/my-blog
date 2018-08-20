---
title: 两数之和(Two Sum)
date: 2018-08-20 14:18:37
tags: [技术,算法]
category: 学习
toc: true
mathjax: true
---

#### 题目一

给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

#### 示例:
    
  给定 nums = [2, 7, 11, 15], target = 9
  
  因为 nums[0] + nums[1] = 2 + 7 = 9
  所以返回 [0, 1]
    

#### 我的解法：

    
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
    
[官方解决方法](https://leetcode-cn.com/articles/two-sum/)

***

#### 题目二 - 输入有序数组

给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

##### 说明:

* 返回的下标值（index1 和 index2）不是从零开始的。
* 你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

#### 示例:
    
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
    

提交了之后，看到了执行结果耗时最短的解法，果然厉害啊：
    
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