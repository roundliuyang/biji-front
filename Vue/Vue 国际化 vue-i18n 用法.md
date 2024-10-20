# Vue 国际化 vue-i18n 用法



在 [Vue 使用 vue-i18n 国际化 - 左小白的技术日常](http://www.zuo11.com/blog/2021/4/vue_international.html) 中，我们简单介绍了 vue-i18n 的基本使用。如果想将它实际应用到项目中，我们还需要考虑怎么做到更加简洁、优雅、可维护，下面是一些实践总结。



## 1. i18n 单独放一个目录，避免在 main.js 中写入太多内容

i18n 相关内容较多，如果都写在 main.js 里，内容会很多，不够优雅，这里可以进行简单封装。在 src 目录下新建一个 i18n 目录，专门用于存放国际化相关内容，使用 index.js 作为入口文件

```js
// src/i18n/index.js
import Vue from "vue";
import VueI18n from "vue-i18n";

Vue.use(VueI18n);

const i18n = new VueI18n({
  locale: "zh-CN", // 设置默认语言环境
  // 在 vue template 的 {{}} 中，使用 $t('un') 即可拿到 un 属性指定的值
  // 不需要在 data() {} 中设置什么
  messages: {
    en: {
      name: "Zhang san",
      hello: "hello world"
    },
    "zh-CN": {
      name: "张三",
      hello: "你好，世界"
    }
  }
});

export default i18n; // 将 i18n 实例导出，用于在 new Vue 时引入
```

这样，我们在 main.js 就只需要修改两行就可以了

```js
// main.js
// ...
import i18n from "./i18n/index"; // 引入 src/i18n/index，得到 i18n 实例

new Vue({
  i18n, // 在 new Vue() 时加入 i18n
  router,
  store,
  render: h => h(App)
}).$mount("#app");
```







## 引用

http://www.zuo11.com/blog/2021/5/vue-i18n_use.html