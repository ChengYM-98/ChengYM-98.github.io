---
layout: posts
title: LeetCode-剑指Offer
date: 2022-11-20 14:01:02
tags: 算法
categories: 前端进阶
cover: /img/05.jpg
---
# 数组
## 03：数组中重复的数字-E
```js
// 题目：请找出数组中任意一个重复的数字
var findRepeatNumber = function (nums) {
    let i = 0;
    let repeatObj = {};
    for (i; i < nums.length; i += 1) {
        if (repeatObj[nums[i]]) {
            return nums[i];
        } else {
            repeatObj[nums[i]] = true;
        }
    }
};
```


## 04：二维数组中的查找-M
```js
// 题目：二维数组每行从左至右非递减，每列从上到下非递减
// 思想：从右上往左下遍历，分割维度的增减性

var findNumberIn2DArray = function (matrix, target) {
    if (!matrix.length) return false;
    let x = matrix[0].length - 1;
    let y = 0;

    while (x >= 0 && y < matrix.length) {
        if (matrix[y][x] === target) {
            return true;
        } else if (matrix[y][x] < target) {
            y++;
        } else {
            x--;
        }
    }
    return false
};
```


## 11：旋转数组的最小数字-E
```js
// 思想：二分法找最小元素

var minArray = function (numbers) {
    let left = 0;
    let right = numbers.length - 1;
    while (left < right) {
        let mid = Math.floor((right + left) / 2);
        if (numbers[mid] < numbers[right]) {
            right = mid;
        } else if (numbers[mid] > numbers[right]) {
            left = mid + 1;
        } else {
            right -= 1;
        }
    };
    return numbers[left];
};
```


## 14-1：剪绳子-M
```js
// 题目：一段绳子可切成M段，求M段长度乘积的最大值
// 思想：动态规划，现在的最大值等于上一段最大值的乘积乘以剩下的段长：nowBigger,而dpp[i]存储不同切法的最大值结果

var cuttingRope = function (n) {
    let i, j, dp = new Array(n + 1).fill(0), nowBigger;
    dp[2] = 1;
    for (i = 2; i <= n; i++) {
        for (j = 1; j < i; j++) {
            nowBigger = Math.max(j * (i - j), j * (dp[i - j]));
            console.log(nowBigger, dp[i], i)
            dp[i] = Math.max(dp[i], nowBigger);
        }
    }
    return dp[n]
};
```


## 14-2：剪绳子2-M
```js
// 题目：一段绳子可切成M段，求M段长度乘积的最大值，最后取模1e9+7
// 思想：就是上一题加了大数越界问题，经过数学归纳，乘以三的时候乘积较大，故程序尽量乘三

var cuttingRope = function (n) {
    if (n !== 1 && n < 4) {
        return n - 1;
    }
    let max = 1;
    while (n > 4) {
        n = n - 3;
        max = max * 3 % (1e9 + 7);
    }
    return max * n % (1e9 + 7);
};
```


## 17：打印从1到最大的n位数-E
```js
var printNumbers = function(n) {
    let num = 10**n-1;
    let result = [];
    for(let i=1;i<=num;i++){
        result.push(i)
    }
    return result;
};
```


## 21：调整数组顺序使奇书位于偶数前面-E
```js
var exchange = function(nums) {
    let l = 0;
    let r = nums.length - 1;
    while(l < r){
        if(nums[l] % 2 === 0 && nums[r] % 2 === 1){
            [nums[l], nums[r]] = [nums[r], nums[l]];
        }
        if(nums[l] % 2 === 1) l++;
        if(nums[r] % 2 === 0) r--;
    }
    return nums;
};
```


## 39：数组中出现次数超过一半的数字-E
```js
var majorityElement = function (nums) {
    if(nums.length === 1){
        return nums[0]
    }
    let mid = Math.floor(nums.length / 2);
    let obj = {};
    for (let i = 0; i < nums.length; i++) {
        let item = nums[i];
        if (obj[item]) {
            obj[item]++;
            if (obj[item] > mid) {
                return item
            }
        } else {
            obj[item] = 1;
        }
    }
};

```


## 40：最小的k个数-E
```js
var getLeastNumbers = function(arr, k) {
    arr.sort((a,b)=>a-b);
    return arr.slice(0,k)
};
```


## 41：数据流中的中位数-H
```js
/**
 * initialize your data structure here.
 */
var MedianFinder = function() {
    this.array = [];
};

/** 
 * @param {number} num
 * @return {void}
 */
MedianFinder.prototype.addNum = function(num) {
    this.array.push(num);
    this.array.sort((a,b)=>a-b);
};

/**
 * @return {number}
 */
MedianFinder.prototype.findMedian = function() {
    const len = this.array.length;
    if(len % 2 === 0){
        return (this.array[len/2]+this.array[len/2-1])/2;
    }else{
        return this.array[Math.floor(len/2)];
    }
};
```


## 43：1~n中1出现的次数-H
```js
var countDigitOne = function(n) {
    let strN = String(n);
    let len = strN.length;
    let result = 0;
    for (let i = 0; i < len; i++) {
        let cur = strN[i];
        let higher = Number(strN.slice(0, i) || 0);
        let lower = Number(strN.slice(i + 1) || 0);
        let dig = 10 ** (len - i - 1);
        if (cur === '0') {
            result += Number(higher * dig);
        } else if (cur === '1') {
            result += Number(higher * dig + 1 + lower);
        } else {
            result += Number((higher + 1) * dig);
        }
    }
    return result;
};
```


