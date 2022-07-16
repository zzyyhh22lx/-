# 关于Vue组件名大小写的一些问题


全局组件在进行注册时，对组件名的大小写是没有进行任何限制的，大小写Aa下划线_短线-甚至是中文“啊”都可以进行注册。当然中文不能做为第一个字符，也不建议在组件名中使用中文（测试浏览器：谷歌、火狐）

```html
<div id="app">
	<a新组件></a新组件>
</div>

Vue.component("a新组件", {
    template: "<h1>vue组件名不能大写吗</h1>"
});

```

说回正题，当我初次进行组件注册时，用的是标准的驼峰命名法，使用时也是复制的组件名，但是浏览器报错了，提示注册的组件不正确。

```html
<div id="app">
	<newComponent></newComponent>
</div>

Vue.component("newComponent", {
    template: "<h1>vue组件名不能大写吗</h1>"
});

```

于是我复制了官网文档上的组件名粘贴到自己的页面后，显示是正常无误的，便怀疑是否是因为组件命名不正确，上网一查果然是，使用时组件名需在大写前加上-符号，并且大写转为小写。
但在接下来在我的试验过程中发现网上的说法并不准确。
首先，组件名首字母大写是不会对使用产生影响的

```html
<div id="app">
	<Item></Item>
</div>

Vue.component("Item", {
    template: "<h1>vue组件名不能大写吗</h1>"
});

```

其次，组件名注册时中间夹杂大写，在使用时必须在大写前加上-符号，但是并不需要将大写转为小写，也就是说组件名在使用时不用区分大小写

```html
<div id="app">
	<New-Component></New-Component>
</div>

Vue.component("NewComponent", {
    template: "<h1>vue组件名不能大写吗</h1>"
});

```

最后，组件的闭合标签是不需要加-符号，也不用区分大小写就可以被识别的。浏览器不会报错，但是对该组件后面的其他组件会造成影响，导致后面的组件无法渲染，所以慎用

```html
<div id="app">
	<Qw-E-Rr></QwERr>
</div>

Vue.component("QwERr", {
    template: "<h1>vue组件名不能大写吗</h1>"
});

```

尽量全部用小写方便维护