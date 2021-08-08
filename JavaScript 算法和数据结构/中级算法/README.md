### 范围内的数字求和
我们会传入一个由两个数字组成的数组。 给出一个含有两个数字的数组，我们需要写一个函数，让它返回这两个数字间所有数字（包含这两个数字）的总和。 最低的数字并不总是第一位。 
* ES6扩展运算符
```
function sumAll(arr) {
  let num = 0;
  for(let i = Math.min(...arr); i <= Math.max(...arr); i++) {
    num += i;
  }
  return num;
}

sumAll([1, 4]);
```
* 等差数列求和, (首项+末项)*项数/2，项数 = 所需要加的次数
```
function sumAll(arr) {
  let newArr = arr.sort((a,b) => a-b);
  let first = newArr[0];
  let last = newArr[1];
  let sum = (first + last) * (last - first + 1) / 2;
  return num;
}

sumAll([1, 4]);
```