## 44：数字序列中某一位的数字-M
```js
var findNthDigit = function(n) {
    let i = 1
    // 如何判断间隔宽度，即每个数字新增多少个0？
    // 一位数的假想序列长度为10，二位数的假想序列为200（000102...9899），三位数3000以此类推，所以假想序列的长度为 i*10^i
    // 而实际的 n 应该落在这个假想序列的范围内。当 i*10^i < n 时，我们需要新增数位操作，来满足条件。
    while (i * Math.pow(10, i) < n) {
        // 新增数位导致 n 向后移
        n += Math.pow(10, i)
        i++
    }
    // 得出隔间数字，并转为字符
    const partition = Math.floor(n / i) + ""
    return partition.charAt(n % i)
};
```


## 45：把数组排成最小的数-M
```js
var minNumber = function (nums) {
   return minNums = nums.sort((a,b)=>{
        //利用js特性
        //字符串之间相减会像number一样正常减掉，会被转换成number
        //字符串之间相加会转换成两个字符串之间拼接，会被转换成string
        if(`${a}${b}` > `${b}${a}`){
            //b 会被排列到 a 之前
            return 1
        }else{
            //小于 0,a 会被排列到 b 之前
            return -1;
        }
    }).join("");
};
```


## 49：丑数
```js
// 题目：我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。
var nthUglyNumber = function (n) {
    let dp = [1];
    let a = 0, b = 0, c = 0;
    for (let i = 1; i < n; i++) {
        let n2 = dp[a] * 2;
        let n3 = dp[b] * 3;
        let n5 = dp[c] * 5;
        dp[i] = Math.min(n2, n3, n5);
        if (dp[i] === n2) a++;
        if (dp[i] === n3) b++;
        if (dp[i] === n5) c++;
    }
    return dp[n - 1];
};
```


## 51：数组中的逆序对-H
```js
// var reversePairs = function(nums) {
//     let sum = 0;
//     for(let i = 0;i<nums.length;i++){
//         for(let j = i+1;j<nums.length;j++){
//             if(nums[i] > nums[j]){
//                 sum++;
//             }
//         }
//     }
//     return sum;
// };
// 时间超出限制
// 调整为归并排序
var reversePairs = function (nums) {
    function mergeSort(arr) {
        if (arr.length < 2) return arr
        let len = arr.length
        let mid = Math.floor(len / 2)
        return merge(mergeSort(arr.slice(0, mid)), mergeSort(arr.slice(mid)))
    }
    function merge(left, right) {
        let res = []
        while (left.length && right.length) {
            if (left[0] <= right[0]) {
                res.push(left.shift())
            } else {
                // 归并排序唯一添加的一行代码
                // 如果是添加右边数组的元素，证明左边剩余元素都能和自己组成逆序对
                // 例如[3,5,7],[2,4]，遍历到2的时候，左边的[3,5,7]都能和2组成逆序对
                result += left.length
                res.push(right.shift())
            }
        }
        return res.concat(left, right)
    }
    let result = 0
    mergeSort(nums)
    return result
};
```


## 53-1：在排序数组中查找数字-E
```js
var search = function(nums, target) {
    let sum = 0;
    for(let i = 0;i<nums.length;i++){
        if(nums[i] === target){
            sum++;
        }
        if(nums[i] > target){
            return sum;
        }
    }
    return sum;
};
```


## 53-2：0~n-1中缺失的数字-E
```js
var missingNumber = function(nums) {
    let set = new Set(nums);
    for(let i = 0;i <= nums.length;i++){
        if(!set.has(i)){
            return i;
        }
    }
};
```


## 57-1：和为s的两个数字-E
```js
var twoSum = function(nums, target) {
    let set = new Set(nums);
    for(let i = 0;i<nums.length;i++){
       if(set.has(target - nums[i])){
           return [nums[i],target - nums[i]]
       }
    }
};
```


## 57-2：和为s的连续正数序列-E
```js
var findContinuousSequence = function (target) {
    let result = [];
    let l = 1, r = 2;
    while (l > 0 && r <= target && l < r) {
        if (((l + r) * (r - l + 1) / 2) === target) {
            let item = [];
            for (let i = l; i <= r; i++) {
                item.push(i);
            }
            result.push(item)
            l++;
            r = l + 1;
        } else if (((l + r) * (r - l + 1) / 2) > target) {
            l++;
            r = l + 1;
        } else {
            r++;
        }
    }
    return result;
};
```


## 59-1：滑动窗口的最大值-H
```js
var maxSlidingWindow = function (nums, k) {
    let res = [];//保存答案
    let queue = [];//要维护的队列,队列中存的数组下表，不是数组元素
    let len = nums.length;
    for (let i = 0; i < len; i++) {
        // 与I一起维护队列在范围内
        while (queue.length && queue[0] <= i - k) {
            queue.shift();
        }
        //进来的元素>=队尾元素，就将队尾元素弹出
        while (queue.length && nums[queue[queue.length - 1]] <= nums[i]) {
            queue.pop();
        }

        queue.push(i);
        // 从下标是k-1的时候就开始插入
        if (i >= k - 1) {
            res.push(nums[queue[0]]);
        }
    }
    return res;
};
```


## 61：扑克牌中的顺子-E
```js
var isStraight = function(nums) {
    let king = 0;
    let start = Infinity;
    let set = new Set(nums);
    let result = true;
    nums.forEach(item => {
        if (item === 0) {
            king++;
        }
        if (item && item < start) {
            start = item;
        }
    });
    for (let i = start; i < start + 5; i++) {
        if (!set.has(i)) {
            if (king) {
                king--;
            } else {
                result = false;
            }
        }
    }
    return result;
};
```


