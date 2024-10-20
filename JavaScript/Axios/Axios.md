# Axios



## 代码示例

```javascript
import common from "@/common";
import Axios from "axios";

class Api {
    constructor() {
        this.axios = Axios.create({});

        // 添加请求拦截器
        this.axios.interceptors.request.use(config => {
            // 自带session的部分，不加token
            if (config.withCredentials) {
                return config;
            }

            if (config.params === undefined) {
                config.params = {};
            }

            config.params.token = sessionStorage.getItem('token') || common.getParameter('token')
            return config;
        });

        // 添加响应拦截器
        this.axios.interceptors.response.use(response => {
            // TODO handle response.status 4XX 5XX, redirect to err page

            if (response.data.code === '0xffff') {
                console.error('code: ' + response.data.code + ', msg: ' + response.data.msg);
                return Promise.reject('Critical Error!');
            } else if (response.data.code !== '0x0000') {
                console.error('code: ' + response.data.code + ', msg: ' + response.data.msg);
                return Promise.reject(response.data.msg);
            }

            return Promise.resolve(response.data.data);
        });
    }

    static async all(requests, responses) {
        return Axios.all(requests).then(Axios.spread(responses))
    }
}

export default Api;
```





## Axios API



### Axios API

可以向 `axios` 传递相关配置来创建请求

**axios(config)**

```js
// 发起一个post请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
```



```js
// 在 node.js 用GET请求获取远程图片
axios({
  method: 'get',
  url: 'http://bit.ly/2mTM3nY',
  responseType: 'stream'
})
  .then(function (response) {
    response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
  });
```



**axios(url[, config])**

```js
// 发起一个 GET 请求 (默认请求方式)
axios('/user/12345');
```



**请求方式别名**

为了方便起见，已经为所有支持的请求方法提供了别名。

```js
axios.request(config)
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
axios.postForm(url[, data[, config]])
axios.putForm(url[, data[, config]])
axios.patchForm(url[, data[, config]])
```



**注意**
在使用别名方法时， url、method、data 这些属性都不必在配置中指定。



### Axios 实例

您可以使用自定义配置新建一个实例。

axios.create([config])

```js
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```





### 拦截器

在请求或响应被 then 或 catch 处理前拦截它们。

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```



### 响应结构

一个请求的响应包含以下信息。

```js
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 是服务器响应头
  // 所有的 header 名称都是小写，而且可以使用方括号语法访问
  // 例如: `response.headers['content-type']`
  headers: {},

  // `config` 是 `axios` 请求的配置信息
  config: {},

  // `request` 是生成此响应的请求
  // 在node.js中它是最后一个ClientRequest实例 (in redirects)，
  // 在浏览器中则是 XMLHttpRequest 实例
  request: {}
}
```

当使用 `then` 时，您将接收如下响应:

```js
axios.get('/user/12345')
  .then(function (response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });
```

当使用 `catch`，或者传递一个[rejection callback](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)作为 `then` 的第二个参数时，响应可以通过 `error` 对象被使用，正如在[错误处理](https://www.axios-http.cn/docs/handling_errors)部分解释的那样。
