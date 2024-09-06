## node-sass 安装到post-install卡死或抛错
npm config set sass-binary-site=https://nexus.intra/repository/nodejs/ --global
具体网址以实际用户国内外环境为准，electron等其它依赖原生库编译的包也是类似，指定编译后的镜像源地址即可跳过本地编译

## node18下构建工具构建结束但是不停止
webpack 插件
```
{
  apply: (compiler) => {
    compiler.hooks.done.tap('DonePlugin', (stats) => {
        setTimeout(() => {
          process.exit(0)
        })
    })
  }
}
```
rollup 插件
```
{
  closeBundle() {
    if (!process.env.ROLLUP_WATCH) {
      setTimeout(() => process.exit(0));
    }
  },
  name: 'force-close'
}
```
类似的问题还有使用高版本node构建vue2的时候卡死：https://github.com/vuejs/vue-loader/issues/1908