## 63：股票的最大利润
```js
var maxProfit = function(prices) {
    let max = 0;
    let len = prices.length;
    for(let i = 0;i<len;i++){
        for(let j = i+1;j<len;j++){
            if(prices[j] - prices[i] > max){
                max = prices[j] - prices[i];
            }
        }
    }
    return max
};
```


## 64：求1+2+...+n -M
```js
var sumNums = function(n) {
 return n && sumNums(n-1)+n;
};
```


## 66：构建乘积数组-M
```js
var constructArr = function(a) {
    let product = 1;
    let result=[];
    for(let i = 0;i<a.length;i++){
        result[i] = product;
        product *= a[i];
    }
    product = 1;
    for(let i = a.length - 1;i>=0;i--){
        result[i] *= product;
        product *= a[i];
    }
    return result;
};
```



# 字符串
## 05：替换空格-E
```js
var replaceSpace = function(s) {
   return s.replace(/ /g,'%20');
};
```


## 19：正则表达式匹配-H
```js
var isMatch = function(s, p) {
    let dp=new Array(s.length+1).fill(0).map(()=>new Array(p.length+1).fill(0));//dp的初始化
    const [m,n]=[s.length, p.length];
    const match=(i,j)=>{//判断两个字符是否匹配，包括.的情况
        if(j==='.'){
            return true;
        }
        if(j!=='.'){
            if(i===j){
                return true;
            }
        }
        return false;
    }
    dp[0][0]=true;//两个空字符串可以匹配
    for(let j=1;j<=n;j++){//当s为空时，p的不同情况来初始化dp(和下面的for循环结合进行初始化)
        dp[0][j]=false;
    }
    for(let j=2;j<=n;j=j+2){//
        dp[0][j]=dp[0][j-2]&&p[j-1]==='*';//当s为空时，p的不同情况来初始化dp(和上面的for循环结合进行初始化)
    }
    for(let i=1;i<=m;i++){
        dp[i][0]=false; //当p不为空，s为空时，dp初始化为fasle
    }
    //后面都是按照官方的题解来分析的结果
    for(let i=1;i<=m;i++){
        for(let j=1;j<=n;j++){
            if(p[j-1]!=='*'){
                if(match(s[i-1],p[j-1])){
                    dp[i][j]=dp[i-1][j-1];
                }else{
                    dp[i][j]=false;
                }
            }else{
                if(match(s[i-1],p[j-2])){
                    dp[i][j]=dp[i-1][j]||dp[i][j-2];
                }else{
                    dp[i][j]=dp[i][j-2];
                }
            }
        }
    }
    return dp[m][n];
};
```


## 20：表示数值的字符串
```js
var isNumber = function(s) {
    let i, len, numFlag = false, dotFlag = false, eFlag = false;
    s = s.trim(); // 去掉首尾空格
    len = s.length; // 去掉后再重新计算长度
    for(i = 0; i < len; i++) {
        // 如果是数字，那么直接将 numFlag 变为 true 即可
        if(s[i] >= '0' && s[i] <= '9') {
            numFlag = true;
        } else if(s[i] === '.' && !dotFlag && !eFlag) {
            // 如果是 .  那必须前面还出现过 .  且前面没出现过 e/E，因为如果前面出现过 e/E 再出现. 说明 e/E 后面跟着小数，不符合题意
            dotFlag = true;
        } else if((s[i] === 'e' || s[i] === 'E') && !eFlag && numFlag) {
            // 如果是 e 或 E，那必须前面没出现过 e/E，且前面出现过数字
            eFlag = true;
            numFlag = false; // 这一步很重要，将是否出现过数字的 Flag 置为 false，防止出现 123E 这种情况，即出现 e/E 后，后面没数字了
        } else if((s[i] === '+' || s[i] === '-') && (i === 0 || (s[i - 1] === 'e' || s[i - 1] === 'E'))) {
            // 如果是 +/- 那必须是在第一位，或是在 e/E 的后面
        } else {
            // 上面情况都不满足，直接返回 false 即可，提前剪枝
            return false;
        }
    }
    return numFlag;
};
```


## 38：字符串的排列
```js
var permutation = function(s) {
  let res = [];
  const dfs = (path, keys) => {
    if (path.length == s.length) {
      let val = path.slice().join("");
      if (res.indexOf(val) < 0) res.push(val);
      return;
    }
    for (let i = 0; i < s.length; i++) {
      // 为什么要用这个来判断呢，举例来说，"aab"，如果以s[i]来判断的话，那已经加入了第0个a是不是不能加入第1个工了
      // 所以不能仅仅用s[i]来判断是不已经做过选择了
      let pathKey = i + "&" + s[i];
      // 已经存在于path中的就不需要再重复再出现了
      if (keys.indexOf(pathKey) > -1) continue;
      keys.push(pathKey);
      path.push(s[i]);
      dfs(path, keys);
      path.pop();
      keys.pop();
    }
  };
  dfs([], []);
  return res;
};
```


## 48：最长不含重复字符的子字符串
```js
var lengthOfLongestSubstring = function (s) {
    let i = 0;
    let j = 0;
    let item = '';
    let result = '';
    for (; i < s.length; i++) {
        for (; j < s.length; j++) {
            if (item.indexOf(s[j]) === -1) {
                item = s.slice(i, j + 1)
                if (item.length > result.length) {
                    result = item;
                }
            } else {
                item = '';
                j = i + 1;
                break;
            }

        }

    }
    return result.length
};
```


## 50：第一次只出现一次的字符
```js
var firstUniqChar = function(s) {
    let result = ' ';
    for(let i = 0;i<s.length;i++){
            if((s.slice(0,i)+s.slice(i+1)).indexOf(s[i]) === -1){
                result = s[i];
                break;
            
        }
    }
    return result;
};
```


