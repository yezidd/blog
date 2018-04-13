---
layout: article
title: vue-cli中webpack配置详解
date: 2018-04-10 15:33:21
tags: webpack vue
---

#### webpack配置

> 先来观察webpack.base.conf.js

```
    module.exports = {
      context: path.resolve(__dirname, '../'),
      entry: {
        app: './src/main.js'
      },
      output: {
        path: config.build.assetsRoot,
        filename: '[name].js',
        publicPath: process.env.NODE_ENV === 'production'
          ? config.build.assetsPublicPath
          : config.dev.assetsPublicPath
      },
      resolve: {
        extensions: ['.js', '.vue', '.json'],
        alias: {
          'vue$': 'vue/dist/vue.esm.js',
          '@': resolve('src'),
        }
      },
      module: {
        rules: [
          {
            test: /\.vue$/,
            loader: 'vue-loader',
            options: vueLoaderConfig
          },
          {
            test: /\.js$/,
            loader: 'babel-loader',
            include: [resolve('src'), resolve('test'), resolve('node_modules/webpack-dev-server/client')]
          },
          {
            test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
            loader: 'url-loader',
            options: {
              limit: 10000,
              name: utils.assetsPath('img/[name].[hash:7].[ext]')
            }
          },
          {
            test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
            loader: 'url-loader',
            options: {
              limit: 10000,
              name: utils.assetsPath('media/[name].[hash:7].[ext]')
            }
          },
          {
            test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
            loader: 'url-loader',
            options: {
              limit: 10000,
              name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
            }
          }
        ]
      },
      node: {
        // prevent webpack from injecting useless setImmediate polyfill because Vue
        // source contains it (although only uses it if it's native).
        setImmediate: false,
        // prevent webpack from injecting mocks to Node native modules
        // that does not make sense for the client
        dgram: 'empty',
        fs: 'empty',
        net: 'empty',
        tls: 'empty',
        child_process: 'empty'
      }
    }
```

> 目录结构 ![vue-cli](/image/1.jpg)

> context字段定义了入口中所使用的根目录 为当前文件夹的父目录

> entry 定义了入口文件为项目根目录的`src/main.js`

> output属性定义了三个属性

```
    {
        path为输出bundle文件的输出路径,查看config文件夹下面的index.js
        找到build对象中的assetsRoot=>path.resolve(__dirname, '../dist')
        所以知道输出到dist文件夹下面,注意这个output目录必须是一个绝对路径

        filename:[name].js 就是对于打包的js使用其本身的文件名字

        publicPath:查看config文件看到都为'/',以 runtime(运行时) 或 loader(载入时) 所创建的每个 URL 为前缀
        意思就是为每一个index.html文件里面的资源文件设置一个前缀，如果没有特殊需求，一般就是'/'根目录
    }
```

> resolve为webpack的解析模块，为解析各种资源定义各种的规则

```
    {
        alias 创建 import 或 require 的别名，来确保模块引入变得更简单。
        例如上面的`'vue$': 'vue/dist/vue.esm.js'`,则我们在项目中import 'vue$'则引用到了
        'vue依赖中的vue.esm.js'文件

        extensions 自动解析确定的扩展.定义了[js,json,vue]这样我们在引入这些文件的时候无需
        申明文件的后缀

    }
```

> 之后的module则是对引入的资源文件进行解析，使用各种loader

> ##### vue-loader [点击这里](https://vue-loader.vuejs.org/zh-cn/configurations/pre-processors.html)

> 用于将Vue 组件转换为 JavaScript 模块

> 查看代码块，可以看到vue-loader的配置文件在`vue-loader.conf.js`文件夹里面

```
    module.exports = {
      loaders: utils.cssLoaders({
        sourceMap: sourceMapEnabled,
        extract: isProduction
      }),
      cssSourceMap: sourceMapEnabled,
      cacheBusting: config.dev.cacheBusting,
      transformToRequire: {
        video: ['src', 'poster'],
        source: 'src',
        img: 'src',
        image: 'xlink:href'
      }
    }
```

> 导出这么一个对象用于vue-loader的配置

> loader属性 -> 用于处理指定 webpack loader 对象覆盖用于 *.vue 文件内的语言块的默认 loader。如果指定，该键对应于语言块的 lang 属性。

> 每种类型默认的lang是:
```
{
    <template>: html
    <script>: js
    <style>: css
}
```
> 查看代码是封装进了一个utils导出的cssLoaders方法,查看utils文件

