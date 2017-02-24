# sell

> A Vue.js project

# 声明
此作品为看完 Ustbhuangyi 老师的 Vue.js高仿饿了么外卖App 课程后，
独自用vue2.0实现，并深刻体会了vue2.0新的变化：

1. 用mounted 和 update 取代 watch 和 ready事件

2.所有需要 css 动画的元素都必须用 transition标签包装

3.所有需要获取子组件或dom结构，都必须在需要获取的元素上面用 ref＝"defineName"命名,
并用 Vue.$ref.defineName 来获取元素

4.平行组件相互通讯 （A,B为平行组件,C为a,b共同父级组件）
1). a组件在事件中添加 Vue.$emit('Afun', data); A组件方法 Afun;
2). c组件 <A @Afun = "Cfun"></A> 在A组件上绑定事件来执行C组件方法 Cfun
3). c组件方法  Cfun(data): Vue.$ref.Bcomponent.Bfun(data)  

即可把A组件获取的data传入到B组件 Bfun中 以此来取代vue1.0中的 $dispatch 派发事件





## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