## 58-1：翻转单词顺序
```js
var reverseWords = function(s) {
    return s.split(' ').filter(item=>item!=='').reverse().join(' ');
};
```


## 58-2：左旋转字符串
```js
var reverseLeftWords = function(s, n) {
    return s.slice(n)+s.slice(0,n)
};
```


## 67：把字符串转换成整数
```js
var strToInt = function(str) {
  str = str.trim(); // 去掉前后空格
    if(str == ""){   
        return 0;  
    }
    var INT_MIN = -Math.pow(2,31), INT_MAX = Math.pow(2,31) - 1;
    var reg = new RegExp(/^[\+\-]?\d+/) ;
    var result = str.match(reg);
    if(result != null){
        if(result[0][0] == "-" && result[0] < INT_MIN){  
            return INT_MIN;
        }else if(result > INT_MAX){
            return INT_MAX;
        }  
        return result; 
    }else{
        return 0;
    }
};
```


# 链表
## 06：从头到尾打印链表
```js
var reversePrint = function(head) {
    let res = [];
    while(head){
        res.unshift(head.val);
        head = head.next;
    }
    return res
};
```


## 18：删除链表的节点
```js
var deleteNode = function(head, val) {
    let res = new ListNode(-1);
    res.next = head;
    let p = res;
    while(p){
        if(p.next.val === val){
            p.next = p.next.next;
            return res.next;
        }
        p = p.next;
    }
};
```


## 22：链表中倒数第k个节点
```js
var getKthFromEnd = function(head, k) {
    let res = [];
    while(head){
        res.push(head);
        head = head.next;
    }
    return res[res.length - k];
};
```


## 24：反转链表
```js
var reverseList = function(head) {
    let prev = null;
    let curr = head;
    while(curr){
        let next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
};
```


## 25：合并两个排序的链表
```js
var mergeTwoLists = function(l1, l2) {
    let list = new ListNode(null);
    let l = list;
    while(l1 && l2){
        if(l1.val <= l2.val){
            l.next = l1;
            l1=l1.next;
        }else{
            l.next = l2;
            l2=l2.next;
        }
        l = l.next;
    }
    l.next = l1 === null ? l2 : l1;
    return list.next;
};
```


## 35：复杂链表的复制
```js
var copyRandomList = function (head) {
    if (head === null) return head;

    let curr = head;
    let newHead = new Node();
    let newCurr = newHead;
    let map = new Map();

    while (curr) {
        // 新链表复制 val 值和 next 指向
        newCurr.val = curr.val;
        newCurr.next = curr.next ? new Node() : null;
        map.set(curr, newCurr);// 把 newCurr 的值存起来
        newCurr = newCurr.next;
        curr = curr.next;
    }

    newCurr = newHead;
    while (head) {
        // 通过引用地址找到对应的链表节点
        newCurr.random = head.random ? map.get(head.random) : null;
        head = head.next;
        newCurr = newCurr.next;
    }

    return newHead;
};
```


## 52：两个链表的第一个公共节点
```js
var getIntersectionNode = function (headA, headB) {
    let a = headA, b = headB
    while (a !== b) {
        a = a ? a.next : headB
        b = b ? b.next : headA
    }
    return a
};
```

# 栈/队列

## 09：用两个栈实现队列
```js
var CQueue = function() {
    this.inStack = [];
    this.outStack = [];
};

/** 
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function(value) {
    this.inStack.push(value)
};

/**
 * @return {number}
 */
CQueue.prototype.deleteHead = function() {
    if (!this.outStack.length) {
        if (!this.inStack.length) {
            return -1;
        }
        while (this.inStack.length) {
            this.outStack.push(this.inStack.pop());
        }
    }
    return this.outStack.pop();
};
```


## 30：包含 min 函数的栈
```js
var MinStack = function() {
    this.arr = [];
    this.minStack = [Infinity];
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
    this.arr.push(x);
    this.minStack.push(Math.min(this.minStack[this.minStack.length-1],x));
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    this.arr.pop();
    this.minStack.pop();
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.arr[this.arr.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.min = function() {
    return this.minStack[this.minStack.length - 1];
};
```


## 31：栈的压入、弹出序列
```js
// 模拟法
var validateStackSequences = function(pushed, popped) {
    let stack = [];
    let i = 0;
    for (const item of pushed) {
        stack.push(item);
        while (stack.length > 0 && stack[stack.length - 1] === popped[i]) {
            stack.pop();
            i++;
        }
    }
    return stack.length === 0
};
```


## 59-II：队列的最大值
```js
var MaxQueue = function () {
    this.queue = [];
    this.maxValueStack = [];
};

/**
 * @return {number}
 */
MaxQueue.prototype.max_value = function () {
    if (!this.maxValueStack.length) {
        return -1;
    }
    return this.maxValueStack[0];
};

/** 
 * @param {number} value
 * @return {void}
 */
MaxQueue.prototype.push_back = function (value) {
    this.queue.push(value);
    while (this.maxValueStack.length && this.maxValueStack[this.maxValueStack.length - 1] < value) {
        this.maxValueStack.pop();
    }
    this.maxValueStack.push(value);
};

/**
 * @return {number}
 */
MaxQueue.prototype.pop_front = function () {
    let front = this.queue.shift()
    if (front === this.maxValueStack[0]) {
        this.maxValueStack.shift();
    }
    return front || -1
};
```


# 树

