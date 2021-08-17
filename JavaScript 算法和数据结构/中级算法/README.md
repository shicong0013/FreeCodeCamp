* [范围内的数字求和](#范围内的数字求和)
* [数组的对称差](#数组的对称差)
* [过滤数组元素](#过滤数组元素)
* [找出包含特定键值对的对象](#找出包含特定键值对的对象)

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

####  过滤数组元素
你将获得一个初始数组（destroyer 函数中的第一个参数），后跟一个或多个参数。 从初始数组中移除所有与后续参数相等的元素。
注意： 你可以使用 arguments 对象。
* 使用`Array.prototype.slice.call(arguments)`方法,删除重复的数组，返回去空后的数组
```
function destroyer(arr) {
  let args = Array.prototype.slice.call(arguments,1);
  for(let i = 0; i < arr.length; i++) {
    for(let j = 0; j < args.length; j++) {
      if(arr[i] === args[j]) {
        delete arr[i];
      }
    }
  }
  return arr.filter(Boolean);
}

destroyer([1, 2, 3, 1, 2, 3], 2, 3);
```
* 使用`Array.from(arguments)`方法，返回不重复的数组
```
function destroyer(arr) {
  let args = Array.from(arguments).slice(1);
  return arr.filter(x => !args.includes(x));
}

destroyer([1, 2, 3, 1, 2, 3], 2, 3);
```
* 使用扩展符号,第一个参数为arr，其余参数为args   

`const destroyer = (arr, ...args) => arr.filter(i => !args.includes(i));`   

#### 找出包含特定键值对的对象   
创建一个查看对象数组（第一个参数）的函数，并返回具有匹配的名称和值对的所有对象的数组（第二个参数）。 如果要包含在返回的数组中，则源对象的每个名称和值对都必须存在于集合中的对象中。   
比如，如果第一个参数是 `[{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }]`，第二个参数是 `{ last: "Capulet" }`。
```
function whatIsInAName(collection, source) {
  let Keys = Object.keys(source);
  // 只修改这一行下面的代码
  return collection.filter(function(obj) {
    for(let i = 0; i < Keys.length; i++) {
      //因为source有时是多组数据，当两个条件为真时如果返回真，那后一组数据条件不满足的话会导致结果错误
      //所以条件改为有一个条件为假时就返回假，到循环结束都没返回假时则这个对象为满足条件的对象
      if(!obj.hasOwnProperty(Keys[i]) || obj[Keys[i]] !== source[Keys[i]]) {
        return false;
      }
    }
    return true;
  })
  // 只修改这一行上面的代码
}

whatIsInAName([{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }], { last: "Capulet" });
```
* 优化，用`every()`代替`for`
```
function whatIsInAName(collection, source) {
  let keys = Object.keys(source);
  // 只修改这一行下面的代码
  return collection.filter(function(obj) {
    return keys.every(function(key) {
      //every会对数组内所有元素做判断，只有有一个不满足条件就返回false
      //所有可以用 &&
      return obj.hasOwnProperty(key) && obj[key] === source[key];
    })
  })
  // 只修改这一行上面的代码
}

whatIsInAName([{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }], { last: "Capulet" });
```
* 使用`map()`替换`every()`
```
function whatIsInAName(collection, source) {
  var keys = Object.keys(source);
  // 只修改这一行下面的代码
  return collection.filter(obj =>
    //map()会对数组内每个值进行条件判断并返回结果 
    keys.map(key => obj.hasOwnProperty(key) && obj[key] === source[key])
      //reduce()对mao()判断的结果再进行判断，全为真时则obj符合条件
      .reduce((a,b) => a && b ));
  // 只修改这一行上面的代码
}

whatIsInAName([{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }], { last: "Capulet" });
```
