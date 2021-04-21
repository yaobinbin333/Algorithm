[[快速幂]]
## 分析
可以用循环和递归的方式来做。如果不用呢
我们其实只要找出a的最大次方是多少，然后用那个数模除以待判断的数即可
用Int类型最大的数：INT_MAX或者0x7ffffff
先找到3的最大幂次（3多少次方到INT_MAX），
然后用pow求出他的最大可以到的数字
最后用最大的数字模除以待求的数即可
## code
```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
const isPowerOfThree = function (n) {
  if (n <= 0) {
    return false;
  }
  // 求 3 的最大次幂
  const maxPow = parseInt((Math.log(0x7fffffff) / Math.log(3)));
  // 求 3 的 maxPow 次幂值
  const maxValue = Math.pow(3, maxPow);
  // 判断该值是否能整除待定值 n
  return (maxValue % n === 0);
};
```