## 07：重建二叉树
```js
// 方法 1: 递归, 传入前序/中序起点和终点, 结合{值:中序下标}快速定位根节点的中序下标位置
// 方法 2: 迭代, 栈存前面的节点, 分析当前节点在左还是在右

var buildTree = function (preorder, inorder) {
    if (preorder.length === 0 || inorder.length === 0) return null;
    // 前序遍历的第一个节点定然是当前树的根节点
    const root = new TreeNode(preorder[0]);
    // 拿 root 把中序遍历的数组劈开，左边为左子树，右边为右子树
    let rootIndex = inorder.findIndex(item => item === preorder[0]);
    if (rootIndex === -1) return null;
    preorder.shift(); // 删除掉先序遍历的根节点
    // 左边的数组定然全部都是当前根节点的左子树上的节点
    root.left = buildTree(preorder, inorder.slice(0, rootIndex));
    // 右边的数组定然全部都是当前根节点的右子树上的节点
    root.right = buildTree(preorder, inorder.slice(rootIndex + 1));
    return root;
};
```


## 26：树的子结构
```js
// dfs遍历对比两棵树
var isSubStructure = function(A, B) {
    if(!A ||!B) return false;
    function dfs(A,B){
        if(B === null) return true;
        if(A === null) return false;
        return A.val === B.val && dfs(A.left,B.left) && dfs(A.right,B.right);
    }
    return dfs(A,B) || isSubStructure(A.left,B) || isSubStructure(A.right,B)
};
```


## 27：二叉树的镜像
```js
// 方法 1: DFS 递归, 原地修改
// 方法 2: BFS 遍历, 无需按层遍历, 原地修改当前 node 的左右节点
var mirrorTree = function(root) {
    if(root === null){
        return root;
    }
    let left = mirrorTree(root.left);
    let right = mirrorTree(root.right);
    root.left = right;
    root.right = left;
    return root
};
```



## 28：对称的二叉树
```js
// 方法 1: DFS 递归, 先生成镜像再逐个比较
// 方法 2: DFS 递归, 直接按照是否对称比较
// 方法 3: BFS 遍历, 存左右两个列表, 需要存空节点
var isSymmetric = function(root) {
    if(root === null){
        return root;
    }
    function reverse(left,right){
        if(left === null &&right === null){
            return true;
        }
        if(left=== null || right=== null || left.val!==right.val){
            return false
        }
        return reverse(left.left,right.right) && reverse(right.left,left.right);

    }
    return reverse(root.left,root.right);
};
```



## 32-I. 从上到下打印二叉树
```js
// 方法 1: Python 列表+for 循环
// 方法 2: 双端队列+while 循环
var levelOrder = function(root) {
    let res = [];
    if(!root) return res;
    let queue = [root];
    while(queue.length){
        const item = queue.shift();
        res.push(item.val);
        if(item.left){
            queue.push(item.left);
        }
        if(item.right){
            queue.push(item.right);
        }
    }
    return res;
};
```



## 32-II. 从上到下打印二叉树 II
```js
// 按层 BFS, 记录当前层长度 curlen
var levelOrder = function(root) {
    let res = [];
    if (!root) return res;
    let queue = [root];
    let map = new Map();
    map.set(root, 0);
    while (queue.length) {
        const item = queue.shift();
        let index = map.get(item);
        if (!res[index]) {
            res[index] = [];
        }
        res[index].push(item.val);
        if (item.left) {
            queue.push(item.left);
            map.set(item.left, index + 1);
        }
        if (item.right) {
            queue.push(item.right);
            map.set(item.right, index + 1);
        }
    }
    return res;
};
```


## 32-III. 从上到下打印二叉树 III
```js
// 按层 BFS+方向 flag
var levelOrder = function(root) {
  let res = [];
    if (!root) return res;
    let queue = [root];
    let map = new Map();
    map.set(root, 0);
    let tag = false;
    while (queue.length) {
        const item = queue.shift();
        let index = map.get(item);
        if (!res[index]) {
            res[index] = [];
            tag = !tag;
        }
        if(tag){
            res[index].push(item.val);
        }else{
            res[index].unshift(item.val);
        }
        if (item.left) {
            queue.push(item.left);
            map.set(item.left, index + 1);
        }
        if (item.right) {
            queue.push(item.right);
            map.set(item.right, index + 1);
        }
    }
    return res;
};
```


## 33：二叉搜索树的后序遍历序列
```js
// 方法 1: 递归, 分治法, 快速排序思想
// 方法 2: 迭代, 维护单调递增栈+根节点
function verifyPostorder(postorder) {
    // 终止条件，树节点 <=2 返回 true
    if (postorder.length <= 2) return true;
    // 根节点
    const root = postorder[postorder.length - 1];
    // 寻找 第一个大于根节点 的节点 inx
    const idx = postorder.findIndex((item) => item > root);

    // 划分出：左子树区间 [left, inx - 1]
    const left = postorder.slice(0, idx);
    // 划分出：右子树区间 [inx, postorder.length - 1]
    const right = postorder.slice(idx, -1);
    // 二叉搜索树的右子树和根相比，最小值一定是根值
    // 因为前面根据找到的第一个大于根节点的值，所以左区间都小于根节点，无需判断
    if (Math.min(root, ...right) !== root) return false
    return verifyPostorder(left) && verifyPostorder(right)
};
```



## 34：二叉树中和为某一值的路径
```js
// 方法 1: DFS, 传入当前节点/路径/路径和
// 方法 2: BFS 存三元组(当前节点, 路径, 路径和)
var pathSum = function(root, target) {
    let res = [];
    function dfs(item = [],root,target){
        if(!root) return;
        item.push(root.val);
        if(target === root.val && !root.left && !root.right){
            res.push(item.slice());
        }
        dfs(item, root.left,target-root.val);
        dfs(item,root.right,target-root.val);
        item.pop();
    }
    dfs([],root,target)
    return res;
};
```



