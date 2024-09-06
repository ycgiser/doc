实际上不管是 vite 还是 webpack，proxy 插件都是基于的 http-proxy，因此核心配置都一样。
以 vite 为例：
```js
import { defineConfig } from "vite";

export default defineConfig({
  server: {
    base: '/chat-merchant/',
    port: 8871,
    proxy: {
      //比如本服务的publicPath是/chat-merchant，代理的基本策略可以是：将非/chat-merchant开头的pathname的请求打到xxx.test.net上
      '^/(?!chat-merchant)': {
        target: 'https://xxx.test.net',
        changeOrigin: true,
        autoRewrite: true,
        configure(proxy, options) {
          proxy.on('proxyReq', function(proxyReq, req, res, options) {
            //具体网关策略不同，比如某网关策略校验了未知Origin则拦截返回403，那可以像下面这样删除Origin
            proxyReq.removeHeader('Origin');
          });
        },
      }
    }
  }
})
```
