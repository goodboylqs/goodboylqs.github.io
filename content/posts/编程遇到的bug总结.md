+++
title = "编程遇到的bug总结"
author = ["553912917@qq.com"]
description = "好记性不如烂笔头"
date = 2024-01-02T08:00:00+08:00
lastmod = 2024-01-02T16:46:35+08:00
tags = ["编程", "c语言"]
categories = ["编程"]
draft = false
+++

## 被调用函数A中动态分配的内存，不能在调用该函数A的B函数中free，而是应该在A中free {#被调用函数a中动态分配的内存-不能在调用该函数a的b函数中free-而是应该在a中free}

-   <span class="timestamp-wrapper"><span class="timestamp">&lt;2024-01-02 二&gt;</span></span>

<!--listend-->

```C
  // https://leetcode.cn/problems/two-sum/
/*
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值
target  的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
你可以按任意顺序返回答案。
*/
#include "stdio.h"
#include "stdlib.h"
int *twoSum(int *nums, int numSize, int target, int *returnSize) {
  returnSize = (int*)malloc(sizeof(int) * 2);   //本意是想在被调用函数twoSum中动态分配内存来存储要返回的内容，然后在调用twoSum的main函数中释放动态分配的returnSize内存，但是这样是不对的。正确的方法是，在main函数中进行动态内存的分配和释放，即把这句放到main函数中
  for (int i = 0; i < numSize - 1; i++) {
    for (int j = i + 1; j < numSize; j++) {
      if (nums[i] + nums[j] == target) {
        returnSize[0] = nums[i];
        returnSize[1] = nums[j];
        return returnSize;
      } else {
        ;
      }
    }
  }
}
int *main() {
  int numSize = 4;
  int nums[4] = {2, 7, 11, 15};
  int target = 9;
  int *returnSize;   //应将这句替换为上面的int *returnSize = (int*)malloc(sizeof(int) * 2)
  twoSum(nums, numSize, target, returnSize);
  printf("nums:");
  for (int i = 0; i < numSize; i++) {
    printf("%d",nums[i]);
    if (i != numSize - 1) {
      printf(",");
    } else {
      printf("\n");
    }
  }
  printf("target:%d\n",target);
  printf("returnSize:%d,%d",returnSize[0],returnSize[1]);
  free(returnSize);
  return 0;
}
```