## 36：二叉搜索树与双向链表
```js
// 方法 1: 递归分治法, 返回转换后的链表头和尾
// 方法 2: 中序遍历, 存 pre 节点, 与当前节点相连
var treeToDoublyList = function (root) {
   function dfs(root) {
       if (root == null) return null; // 递归边界: 叶子结点返回
       dfs(root.left);
       // 当 pre === null 的时候，为最小的叶子节点，设置成链表头结点
       pre != null ? pre.right = root : head = root
       root.left = pre;
       pre = root; // 链表指针向右移动
       dfs(root.right);
   }
   let pre = null, head = null;
   if (root == null) return root;
   dfs(root);
   head.left = pre;
   pre.right = head;
   return head;
};
```



## 37：序列化二叉树
```js
// BFS, 存节点详细信息[当前节点下标, 父节点下标, 父节点指向方向, 节点]
var serialize = function(root) {
    if(!root){
        return [];
    }
    let res = [];
    /*
    1. bfs遍历
    2. 
    */
    let queue = [root];
    while(queue.length){
        let cur = queue.pop();
        //为空也要放数组里
        if(!cur){
            res.push(cur);
            continue;
        }
        res.push(cur.val);
        queue.unshift(cur.left);
        queue.unshift(cur.right);
    }
    return res;
};


var deserialize = function(data) {
    if(!data || !data.length){
        return null;
    }
    //利用一个queue,第1个元素不可能为空的
    let root = new TreeNode(data[0]);
    let queue = [root];
    let i = 1;
    while(i<data.length){
        let cur = queue.pop();
        if(i<data.length){
            if(data[i]!==null){
                cur.left = new TreeNode(data[i]);
                queue.unshift(cur.left);
            }
            i++;
        }
        if(i<data.length){
            if(data[i]!==null){
                cur.right = new TreeNode(data[i]);
                queue.unshift(cur.right);
            }
            i++;
        }
    }
    return root;
};
```


## 54：二叉搜索树的第 k 大节点
```js
// 中序遍历翻转, 先右子树再根再左子树
var kthLargest = function(root, k) {
    let res=[];
    function dfs(root){
        if(!root) return;
        dfs(root.left);
        res.push(root.val);
        dfs(root.right);
    }
    dfs(root)
    return res[res.length-k];
};
```


## 55： I. 二叉树的深度
```js
// 方法 1: DFS 递归求深度
// 方法 2: BFS 求层数
var maxDepth = function(root) {
    if(!root) return 0
    let queue = [root];
    let deep = 0;
    while(queue.length){
        const len = queue.length;
        for(let i = 0;i<len;i++){
            const item = queue.shift();
            if(item.left){
                queue.push(item.left);
            }
            if(item.right){
                queue.push(item.right);
            }
        }
        deep++;
    }
    return deep;
};
```



## 55： II. 平衡二叉树
```js
// 递归+全局标记, 边求深度边判断, 返回深度, 全局变量标记当前是否平衡, 不平衡时直接返回 0
var isBalanced = function(root) {
    var flag = true;
    //  输出每个节点的高度
    var Btdepth = function(root) {
        if(root === null) {
            return 0;
        }
        let ldepth = Btdepth(root.left);
        let rdepth = Btdepth(root.right);
        if(Math.abs(ldepth - rdepth) > 1){
            flag = false;
        }
        return ldepth > rdepth ? ldepth + 1 : rdepth + 1;
    }
    Btdepth(root);
    return flag;
};
```


## 68： I. 二叉搜索树的最近公共祖先
```js
// 根据根节点和 p/q 的值的关系来决定找到/向左/向右
 var lowestCommonAncestor = function(root, p, q) {
    let ans;
    const dfs = (root, p, q) => {
        if (root === null) return false;
        const lson = dfs(root.left, p, q);
        const rson = dfs(root.right, p, q);
        if ((lson && rson) || ((root.val === p.val || root.val === q.val) && (lson || rson))) {
            ans = root;
        } 
        return lson || rson || (root.val === p.val || root.val === q.val);
    }
    dfs(root, p, q);
    return ans;
};
```


## 68： II. 二叉树的最近公共祖先
```js
// 方法 1: 递归, 返回 findp 和 findq, 注意祖先只能赋值一次, 这样才是最近的祖先
// 方法 2: 迭代, parent 字典+visit 集合, 找到一个在 v 集合的父节点后就直接返回, 这样才是最近祖先
var lowestCommonAncestor = function(root, p, q) {
    let ans;
    const dfs = (root, p, q) => {
        if (root === null) return false;
        const lson = dfs(root.left, p, q);
        const rson = dfs(root.right, p, q);
        if ((lson && rson) || ((root.val === p.val || root.val === q.val) && (lson || rson))) {
            ans = root;
        } 
        return lson || rson || (root.val === p.val || root.val === q.val);
    }
    dfs(root, p, q);
    return ans;
};
```



# 图

