> 给定两个数组，编写一个函数来计算它们的交集。

示例 1:

输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2,2]
示例 2:

输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [4,9]
说明：

输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。
我们可以不考虑输出结果的顺序。
进阶:

如果给定的数组已经排好序呢？你将如何优化你的算法？
如果 nums1 的大小比 nums2 小很多，哪种方法更优？
如果 nums2 的元素存储在磁盘上，磁盘内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/intersection-of-two-arrays-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

#### 最笨的解法
```
var intersect = function() {
    if (nums1.length > nums2.length) {
        [nums1, nums2] = [nums2, nums1]
    }
    
    let arr = []
   
    for (let i = 0; i < nums1.length; ) {
        let index = nums2.indexOf(nums1[i])
        if (index !== -1) {
            arr.push([nums1[i]])
            nums2.splice(index, 1)
            nums1.splice(i, 1)
        } else {
            i++
        }
    }

    return arr
}
```
#### 双指针
```
var intersect = function(nums1, nums2) {
    nums1 = nums1.sort((a, b) => a - b)
    nums2 = nums2.sort((a, b) => a - b)
    if (nums1.length > nums2.length) {
        [nums1, nums2] = [nums2, nums1]
    }

    let [i, j] = [0, 0]
    let res = []
    while( i < nums1.length && j < nums2.length) {
        if (nums1[i] < nums2[j]) {
            i++
        } else if (nums1[i] > nums2[j]) {
            j++
        } else {
            res.push(nums1[i])
            i++;
            j++;
        }
    }
    return res
}
```
这里多定义了一个res数组，其实可以优化下，直接利用已经存在的数组来存放结果，不过jsperf上看没太大区别：
```
var intersect = function(nums1, nums2) {
    nums1 = nums1.sort((a, b) => a - b)
    nums2 = nums2.sort((a, b) => a - b)
    if (nums1.length > nums2.length) {
        [nums1, nums2] = [nums2, nums1]
    }

    let [i, j, k] = [0, 0, 0]
 
    while( i < nums1.length && j < nums2.length) {
        if (nums1[i] < nums2[j]) {
            i++
        } else if (nums1[i] > nums2[j]) {
            j++
        } else {
           nums1[k++] = nums1[i++]
           j++;
        }
    }
    return nums1.slice(0, k)
}
```

#### 哈希表
这种方式不需要排序，然后时间复杂度也不会很高O(m+n)，但是LeetCode上跑的不理想。。。
```
var intersect = function(nums1, nums2) {
    let obj = {}
    let res = []
    for (const item of nums2) {
        obj[item] = obj[item] ? ++obj[item] : 1
    }
    for (const item of nums1) {
        if (obj[item] > 0) {
            obj[item]--
            res.push(item)
        }
    }
    return res
}
```