```
    exports.cssLoaders = function (options) {
      options = options || {}

      const cssLoader = {
        loader: 'css-loader',
        options: {
          sourceMap: options.sourceMap
        }
      }

      const postcssLoader = {
        loader: 'postcss-loader',
        options: {
          sourceMap: options.sourceMap
        }
      }

      // generate loader string to be used with extract text plugin
      function generateLoaders (loader, loaderOptions) {
        const loaders = options.usePostCSS ? [cssLoader, postcssLoader] : [cssLoader]

        if (loader) {
          loaders.push({
            loader: loader + '-loader',
            options: Object.assign({}, loaderOptions, {
              sourceMap: options.sourceMap
            })
          })
        }

        // Extract CSS when that option is specified
        // (which is the case during production build)
        if (options.extract) {
          return ExtractTextPlugin.extract({
            use: loaders,
            fallback: 'vue-style-loader'
          })
        } else {
          return ['vue-style-loader'].concat(loaders)
        }
      }

      // https://vue-loader.vuejs.org/en/configurations/extract-css.html
      return {
        css: generateLoaders(),
        postcss: generateLoaders(),
        less: generateLoaders('less'),
        sass: generateLoaders('sass', { indentedSyntax: true }),
        scss: generateLoaders('sass'),
        stylus: generateLoaders('stylus'),
        styl: generateLoaders('stylus')
      }
    }
```

> 首先这个函数返回了css,postcss,less,sass,scss,stylus,styl lang类型决定使用的loader

> 查看最终的数据可以看出函数执行情况

> 最后处理的loader都将是vue-style-loader

> 所以可以看出是 经过各种loader处理vue中css文件然后统一交给vue-style-loader处理

> cssSourceMap 属性 打包是否支持source-map，开发环境下加快打包速度不开启，生产环境开启，为了寻找问题源代码

> cacheBusting 属性 是否支持缓存

> transformToRequire属性 在模版编译过程中，编译器可以将某些属性，如 src 路径，转换为 require 调用，以便目标资源可以由 webpack 处理。默认配置会转换 <img> 标签上的 src 属性和 SVG 的 <image> 标签上的 xlink：href 属性。

> 看base.config中处理外部资源的url-loader

> 这个是设置limit为一个阈值，如果大于则使用file-loader产生一个require路径,如果小于这个阈值则转化为base64二进制文件

> #### node属性 [点击这里](https://doc.webpack-china.org/configuration/node/)

> 设置一个pollyfill让浏览器使用上node的一些全局模块

> 针对一些node api设置为 false则不使用,mock则供 mock 实现预期接口，但功能很少或没有,empty则提供空对象,true则实现pollyfill

```
{
  setImmediate:false
  dgram: 'empty',
  fs: 'empty',
  net: 'empty',
  tls: 'empty',
  __filename:true,
  child_process: 'empty'
}
```

> 讲完这个base.config.js然后我们按照开发环境和生产环境来分别讲一下两者的代码流程走向

#### 开发环境(webpack-dev-server --inline --progress --config build/webpack.dev.conf.js)

> webpack.dev.config.js 为webpack的入口

```
    module.exports = new Promise((resolve, reject) => {
      portfinder.basePort = process.env.PORT || config.dev.port;

      const notifier = require('node-notifier')

      notifier.notify({
        title: 'My notification',
        message: 'Hello, there!'
      });

      portfinder.getPort((err, port) => {
        if (err) {
          reject(err)
        } else {
          console.log("------", port);
          // publish the new Port, necessary for e2e tests
          process.env.PORT = port;
          // add port to devServer config
          devWebpackConfig.devServer.port = port;

          // Add FriendlyErrorsPlugin
          devWebpackConfig.plugins.push(new FriendlyErrorsPlugin({
            compilationSuccessInfo: {
              messages: [`Your application is running here: http://${devWebpackConfig.devServer.host}:${port}`],
            },
            onErrors: config.dev.notifyOnErrors
              ? utils.createNotifierCallback()
              : undefined
          }));
          console.log(devWebpackConfig.module.rules);
          resolve(devWebpackConfig)
        }
      })
    })