## 12：矩阵中的路径
```js
// DFS, 找有效起点, 记录当前路径, 使用 visit 集合或改变原数组的值来标记已经遍历的点
var exist = function (board, word) {
    function search(wn, i, j) {
        if (board[i][j] === word[wn]) {
            let temp = board[i][j];
            board[i][j] = true;
            if (wn === word.length - 1) {
                return true
            }
            // 上
            if (board?.[i - 1]?.[j] && board?.[i - 1]?.[j] !== true) {
                if (search(wn + 1, i - 1, j)) {
                    return true;
                }
            }
            // 下
            if (board?.[i + 1]?.[j] && board?.[i + 1]?.[j] !== true) {
                if (search(wn + 1, i + 1, j)) {
                    return true;
                };
            }
            // 左
            if (board?.[i]?.[j - 1] && board?.[i]?.[j - 1] !== true) {
                if (search(wn + 1, i, j - 1)) {
                    return true;
                };
            }
            // 右
            if (board?.[i]?.[j + 1] && board?.[i]?.[j + 1] !== true) {
                if (search(wn + 1, i, j + 1)) {
                    return true;
                };
            }
            board[i][j] = temp;
        }
    }
    for (let i = 0; i < board.length; i++) {
        for (let j = 0; j < board[0].length; j++) {
            if (board[i][j] === word[0]) {
                if (search(0, i, j)) {
                    return true;
                }
            }
        }
    }
    return false;
};
```


## 13：机器人的运动范围
```js
// 方法 1: DFS + visit 集合
// 方法 2: BFS + visit 集合
// 方法 3: DP, 如果 rc 数位和大于 k, dp[r,c] = False, 否则 dp[r,c] = dp[r-1,c] or dp[r,c-1]
var movingCount = function(m, n, k) {
    const grid =  Array.from({length:m},()=>new Array(n).fill(1));
    let count = 0;
    function hepler(i,j){
        if(i<0 || j<0 || i>m-1 || j>n-1 || grid[i][j] === 0 || getSum(i,j)>k) return;
        count++
        grid[i][j] = 0;
        //hepler(i-1,j);
        hepler(i+1,j);
        //hepler(i,j-1);
        hepler(i,j+1);
    }
    hepler(0,0)
    return count;
};
function getSum(num1,num2){
    let sum = 0;
    for(let num of num1+''+num2) {
        sum += +num;
    }
    return sum;
}
```



## 29：顺时针打印矩阵
```js
// 方法 1: 维护当前下标和方向和 visit 集合, 逐个点遍历
// 方法 2: 按层遍历, 剥洋葱, 注意可能不存在向左或向上的部分
var spiralOrder = function(matrix) {
    if(matrix.length == 0) return [];//如果为空数组直接返回
    let t = 0;//上边界
    let b = matrix.length - 1;//下边界
    let l = 0;//左边界
    let r = matrix[0].length - 1;//右边界
    let res = [];
    while(true) {//模拟顺时针的走向
        for(let i = l; i <= r; i++) {//从左到右
            res.push(matrix[t][i]);
        }
        if(++t > b) break;//更新边界并判断新边界是否超过范围
        for(let i = t; i <= b; i++) {//从上到下
            res.push(matrix[i][r]);
        }
        if(--r < l) break;
        for(let i = r; i >= l; i--) {//从右到左
            res.push(matrix[b][i]);
        }
        if(--b < t) break;
        for(let i = b; i >= t; i--) {//从下到上
            res.push(matrix[i][l]);
        }
        if(++l > r) break;
    }
    return res;
};
```



# 位运算

## 15：二进制中 1 的个数
```js
// 方法 1: 转二进制字符串统计
// 方法 2: 循环移位统计
// 方法 3: n &= n - 1
// 方法 4: n -= n & -n
var hammingWeight = function(n) {
    let res = 0;
    for(let i = 0; i < 32; i++){
        if((n & (1 << i)) !== 0){
            res++;
        }
    }
    return res;
};
```



## 16：数值的整数次方
```js
// 方法 1: 二分分治法递归+记忆化搜索, 分别调用 n//2 和 n-n//2, 注意递归出口
// 方法 2: 快速幂, 奇数乘入结果, 偶数不变, 然后幂右移, 数字乘以自身
var myPow = function(x, n) {
     let reusult = 1.0
    //如果负数，2^-2可以编程 （1/2）^2
    if(n<0){
        //js中默认不是整除
        x = 1/x 
        n = -n
    }
    while(n>0){
        if(n&1){
            reusult*=x
        }
        x*=x
        //我的天，js中的移位还出错
        //原来是>>是有符号数的移位，>>>这个是无符号数的移位
        //下面第一种是错误的，剩下两个都是正确的
        // n = n>>1
        // n = Math.floor(n/2)
        n = n>>>1
    }
    return reusult
}
```



## 56： I. 数组中数字出现的次数
```js
// 其他数字出现偶数次, 某两个数字各出现奇数次 => 异或后根据异或结果的某个 1 分两类
var singleNumbers = function (nums) {
    // 求所有数字异或和
    let sum = 0;
    for (let num of nums) {
        sum ^= num;
    }
    // 找异或和第一个为 1 的位
    let mask = 1;
    while ((sum & mask) == 0) {
        mask <<= 1;
    }
    // 以该位为依据分组异或
    let x = 0;
    let y = 0;
    for (let num of nums) {
        if ((num & mask) == 0) {
            x ^= num;
        } else {
            y ^= num;
        }
    }
    return [x, y];
};
```


## 56： II. 数组中数字出现的次数 II
```js
// 其他数字出现奇数次, 某个数字出现 1 次 => 按照二进制每一位统计次数, 然后对这个奇数次取模
var singleNumber = function(nums) {
   let res = 0;
   for(let i = 0; i < 32; i++) {
       let cnt = 0;
       let bit = 1 <<i;
       for(let j = 0; j < nums.length;j++) {
           if(nums[j] & bit) cnt ++;
       }
       if(cnt % 3 !==0 ) res = res | bit
   }
   return res;
};
```


