```javascript

            const name = Symbol('name');
            const name2 = Symbol('name');
            var bool = (name === name2); //false
            output(bool, 'output');

            let love = Symbol('love');
            let age = Symbol('age');
            let lhy = {
                // 要加中括号
                [age]: 14
            };
            // 不可以用 . 表示对象的属性，要用 [] 表示
            // symbol 不可以枚举 就是不能用for in输出 没有key  否则没有输出
            lhy[love] = "xzy";
            console.log(lhy);
```