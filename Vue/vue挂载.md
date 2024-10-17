# vue挂载

在Vue中，挂载是指将Vue实例连接到指定的HTML元素上，使其能够控制该元素及其子元素。在Vue中，挂载使用`mount`方法进行操作。

具体来说，以下是几种常见的挂载方式：



1. 使用`el`选项挂载：在Vue实例中通过`el`选项指定一个DOM元素的选择器，Vue会将该实例挂载到该元素上。示例代码如下：

   ```javascript
   new Vue({
     el: '#app'
   });
   ```

   上述代码会将Vue实例挂载到具有`id="app"`的DOM元素上。

2. 使用`mount`方法手动挂载：可以使用`mount`方法手动挂载Vue实例。示例代码如下：

   ```javascript
   const app = new Vue();
   app.$mount('#app');
   ```

   或者：

   ```javascript
   const app = new Vue();
   app.$mount();
   document.getElementById('app').appendChild(app.$el);
   ```

   上述代码会将Vue实例手动挂载到具有`id="app"`的DOM元素上。

3. 使用Vue组件挂载：可以将Vue组件挂载到其他Vue组件中。首先需要在父组件中导入子组件，然后在父组件的`components`选项中注册子组件。接着，在父组件的模板中使用子组件标签进行挂载。示例代码如下：

   ```javascript
   // 子组件
   const ChildComponent = Vue.component('child-component', {
     template: '<div>This is a child component</div>'
   });
   
   // 父组件
   new Vue({
     el: '#app',
     components: {
       'child-component': ChildComponent
     },
     template: '<div><child-component></child-component></div>'
   });
   ```

   上述代码会将子组件挂载到具有`id="app"`的DOM元素上。

以上是在Vue中常用的几种挂载方式，可以根据具体情况选择合适的方式来进行挂载。