```

> 最终导出的webpack配置对象,为一个promise对象，webpack支持promise对象，所以会等待这个promise对象完成

> [node-portfinder](https://github.com/indexzero/node-portfinder)是 一个寻找系统开放端口的一个库

> 设置portfiner基础端口为环境变量中设置的端口或者是config文件中设置的端口,此处为8080

> [node-notifier](https://github.com/mikaelbr/node-notifier)是 一个unix系统上实现通知的一个库

> portfinder.getPort((err,port)=>{});

> 获得端口之后各处赋值

> 然后获取原先配置好的 dev配置对象在最后插件数组中push一个[FriendlyErrorsPlugin](https://github.com/geowarin/friendly-errors-webpack-plugin)插件的使用

> 即可获得npm run dev 实现的各种效果

> 如果报错，那么配置了notyfiOnErrors则会出现一个通知

> 继续看dev环境下的webpack配置

```
    const devWebpackConfig = merge(baseWebpackConfig, {
      module: {
        rules: utils.styleLoaders({sourceMap: config.dev.cssSourceMap, usePostCSS: true})
      },
      // cheap-module-eval-source-map is faster for development
      devtool: config.dev.devtool,

      // these devServer options should be customized in /config/index.js
      devServer: {
        clientLogLevel: 'warning',
        historyApiFallback: {
          rewrites: [
            {from: /.*/, to: path.posix.join(config.dev.assetsPublicPath, 'index.html')},
          ],
        },
        hot: true,
        contentBase: false, // since we use CopyWebpackPlugin.
        compress: true,
        host: HOST || config.dev.host,
        port: PORT || config.dev.port,
        open: config.dev.autoOpenBrowser,
        overlay: config.dev.errorOverlay
          ? {warnings: false, errors: true}
          : false,
        publicPath: config.dev.assetsPublicPath,
        proxy: config.dev.proxyTable,
        quiet: true, // necessary for FriendlyErrorsPlugin
        watchOptions: {
          poll: config.dev.poll,
        }
      },
      plugins: [
        new webpack.DefinePlugin({
          'process.env': require('../config/dev.env')
        }),
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NamedModulesPlugin(), // HMR shows correct file names in console on update.
        new webpack.NoEmitOnErrorsPlugin(),
        // https://github.com/ampedandwired/html-webpack-plugin
        new HtmlWebpackPlugin({
          filename: 'index.html',
          template: 'index.html',
          inject: true
        }),
        // copy custom static assets
        new CopyWebpackPlugin([
          {
            from: path.resolve(__dirname, '../static'),
            to: config.dev.assetsSubDirectory,
            ignore: ['.*']
          }
        ])
      ]
    })
```

> 通过官方的一个webpack-merge插件将base.config和dev.config结合在一起

> 通过merge增加了一个规则是`utils.styleLoaders({sourceMap: config.dev.cssSourceMap, usePostCSS: true})`

> 查看utils源码

```
    exports.styleLoaders = function (options) {
      const output = []
      const loaders = exports.cssLoaders(options)

      for (const extension in loaders) {
        const loader = loaders[extension]
        output.push({
          test: new RegExp('\\.' + extension + '$'),
          use: loader
        })
      }

      return output
    }
```

> 可知道是返回一个之前导出的cssLoaders方法返回的css，postcss...等外部配置loader

> #### [devtool](https://doc.webpack-china.org/configuration/devtool/)

> 此选项控制是否生成，以及如何生成 source map

> 具体可以看webpack官网 和github实例

> #### [devServer](https://doc.webpack-china.org/configuration/dev-server/#devserver)

> clientLogLevel 是客户端log的等级(当使用内联模式(inline mode)时，在开发工具(DevTools)的控制台(console)将显示消息)

> 四个等级['none','waring','error','info']

> historyApiFallback 使用html5 history Api时对路径进行重写

> hot是否开启热更新,搭配HotModuleReplacementPlugin插件使用

> contentBase 告诉服务器从哪里提供内容。只有在你想要提供静态文件时才需要。devServer.publicPath 将用于确定应该从哪里提供 bundle，并且此选项优先。

> 设置为false则禁用contentBase,由于使用了CopyWebpackPlugin将静态资源文件拷贝到相对于publicPath目录的static目录，所以暂时不需要设置contentBase

> compress 是否启用gzip压缩

> overlay 是否开启一个全屏的错误提醒,提供一个对象或者时boolean,默认不开启

> publicPath 设置打包资源的根路径

> proxy 设置代理转发，实现跨域访问.[针对请求的api,实现请求转发](https://doc.webpack-china.org/configuration/dev-server/#devserver-proxy)

> quiet 设置为true则所有的信息都不输出在控制台上

> watchOptions webpack 使用文件系统(file system)获取文件改动的通知。在某些情况下，不会正常工作。例如，当使用 Network File System (NFS) 时。

> poll 属性控制是否开启轮询，如果输入一个整数，则为时间间隔

> 再看插件环节，由于开发环境为了追求打包速度所以没有过多对于体积的要求

#### 生产环境(node build/build.js)