## 65：不用加减乘除做加法
```js
// 利用 (a&b)<<1 求进位, 利用 a^b 求无进位和, 两者赋值为新的 a 和 b 不断循环, 直到进位为 0 即可, 注意 python 负数的处理
var add = function(a, b) {
  while (b != 0) {
        const carry = (a & b) << 1;
        a = a ^ b;
        b = carry;
    }
    return a;
};
```



# 动态规划
## 10：I. 斐波那契数列
```js
// 两变量动态规划, 存前两个值, 当前值为它们的和
var fib = function(n) {
    let x = 0;
    let y = 1;
    let res = 0;
    if(n === 0){
        return 0;
    }else if(n===1){
        return 1;
    }
    for(let i = 0;i < n-1;i++){
        res = ( x + y ) % 1000000007;
        [x,y]=[y,res];
    }
    return res;
};
```


## 10：II. 青蛙跳台阶问题
```js
// 两变量动态规划, 存前两个值, 当前值为它们的和, 与上一题唯一区别是初始化不同
var numWays = function(n) {
    if(n === 0 || n === 1){
        return 1;
    }
    if( n === 2){
        return 2;
    }
    let x = 1;
    let y = 2;
    let res = 0;
    for(let i = 3; i<=n; i++){
        res = x + y;
        [x,y] = [y,res% 1000000007];
    };
    return y ;
};
```


## 42：连续子数组的最大和
```js
// 动态规划+空间优化, dp 存当前数字结尾的最大和, dp = max(dp + x, x)
var maxSubArray = function(nums) {
    let pre = 0, maxAns = nums[0];
    nums.forEach((x) => {
        pre = Math.max(pre + x, x);
        maxAns = Math.max(maxAns, pre);
    });
    return maxAns;
};
```


## 46：把数字翻译成字符串
```js
// 动态规划, dp[i]代表末尾第 i 位时可以翻译的字符串数目, dp[i] = dp[i-2] (使用2个字符) + dp[i-1] (使用1个字符) if 10 <= int(s[i - 1:i + 1]) <= 25 else dp[i-1]
const translateNum = (num) => {
  const str = num.toString();
  const n = str.length;

  const dp = new Array(n + 1);
  dp[0] = 1;
  dp[1] = 1;

  for (let i = 2; i < n + 1; i++) {
    const temp = Number(str[i - 2] + str[i - 1]);
    if (temp >= 10 && temp <= 25) {
      dp[i] = dp[i - 1] + dp[i - 2];
    } else {
      dp[i] = dp[i - 1];
    }
  }
  return dp[n]; // 翻译前n个数的方法数，即翻译整个数字
}

function translateNum(num) {
    const dfs = (str, inx) => {
        // 指针抵达边界和超出边界返回 1
        if (inx >= str.length - 1) return 1;

        // 计算该 2 位数
        const sum = Number(str[inx] + str[inx + 1]);

        // 和在 [10,25] 之间可以直译
        if (sum >= 10 && sum <= 25) {
            // 2 个分支的结果相加
            return dfs(str, inx + 1) + dfs(str, inx + 2);
        } else {
            // 返回 1 个分支的结果
            return dfs(str, inx + 1);
        }
    }
    return dfs(num.toString(), 0);
};
```



## 47：礼物的最大价值
```js
// 动态规划, dp[r,c]代表格子(r,c)所能获得的最大价值, dp[r,c] = max(dp[r-1,c], dp[r,c-1]) + grid[r][c] (r-1>=0 and c-1>=0)
var maxValue = function(grid) {
  if (grid.length === 0 || grid[0].length === 0) return 0;
  let rowLimit = grid.length,
      colLimit = grid[0].length;
  
  for (let row = 0; row < rowLimit; row++) {
    for (let col = 0; col < colLimit; col++) {
      let left = col - 1 < 0 ? 0 : grid[row][col - 1],
          top = row - 1 < 0 ? 0 : grid[row - 1][col];
      
      grid[row][col] += Math.max(left, top);
    }
  }
  
  return grid[rowLimit - 1][colLimit - 1];
};
```


## 60：n 个骰子的点数
```js
// 动态规划+空间优化, dp 存当前骰子数下面的所有可能取值的概率, 推导出骰子数+1 的所有概率
const dicesProbability = function (n) {
    let f = new Array(n + 1).fill(0).map(i => new Array(6 * n + 1).fill(0));
    // f[n][s],代表第n个骰子拼出点数为S的方案数
    f[0][0] = 1;
    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= i * 6; j++) {
            // 防止 j - k 小于0溢出了
            for (let k = 1; k <= Math.min(j, 6); k++) {
                f[i][j] += f[i - 1][j - k];
            }
        }
    }
    let res = f[n].slice(n);
    let sum = res.reduce((p, c) => p + c, 0);
    return res.map(v => v / sum);
};
```


## 62：圆圈中最后剩下的数字
```js
// 直接模拟实现,遗憾超时
var lastRemaining = function(n, m) {
  const arr = Array(n)
    .fill(0)
    .map((v, k) => k);
  let start = 0;
  while (n > 1) {
    const index = (start + m - 1) % n;
    n--;
    start = index + 1 > n ? 0 : index;
    arr.splice(index, 1);
  }
  return arr[0];
};

// 动态规划+空间优化, 反推, dp[n, m] = (dp[n-1,m]+m)%n, 遍历[2,n], 注意推导过程
var lastRemaining = function (n, m) {
  // base case 最终活下来那个人的初始位置 当i为1时，当然是索引0啦
  let pos = 0;
  for (let i = 2; i <= n; i++) {
    pos = (pos + m) % i;
  }
  return pos;
};
```




