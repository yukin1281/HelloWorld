# vue-cli打包配置

## package.json配置

```javascript
"scripts": {
    "dev": "vue-cli-service serve",                         //启动开发环境
    "staging": "vue-cli-service serve --mode staging",      //启动测试环境
    "production": "vue-cli-service serve --mode production",//启动正式环境
    "build:prod": "vue-cli-service build",                  //生产环境打包
    "build:stage": "vue-cli-service build --mode staging",  //测试环境打包
    "preview": "node build/index.js --preview",
    "lint": "eslint --ext .js,.vue src"
},
```

运行命令：`npm run dev`  `npm run build:prod`  `npm run build:stage`

## vue.config.js配置

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  // 去掉console
  terser: {
    terserOptions: {
      compress: {
        drop_console: true,
        drop_debugger: true
      }
    }
  }
})


```

## devServer各个属性

webpack的devServer配置中，proxy用于配置代理服务器，使得开发环境中请求可以代理到其他服务器上。

proxy属性的所有属性含义如下：

- `target`：代理的目标服务器地址，可以是一个字符串或一个对象，字符串表示目标服务器的地址，对象可以包含以下属性：
  - `host`：目标服务器的主机名
  - `port`：目标服务器的端口号
  - `protocol`：目标服务器的协议，默认为http
- `changeOrigin`：是否改变请求头中的origin字段，默认为false，表示不改变
- `secure`：是否使用[https协议](https://so.csdn.net/so/search?q=https协议&spm=1001.2101.3001.7020)，默认为false
- `pathRewrite`：路径重写规则，可以是一个对象或一个函数，用于将请求的路径重写为代理服务器上的路径

- `bypass`：一个函数，用于决定哪些请求不需要被代理，返回true表示不需要代理，返回false表示需要代理
- `ws`：是否启用WebSocket代理，默认为false。
- `headers` : 可以用来设置请求头信息

```js
devServer: {
  proxy: {
    '/api': {
      ws:false,
      target: 'http://localhost:3000',
      changeOrigin: true,
      pathRewrite: {
        '^/api': ''
      },
       headers: {
          Accept: 'application/json'
       }
    }
  }
}
```

这个配置表示将所有以/api开头的请求代理到本地的3000端口上，同时将请求路径中的/api替换为空字符串。

## chainWebpack配置作用

chainWebpack 属性可以用来自定义 webpack 的配置。

在生产环境下去除 `console` 和 `debugger`

```js
module.exports = {
    chainWebpack: (config) => {
    config.optimization.minimizer('terser').tap((args) => {
      args[0].terserOptions.compress.drop_console = process.env.NODE_ENV === 'production';
      args[0].terserOptions.compress.drop_debugger = process.env.NODE_ENV === 'production';
      return args;
    });
  }
}
```

## 参考

https://blog.csdn.net/admans/article/details/131601438

https://cli.vuejs.org/zh/config/#vue-config-js











 
