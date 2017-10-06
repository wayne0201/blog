# vue-router分析![](https://github.com/lj614418910/blog/blob/master/images/vueRouter.png)

## vue-router的使用
- 对于开发者来说使用vue-router主要是有两个方面。
	- 配置：路由和组件的匹配，主要是对url和当时`<router-view/>`应该渲染哪个组件的配置。
	- 跳转：通过`<router-link/>`或者通过调用router里的方法进行路由的变更，进而重新渲染视图。

### 实际使用

``` html
<div id="app">
  ...
  <router-view></router-view>
  ...
</div>
```

``` javascript
import VueRouter from 'vue-router'
var router = new VueRouter({
  routes: [
   {path: '/foo', component: {template: '<div>FOO</div>'}},
   {path: '/bar', component: {template: '<div>BAR</div>'}}
  ]
})

var vm = new Vue({
  el: '#app',
  router: router
})

```

- 上面分别是从入口js和根组件之中的写法。
	- **配置:**
	- 首先我们引入vue-router,然后将它实例化。实例化的时候要给它传一个对象，对象里有个routes的数组，里面是你配置的多个路由的信息，包括当跳转到当前路由时，挂载哪个子组件和一些在这个路由时的其他配置信息。
	- 实例化后的router对象，将它传到new Vue()参数对象中的router属性中去。
	- 然后在根组件中引入`<router-view/>`，然后基于 router 中的配置，`<router-view/>`根据页面url地址显示不同的内容。
	- **跳转:**
	- 因为只要url发生改变，vue就会重新去渲染<router-view/>组件，所以我们要做的就是去修改url地址，修改url有很多方法，但是vue-router给我们提供了两种比较适用的方式。
	- **第一种:组件式** `<router-link>`组件支持用户在具有路由功能的应用中（点击）导航。 通过`to`属性指定目标地址，默认渲染成带有正确链接的`<a>`标签，可以通过配置 tag 属性生成别的标签。
		-  `<router-link :to="...">`
		-  `<router-link :to="..." replace>`
	- **第二种:编程式导航** 除了使用`<router-link>`创建`a`标签来定义导航链接，我们还可以借助`router`的实例方法，通过编写代码来实现。推荐使用这种方式，更自由，更优雅。
		- `router.push(location)`:这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。
		- `router.replace(location)`:跟`router.push`很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。
		- `router.go(n)`:这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步,如果 history 记录不够用，那就默默地失败呗。
- 只要懂得如何**配置路由**和**路由跳转**就可以利用vue-router进行初步的开发了，可以在项目需要的时候适当添加其它的拓展性功能。	



