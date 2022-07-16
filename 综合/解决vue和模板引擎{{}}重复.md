# 解决vue和模板引擎{{}}重复



- 解决方法：
  + `  delimiters: ['$$', '$$'],`
  + 可以在组件设置，也可以局部设置

```javascript
Vue.component('blog-post', {
            template: '<li>$$meg$$</li>',
            delimiters: ['$$', '$$'],
            data: function() {
                return {
                    meg: 'hhhhhhhh'
                }
            }
        });

        new Vue({
            el: '#blog',
            data: {
                mes: 'lhy',
            },
            delimiters: ['$$', '$$']
        })
```

```html
<div id="blog">
        <blog-post></blog-post>
        <p>$$ mes $$</p>
    </div>
```

