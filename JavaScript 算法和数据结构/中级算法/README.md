* [范围内的数字求和](#范围内的数字求和)
* [数组的对称差](#数组的对称差)
* [过滤数组元素](#过滤数组元素)
* [找出包含特定键值对的对象](#找出包含特定键值对的对象)
* [短线连接格式](#短线连接格式)
* [儿童黑话](#儿童黑话)
* [搜索与替换](#搜索与替换)
* [DNA配对](#DNA配对)
* [寻找缺失的字母](#寻找缺失的字母)
* [集合排序](#集合排序)

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

#### 短线连接格式
将字符串转换为短线连接格式。 短线连接格式是小写单词全部小写并以破折号分隔。
```
function spinalCase(str) {
  let regx = /\s+|_+/g;
  str = str.replace(/([a-z])([A-Z])/g,'$1 $2'); //匹配小写与大写字母，$1和$2是对应的匹配结果，用空格分隔
  return str.replace(regx, '-').toLowerCase();
}

spinalCase('AllThe-small Things');
```
* 优化，使用`split()`分割字符串再连接
```
function spinalCase(str) {
  return str.split(/\s|_|(?=[A-Z])/).join('-').toLowerCase();
}
//正则(?=[A-Z])表示匹配以大写字母开始
spinalCase('This Is Spinal Tap');
```

#### 儿童黑话
儿童黑话也叫 Pig Latin，是一种英语语言游戏。 规则如下：  
-如果单词以辅音开头，就把第一个辅音字母或第一组辅音簇移到单词的结尾，并在后面加上 ay。  
-如果单词以元音开头，只需要在结尾加上 way。  
```
function translatePigLatin(str) {
  let pigLatin = '';
  //匹配元音,因为只用匹配一次所以不应加gi
  let regex = /[aeiou]/;
  //判断字符串第一个字母是否为元音
  if (str[0].match(regex)) {
      pigLatin = str + 'way';
    } else if(str.match(regex) === null) {
      //全辅音单词直接加ay
        pigLatin = str + 'ay';
      } else {
          //第一个元音的位置
          let vowelIndice = str.indexOf(str.match(regex));
          //第一个元音到最后一个字符+第一个字符到第一个元音+ay
          pigLatin = str.substr(vowelIndice) + str.substr(0, vowelIndice) + 'ay';
          }
  console.log(pigLatin);
  return pigLatin;
}

translatePigLatin("california");
```

#### 搜索与替换
在这道题目中，我们需要写一个字符串的搜索与替换函数，它的返回值为完成替换后的新字符串。   
这个函数接收的第一个参数为待替换的句子。   
第二个参数为句中需要被替换的单词。   
第三个参数为替换后的单词。   
注意： 在更换原始单词时保留原始单词中第一个字符的大小写。 即如果传入的第二个参数为 Book，第三个参数为 dog，那么替换后的结果应为 Dog   
```
function myReplace(str, before, after) {
  //需要被替换的单词首字母是否和进行大写转换的首字母一致
  if(str[index] === str[index].toUpperCase()) {
    //一致则把after首字母转化大写
    after = after.charAt(0).toUpperCase() + after.slice(1);
  } else {
    //不一致则说明before首字母为小写
    after = after.charAt(0).toLowerCase() + after.slice(1);
  }
  str = str.replace(before, after)
  return str;
}

myReplace("A quick brown fox jumped over the lazy dog", "jumped", "leaped");
```
* 使用正则
```
function myReplace(str, before, after) {
  //检查第一个字母是否为大写
  if(/[A-Z]/.test(before)) {
    after = after.charAt(0).toUpperCase() + after.slice(1);
  } else {
    after = after.charAt(0).toLowerCase() + after.slice(1);
  }
  str = str.replace(before, after)
  return str;
}

myReplace("A quick brown fox jumped over the lazy dog", "jumped", "leaped");
```

#### DNA配对
给出的 DNA 链上缺少配对元素。 请基于每个字符，获取与其配对的元素，并将结果作为二维数组返回。      
DNA 的碱基对 有两种形式：一种是 A 与 T，一种是 C 与 G。 请为参数中给出的每个字符配对相应的碱基。   
注意，参数中给出的字符应作为每个子数组中的第一个元素返回。   
例如，传入 GCG 时，应返回 [["G", "C"], ["C","G"], ["G", "C"]]。   
字符和它的配对组成一个数组中，所有配对数组放在一个数组里。   
* 遍历字符挨个替换
```
function pairElement(str) {
  let newStr = [];
  let search = function(x) {
    switch(x) {
      case 'A':
        newStr.push(['A','T']);
        break;
      case 'T':
        newStr.push(['T','A']);
        break;
      case 'C':
        newStr.push(['C','G']);
        break;
      case 'G':
        newStr.push(['G','C']);
        break;
    }
  }
  for(let i = 0; i < str.length; i++) {
    search(str[i]);
  }
  return newStr;
}

pairElement("GCG");
```
* 使用类似哈希表的方式，查找对应的匹配
```
function pairElement(str) {
  let pairs = {
    'A': ['A','T'],
    'T': ['T','A'],
    'C': ['C','G'],
    'G': ['G','C']
  }
  let arr = str.split('');
  return arr.map(x => pairs[x]);
}

pairElement("GCG");
```

#### 寻找缺失的字母
在这道题目中，我们需要写一个函数，找出传入的字符串里缺失的字母并返回它。   
如果所有字母都在传入的字符串范围内，返回 undefined。   
```
function fearNotLetter(str) {
  for(let i = 0; i < str.length; i++) {
    //当前循环的字母Unicode编码
    var code = str.charCodeAt(i);
    //判断当前字母Unicode编码是否和第一位编码+循环次数相等
    //字母的编码是连续的，如果没有缺失字母则当前字母的编码等于第一位编码+循环次数
    if(code !== str.charCodeAt(str[0]) + i) {
      //把当前字母的编码-1就是缺失的前一位字母
      return String.fromCharCode(code - 1);
    }
  }
  return undefined;
}

fearNotLetter("abce");
``` 
* 不使用循环
```
function fearNotLetter(str) {
  //第一位字母的Unicode
  let compare = str.charCodeAt(0);
  let missing;
  str.split('').map(function(letter,index) {
    //第一次执行函数时判断当前字母的编码是否和第一位字母的编码相等
    //相等的话则compare++，等于下一位字母的编码
    if(str.charCodeAt(index) === compare) {
      compare++;
    } else {
      //不相等则当前的compare是缺失的字母
      missing = String.fromCharCode(compare);
    }
  })
  return missing;
}

fearNotLetter("abce");
```
* 优化一下
```
function fearNotLetter(str) {
  for(let i = 1; i < str.length; i++) {
    //当前字母的编码如果大于前一个字母编码1以上时，则两个字母直接缺失字母
    if(str.charCodeAt(i) - str.charCodeAt(i-1) > 1) {
      //把当前字母的编码-1后则是缺失字母的编码
      return String.fromCharCode(str.charCodeAt(i) - 1)
    }
  }
}

fearNotLetter("abce");
```

#### 集合排序
编写一个带有两个或更多数组的函数，并按原始提供的数组的顺序返回一个新的唯一值数组。  
换句话说，所有数组中出现的所有值都应按其原始顺序包括在内，但最终数组中不得重复。  
去重后的数字应按其出现在参数中的原始顺序排序，最终数组不应按数字大小进行排序。  
```
function uniteUnique(arr) {
  //存储最终结果的数组
  let finalArr = [];
  //循环参数对象
  for(let i = 0; i < arguments.length; i++) {
    //把当前需要循环的数组存入
    var arrArg = arguments[i];
    //循环当前数组
    for(let j = 0; j < arrArg.length; j++) {
      var indexValue = arrArg[j];
      //检查当前值是否在最终结果的数组上
      if(finalArr.indexOf(indexValue) < 0) {
        finalArr.push(indexValue)
      }
    }
  }
  return finalArr;
}

uniteUnique([1, 3, 2], [5, 2, 1, 4], [2, 1]);
```
* 使用扩展符号
```
function uniteUnique(arr) {
  let args = [...arguments];
  let result = [];
  for(let i = 0; i < args.length; i++) {
    for(let j = 0; j < args[i].length; j++) {
       if(!result.includes(args[i][j])) {
        result.push(args[i][j]);
      }
    }
  }
  return result;
}

uniteUnique([1, 3, 2], [5, 2, 1, 4], [2, 1]);
```
```
function uniteUnique(arr) {
  let newArr;
  //把参数对象转换成数组
  let args = Array.prototype.slice.call(arguments);
  //对每组数组做判断
  newArr = args.reduce(function(arrA,arrB) {
    //删除arrB中重复的元素后与arrA连接
    return arrA.concat(arrB.filter(function(i) {
      return arrA.indexOf(i) === -1;
    }))
  })
  return newArr;
}

uniteUnique([1, 3, 2], [5, 2, 1, 4], [2, 1]);
```
* Set 对象
```
function uniteUnique(...arr) {
  //使用扩展符合并数组
  let newArr = [].concat(...arr);
  //使用ES6 Set对象，拥有数组去重的特性
  return [...new Set(newArr)];
}

uniteUnique([1, 3, 2], [5, 2, 1, 4], [2, 1]);
```
