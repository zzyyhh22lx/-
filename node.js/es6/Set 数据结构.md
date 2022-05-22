<h1 align="center">Set 数据结构</h1>

```javascript
// Set 构造函数，用来创造set数据结构*
//没有初始化值的话,  返回	Set(0){}

const a = new Set([1, 2, 3, 1, 2]);

console.log(a); //Set(3) { 1, 2, 3 }*

console.log(a.size); // 3  获取a的长度,size相当于length*



// 把set 变成数组

let arr = [...a];

console.log(arr);

//往set数据结构 传入初始值，会把重复的值过滤掉*
```



#### 实例方法

- **add(value)： 添加某个值，返回set结构本身**；

- **delete(value)： 删除某个值，返回一个布尔值，表示删除是否成功；**
- **has(value)： 返回一个布尔值，表示该值是否为Set的成员**；
- **clear()： 清除所有成员，没有返回值。**



```javascript
const s=new Set();
//set可以链式调用
s.add(1).add(2);

s.delete(2);//如果括号里面的值Set里面不存在会返回false
s.has(1)//true
//删除全部
s.clear();
```



**Set是类数组，不过也有forEach遍历**

```javascript
const arr=new Set([1,2,3]);
arr.forEach((element, index) => console.log(element)) //1 2 3
```

