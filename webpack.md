## webpack 配置

### 1.全局安装( 准备 )

cnpm install webpack webpack-cli -g

### 2.初始化项目( 正式开始步骤 )

cnpm init 

- 局部安装一下(开发环境) 		cnpm install webpack webpack-cli --save-dev

### 3.手动创建入口

根目录下新建src文件夹，在内新建index.js		=>执行webpack命令，进行打包

### 4.配置入口出口文件（新建webpack.config.js文件）

```javascript
var path = require('path');
module.exports = {
    //入口文件
    entry:'./src/index.js',
     /*entry:{//多入口文件写法
        main:'./src/index.js'
    },*/
    //出口文件
    output:{
        filename:'[name].js',//根据入口文件的名字自动生成输出文件名
        path:path.resolve(__dirname,'dist')
    },
    //开发模式-生产环境->production
    mode:'development'
}
```

dist文件夹( 打包后的文件 ) ==> 目录下新建index.html，渲染页面->可在后边以loader的形式加载（推荐）

### 5. 自动监听-自动打包  webpack --watch

### 6.配置服务器启动 ( 自动更新 ->整个页面的刷新)

- 安装服务->cnpm install webpack-dev-server -D

- 启动服务->webpack-dev-server 

- 配置服务启动默认目录

  ```javascript
  devServer:{
          contentBase:'dist',
          port:9999
      }
  
  ```

  - 配置完之后需要重新启动下服务->webpack-dev-server 

### 7.热更新 ( 类ajax刷新 )

webpack-dev-server --hot

### 8.loader（解析器）

- 本地文件写在src下->打包到dist下

- 在入口文件中引入 exa ：  require('./index.css') 

------->**注意：只有入口文件require加载进来的才会被执行打包---->plugin的不会打包**<-------

```javascript
module:{
        rules:[
            //css loader
            {
                test:/\.css$/,//匹配css文件
                use:['style-loader','css-loader']//加载相应插件
            }
        ]
    }
//后面可以利用plugin单独抽离输出一个css文件，这样是一起压缩在出口文件里的
```

- css loader需要下载相应的解析插件->cnpm install style-loader css-loader -D

下载完后->打包+启动服务器

- html loader下载相应插件->cnpm install file-loader html-loader extract-loader -D

  ```javascript
   //html loader
  {
      test:/\.html$/,
          use:[
              // 3.单独抽离的html进行配置
              {
                  loader:'file-loader',
                  options:{
                      name:'index.html'
                  }
              },
              //2.单独抽离html->这样打包之后html就会以一个单独的文件进行存在。而不是都再出口文件中
              {
                  loader:'extract-loader'
              },
              //1.找到html文件
              {
                  loader:'html-loader'
              }
          ]
  }
  ```

  - img loader ->cnpm install url-loader -D

    ```javascript
     // img loader
    {
        test:/\.(png|jpg)$/,
            use:[
                {
                    loader:'url-loader',
                    options:{//图片默认是以base64格式加载的
                        limit:8192,//限制大小
                 //[contenthash:8]给文件加一个根据文件内容生成的hash值（文件ID）文件改变hash也会改变图片只用hash就可以
                        name:"img/[name].[hash:8][ext]"//然后解析成正常的图片格式，以一个单独的文件在dist中存在
                    }
                }
            ]
    }
    ```

    

### 9.plugin (插件)

- HTML插件（单独抽离html）--->cnpm install html-webpack-plugin -D

  - 不同的是需要在webpack.config.js中引入一下所需的插件

  ```javascript
  var HtmlWebpackPlugin = require('html-webpack-plugin'); 
  ```

  然后应用

  ```javascript
  //应用 plugin
      plugins:[
          new HtmlWebpackPlugin({
              title:'',//定义打包后html的标题
              template:'./src/index.html'//指定抽离的html文件
               //压缩输出的html
              // minify:{
              //     collapseWhitespace:true
              // }
          })
      ]
  ```

- JS插件  --> cnpm install uglifyjs-webpack-plugin -D

  webpack.config.js中引入

```javascript
var Uglifyjs = require('uglifyjs-webpack-plugin');
```

应用

```javascript
new Uglifyjs() 
```

- CSS插件 -->cnpm install mini-css-extract-plugin -D

  ```javascript
  //单独抽离出css文件--->可对比require的css
  new MiniCss({
      filename:'[name]_[contenthash:8].css'//定义输出的名字
  })
  ```

多个配置文件时指定用哪个打包：

webpack -config = name

### 10.执行过程

![](D:\doc\MD\执行过程.png)

串行执行



