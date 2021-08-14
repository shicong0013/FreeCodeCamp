* [范围内的数字求和](####范围内的数字求和)
* [数组的对称差](####数组的对称差)

#### 范围内的数字求和
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
  let num = (first + last) * (last - first + 1) / 2;
  return num;
}

sumAll([1, 4]);
```
#### 数组的对称差
比较两个数组并返回一个新数组，包含所有只在其中一个数组中出现的元素，排除两个数组都存在的元素。 换言之，我们需要返回两个数组的对称差。
* 合并两个数组后参照两个数组，只有有一个返回没有则添加至返回结果中
```
function diffArray(arr1, arr2) {
  return arr1.concat(arr2)
          .filter(
            x => !arr1.includes(x) || !arr2.includes(x)
          );
}

diffArray([1, 2, 3, 5], [1, 2, 3, 4, 5]);
```
* 分别检查两个数组中不重复的数组再合并
```
function diffArray(arr1, arr2) {
  return [...diff(arr1,arr2), ...diff(arr2,arr1)];
  
  function diff(a,b) {
    return a.filter(x => b.indexOf(x) === -1);
  }
}

diffArray([1, 2, 3, 5], [1, 2, 3, 4, 5]);
```
