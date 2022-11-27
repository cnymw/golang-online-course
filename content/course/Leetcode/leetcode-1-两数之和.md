---

title: 1. 两数之和
date: '2022-11-26'
type: book
weight: 1
commentable: true
editable: true
tags:
  - loop
  - hashmap
---

## 题目

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

```text
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

## 题解

### 解法 1：暴力枚举

看到该题目第一反应是将所有的数字遍历 O(n*n) 遍，暴力得到所有的可能的结果以及下标。

当我们遍历的时候，每一个在索引 index 之前的元素已经和索引 index 匹配过了，所以第二层循环只需要遍历索引 index 之后的元素即可。

```go
// 解法 1：暴力枚举
func twoSum(nums []int, target int) []int {
	n := len(nums)
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			if nums[i]+nums[j] == target {
				return []int{i, j}
			}
		}
	}
	return []int{0}
}
```

### 解法 2：哈希表

通过分析可以知道，结果 target = 9，第一个输入为 2，我们只需要知道 7（9-2=7） 在不在数组中即可。

同理，target=9,第三个输入 11，我们只需要知道 -2（9-11=-2）在不在数组中即可。

于是，我们可以将所有数组元素放入到哈希表中，然后遍历数组，通过判断 map[target-nums[i]] == null 条件即可得出结果。

在这里需要注意，当 target = 2 * num[i] 时，会出现 [i,i] 这样子的结果，例如 target=6,nums[0]=3, 会返回结果 [0,0]，所以需要排除这种情况。

```go
func twoSum(nums []int, target int) []int {
	// hashTable 通过 map 结构记录结果
	hashTable := map[int]int{}
	
	for i, x := range nums {
		// 从 hashTable 里直接取对应的结果。
		// 例如当前为 x=2，target 为 9，那么取 hashTable[7]
		// 如果取的到话，直接返回结果
		if p, ok := hashTable[target-x]; ok {
			return []int{p, i}
		}
		// 取不到的话，存入hashTable
		// 例如当前 x=2，存入 hashTable[2]
		hashTable[x] = i
	}
	return []int{0, 0}
}
```