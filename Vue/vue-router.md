## vue-router的使用
### 0、安装vue-router
``` JavaScript
npm install -save vue-router
```

### 1、导入VueRouter
``` JavaScript
Vue.use(VueRouter)
```

### 2、定义路由组件
``` JavaScript
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }
```
也可以从其他文件 import 进来

### 3、定义路由
``` JavaScript
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]
```
这是定义的一个路由数组，每个路由地址映射一个组件。其中component可以是通过Vue.extend()创建的组件构建器，或者只是一个组件配置对象。

### 4、创建router实例，然后传入刚刚定义的路由数组
``` JavaScript
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})
```

### 5、挂载根实例
``` JavaScript
new Vue({
  router,
  render (h) {
    return h(App) 
  },
}).$mount('#app')
```

### 6、渲染组件
``` HTML
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <router-view></router-view>
</div>
```
可以使用 ```router-link``` 组件来导航，负责点击跳转。路由匹配到组件将被渲染到 ```router-view``` 。

### 嵌套路由
实际项目中的应用界面，通常是多层嵌套组件组合而成的。URL中的各段动态路径也按照某种结构对应嵌套组件。
``` JavaScript
const routes = [
  {
    path: '/login', component: Login,
  },
  {
    path: '/', component: VMain, redirect: '/home',
    children: [
      // 当 //foo 匹配成功，
      // Foo 会被渲染在 VMain 的 <router-view> 中
      { path: '/foo', component: Foo },
      // 当 /bar 匹配成功，
      // Bar 会被渲染在 VMain 的 <router-view> 中
      { path: '/bar', component: Bar }
    ]
  }
]
```