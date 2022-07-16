# MVVM

实现：

```js
1.实现一个监听器Observer

数据监听器的核心方法就是Object.defineProperty(

)，通过遍历循环对所有属性值进行监听，并对其进行Object.defineProperty( )处理，那么代码可以这样写：

//对所有属性都要蒋婷,递归遍历所有属性

function defineReactive(data,key,val) {

    observe(val);  //递归遍历所有的属性

    Object.defineProperty(data,key,{

        enumerable:true,         //当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。

        configurable:true,       //当且仅当该属性的enumerable为true时，该属性才能够出现在对象的枚举属性中

        get:function() {

            returnval;

        },

        set:function(newVal) {

            val = newVal;

            console.log('属性'+key+'已经被监听,现在值为:"'+newVal.toString()+'"');

        }

    })

}



function observe(data) {

    if(!data || typeof data !== 'object'){

        return;

    }

    Object.keys(data).forEach(function(key){

        defineReactive(data,key,data[key]);

    });

}



varlibrary = {

    book1: {

        name:''

    },

    book2:''

};

observe(library);

library.book1.name = 'vue权威指南'; // 属性name已经被监听了，现在值为：“vue权威指南”

library.book2 = '没有此书籍';  // 属性book2已经被监听了，现在值为：“没有此书籍”

通过observe()方法进行遍历向下找到所有的属性，并通过defineReactive()方法进行数据劫持监听．

在上面的思路中，我们需要一个可以容纳消息订阅者的消息订阅器Dep，订阅器主要收集消息订阅者，然后在属性变化时执行相应订阅者的更新函数，那么消息订阅器Dep需要有一个容器，用来存放消息订阅者．我们将上面的监听器Observer稍微修改一下：

function defineReactive(data,key,val) {

    observe(val);

    var dep = newDep();

    Object.defineProperty(data, key, {

        enumerable:true,

        configurable:true,

        get: function() {

            if (是否需要添加订阅者) {    //Watcher初始化触发

                dep.addSub(watcher);// 在这里添加一个订阅者

            }

            returnval;

        },

        set: function(newVal) {

            if(val === newVal) {

                return;

            }

            val = newVal;

            console.log('属性' + key + '已经被监听了，现在值为：“' + newVal.toString()

+ '”');

            dep.notify();// 如果数据变化，通知所有订阅者

        }

    });

}



function observe(data) {

    if(!data || typeof data !== 'object'){

        return;

    }

    Object.keys(data).forEach(function(key){

        defineReactive(data,key,data[key]);

    });

}



function Dep() {

    this.subs = [];

}



//prototype 属性使您有能力向对象添加属性和方法

//prototype这个属性只有函数对象才有，具体的说就是构造函数具有.只要你声明定义了一个函数对象，这个prototype就会存在

//对象实例是没有这个属性

Dep.prototype ={                       

    addSub:function(sub) {

        this.subs.push(sub);

    },

    notify:function() {

        this.subs.forEach(function(sub){

            sub.update();        //通知每个订阅者检查更新

        })

    }

}

Dep.target = null;

在代码中，我们将订阅器Dep添加一个订阅者设计在get里面，这是为了让Watcher在初始化时触发，因此判断是否需要需要添加订阅者，至于具体实现的方法，我们在下文中深究．在set方法中，如果函数变化，就会通知所有的订阅者，订阅者们将会执行相对应的更新函数，到目前为止，一个比较完善的Observer已经成型了，下面我们要写订阅者Watcher.

２．实现订阅者Watcher

根据我们的思路，订阅者Wahcher在初始化时要将自己添加到订阅器Dep中，那么如何进行添加呢？

我们已经知道监听器Observer是在get函数中执行了添加订阅者的操作的，所以我们只需要在订阅者Watcher在初始化时触发相对应的get函数来执行添加订阅者的操作即可．那么怎么触发对应的get函数呢？我们只需要获取对应的属性值，就可以通过Object.defineProperty( )触发对应的get了．

在这里需要注意一个细节，我们只需要在订阅者初始化时才执行添加订阅者，所以我们需要一个判断，在Dep.target上缓存一下订阅者，添加成功后去除就行了，代码如下：

functionWatcher(vm,exp,cb) {

    this.vm = vm;    //指向SelfVue的作用域

    this.exp = exp;  //绑定属性的key值

    this.cb = cb;    //闭包

    this.value = this.get();

}



Watcher.prototype = {

    update:function() {

        this.run();

    },

    run:function() {

        var value = this.vm.data[this.exp];

        var oldVal = this.value;

        if(value !== oldVal) {

            this.value = value;

            this.cb.call(this.vm,value,oldVal);

        }

    },

    get:function() {

        Dep.target =this;                   // 缓存自己

        var value = this.vm.data[this.exp];  // 强制执行监听器里的get函数

        Dep.target =null;                   // 释放自己

        returnvalue;

    }

}

这个时候我们需要对监听器Observer中的defineReactive()做稍微的调整：

functiondefineReactive(data,key,val) {

    observe(val);

    var dep = newDep();

    Object.defineProperty(data, key, {

        enumerable:true,

        configurable:true,

        get: function() {

            if(Dep.target) {   //判断是否需要添加订阅者

                 dep.addSub(Dep.target);

            }

            returnval;

        },

        set: function(newVal) {

            if(val === newVal) {

                return;

            }

            val = newVal;

            console.log('属性' + key + '已经被监听了，现在值为：“' + newVal.toString()

+ '”');

            dep.notify();// 如果数据变化，通知所有订阅者

        }

    });

}

到目前为止，一个简易版的Watcher已经成型了，我们只需要将订阅者Watcher和监听器Observer关联起来，就可以实现一个简单的双向绑定．因为这里还没有设计指令解析器，所以对于模板数据我们都进行写死处理，假设模板上有一个节点元素，且id为＇name＇,并且双向绑定的绑定变量也是'name'，且是通过两个大双括号包起来（暂时没有什么用处），模板代码如下：

   

{{name}}
我们需要定义一个SelfVue类，来实现observer和watcher的关联，代码如下：

//将Observer和Watcher关联起来

functionSelfVue(data,el,exp) {

    this.data = data;

    observe(data);

    el.innerHTML =this.data[exp];

    new Watcher(this,exp,function(value){

        el.innerHTML = value;

    });

    return this;

}

然后在页面上new一个SelfVue，就可以实现双向绑定了：

   





     var ele =

document.querySelector('#name');

     var selfVue = newSelfVue({

         name:'hello world'

     },ele,'name');



     window.setTimeout(function() {

         console.log('name值改变了');

         selfVue.name ='byebye world';

     },2000);

这时我们打开页面，显示的是'hello

world',２s后变成了'byebye world',一个简单的双向绑定实现了．

对比vue,我们发现了有一个问题，我们在为属性赋值的时候形式是： '  selfVue.data.name = 'byebye world'  ',而我们理想的形式是：'

 selfVue.name = 'byebye world'  '，那么怎么实现这种形式呢，只需要在new SelfVue时做一个代理处理，让访问SelfVue的属性代理为访问selfVue.data的属性，原理还是使用Object.defineProperty( )对属性在包装一层．代码如下：

functionSelfVue(data,el,exp) {

    var self = this;

    this.data = data;

    //Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组

    Object.keys(data).forEach(function(key) {

        self.proxyKeys(key);     //绑定代理属性

    });

    observe(data);

    el.innerHTML =this.data[exp];   // 初始化模板数据的值

    new Watcher(this,exp,function(value){

        el.innerHTML = value;

    });

    return this;

}



SelfVue.prototype = {

    proxyKeys:function(key) {

        var self = this;

        Object.defineProperty(this,key,{

            enumerable:false,

            configurable:true,

            get:functionproxyGetter() {

                returnself.data[key];

            },

            set:functionproxySetter(newVal) {

                self.data[key] = newVal;

            }

        });

    }

}

这样我们就可以用理想的形式改变模板数据了．

３．实现指令解析器Compile

再上面的双向绑定demo中，我们发现整个过程都没有解析dom节点，而是固定某个节点进行替换数据，所以接下来我们要实现一个解析器Compile来解析和绑定工作，分析解析器的作用，实现步骤如下：

1.解析模板指令，并替换模板数据，初始化视图



2.将模板指令对应的节点绑定对应的更新函数，初始化相应的订阅器

为了解析模板，首先要获得dom元素，然后对含有dom元素上含有指令的节点进行处理，这个过程对dom元素的操作比较繁琐，所以我们可以先建一个fragment片段，将需要解析的dom元素存到fragment片段中在做处理：

nodeToFragment:function(el){

        varfragment =document.createDocumentFragment();   //createdocumentfragment()方法创建了一虚拟的节点对象，节点对象包含所有属性和方法。

        varchild =el.firstChild;

        while(child) {

            // 将Dom元素移入fragment中

            fragment.appendChild(child);

            child = el.firstChild;

        }

        returnfragment;

    }

接下来需要遍历所有节点，对含有指令的节点进行特殊的处理，这里我们先处理最简单的情况，只对带有 '{{变量}}' 这种形式的指令进行处理，代码如下：

//遍历各个节点,对含有相关指定的节点进行特殊处理

    compileElement:function(el) {

        varchildNodes =el.childNodes;   //childNodes属性返回节点的子节点集合，以 NodeList 对象。

        var self = this;

        //slice() 方法可从已有的数组中返回选定的元素。

       [].slice.call(childNodes).forEach(function(node) {

            varreg =/\{\{(.*)\}\}/;

            vartext =node.textContent;  //textContent 属性设置或返回指定节点的文本内容

            if(self.isTextNode(node)&& reg.test(text)) {      //判断是否符合{{}}的指令

                //exec()方法用于检索字符串中的正则表达式的匹配。

                //返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。

               self.compileText(node,reg.exec(text)[1]);

            }

            if(node.childNodes&& node.childNodes.length) {

                self.compileElement(node);    //继续递归遍历子节点

            }

        });

    },

    compileText:function(node,exp) {

        var self = this;

        var initText = this.vm[exp];

        this.updateText(node,initText);    // 将初始化的数据初始化到视图中

        new Watcher(this.vm,exp,function(value){

            self.updateText(node,value);

        });



    },

    updateText:function(node,value) {

        node.textContent =typeof value == 'undefined' ?'': value;

    },

获取到最外层节点后，调用compileElement函数，对所有子节点进行判断，如果节点是文本节点且匹配{{}}这种形式指令的节点就开始进行编译处理，编译处理首先需要初始化视图数据，对应上面所说的步骤1，接下去需要生成一个并绑定更新函数的订阅器，对应上面所说的步骤2。这样就完成指令的解析、初始化、编译三个过程，一个解析器Compile也就可以正常的工作了。

为了将解析器Compile与监听器Observer和订阅者Watcher关联起来，我们需要再修改一下类SelfVue函数：

functionSelfVue(options) {

    var self = this;

    this.vm = this;

    this.data = options.data;

    Object.keys(this.data).forEach(function(key){

        self.proxyKeys(key);     //绑定代理属性

    });

    observe(options.data);

    new Compile(options.el,this.vm);

    return this;

}

更改后，我们就不要像之前通过传入固定的元素值进行双向绑定了，可以随便命名各种变量进行双向绑定了：

   

       

{{title}}
       

{{name}}
       

{{content}}






    var selfVue = newSelfVue({

        el:'#app',

        data:{

            title:'aaa',

            name:'bbb',

            content:'ccc'

        }

    });

    window.setTimeout(function() {

        selfVue.title ='ddd';

        selfVue.name ='eee';

        selfVue.content ='fff'

    },2000);

到这里，一个数据双向绑定功能已经基本完成了，接下去就是需要完善更多指令的解析编译，在哪里进行更多指令的处理呢？答案很明显，只要在上文说的compileElement函数加上对其他指令节点进行判断，然后遍历其所有属性，看是否有匹配的指令的属性，如果有的话，就对其进行解析编译。这里我们再添加一个v-model指令和事件指令的解析编译，对于这些节点我们使用函数compile进行解析处理：

compile:function(node){

        varnodeAttrs =node.attributes;   //attributes 属性返回指定节点的属性集合，即 NamedNodeMap。

        var self = this;

        //Array.prototype属性表示Array构造函数的原型，并允许为所有Array对象添加新的属性和方法。

        //Array.prototype本身就是一个Array

       Array.prototype.forEach.call(nodeAttrs,function(attr) {

            varattrName =attr.name;      //添加事件的方法名和前缀:v-on:click="onClick" ,则attrName = 'v-on:click'

id="app" attrname= 'id'

            if(self.isDirective(attrName)){    

                varexp =attr.value;      //添加事件的方法名和前缀:v-on:click="onClick" ,exp = 'onClick'



                //substring()方法用于提取字符串中介于两个指定下标之间的字符。返回值为一个新的字符串

                //dir = 'on:click'

                var dir =

attrName.substring(2); 

                if(self.isEventDirective(dir)){   //事件指令

                   self.compileEvent(node,self.vm,exp,dir);

                }else{          //v-model指令

                     self.compileModel(node,self.vm,exp,dir);

                }



                node.removeAttribute(attrName);

            }

        });

    }

上面的compile函数是挂载Compile原型上的，它首先遍历所有节点属性，然后再判断属性是否是指令属性，如果是的话再区分是哪种指令，再进行相应的处理.

最后我们再次改造一下SelfVue，是它的格式看上去更像vue:

functionSelfVue(options) {

    var self = this;

    this.data = options.data;

    this.methods =options.methods;

    Object.keys(this.data).forEach(function(key){

        self.proxyKeys(key);   

    });

    observe(options.data);

    new Compile(options.el,this);

    options.mounted.call(this);

}

测试一下：

   

           

{{title}}
           

           

{{name}}
            clickme!





    newSelfVue({

        el:'#app',

        data: {

            title:'hello world',

            name:'canfoo'

        },

        methods: {

            clickMe: function () {

                this.title = 'hello world';

            }

        },

        mounted: function () {

            window.setTimeout(() => {

                this.title = '你好';

            },1000);

        }

    });
```

