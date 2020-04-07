---
title: webpack4 个人配置
date: 2018-09-06 16:04:31
tags: webpack
categories: 学习笔记
---

# webpack4 个人配置

很久之前配过一份自用的webpack3，也一直在用，而4出来很久了，而且修改提升了很多，一直想升级成4，拖了很久，最近几天零零星星的时间，翻查资料，按着之前3的一些功能设置完成了一份4的配置，下面记录一下配置过程。
<!-- more -->

> 源码已经放 [Github](https://github.com/rason00/Practice/tree/master/Webpack)

## 设备

```diff
> 电脑：MacBook Pro
> cnpm：6.0.0
> node：8.9.0
> webpack：4.17.2
> 时间：2018-09-05
```

## 目标

1. 自动生成html
2. 自动清理dist文件夹
3. 本地服务器
4. 分离css
5. 分离css图片
6. 处理html图片
7. 配置sass
8. 自动添加css3前缀
9. 消除未使用的css
10. 配置babel
11. 引入jquery
12. 分割第三方js
13. 配置全局资源文件夹
14. 配置测试和生产环境
15. 配置多入口多页面

## 过程

### 初始化

**首先打开终端输入以下命令**

```diff
mkdir demo 	// 创建demo文件夹
cd demo 	// 进入demo文件夹
npm init -y // 初始化
```

> 注：// xxx 只是注释

此时你的 `demo`文件夹里面的目录是如此的

```diff
demo
|- package.json
```

**继续初始化目录成下面（以下目录仅是个人习惯）**

```diff
demo
|- /src          // 源码目录
  |- /assets     // 全局资源目录
  |- /css        // 样式目录
    |- css.css
    |- sass.sass
  |- /images     // 图片目录
    |- 1.jpg
    |- 2.jpg
  |- /js         // js目录
    |- index.js
  |- /page       // 页面目录
    |- index.html
|- package.json
```



**安装`webpack`和`webpack-cli`**

```diff
cnpm install webpack webpack-cli --save-dev
```

> 完成后目录会多出 `node_modules` 的资源目录，无需理会

添加配置文件（根目录新建 `webpack.config.js` 文件）

**webpack.config.js**

```diff
const path = require('path');

module.exports = {
  //入口
  entry: {
      app: './src/js/index.js'
  }, // 对应初始化目录的js文件
  // 插件
  plugins: [],
  //出口
  output: {
    filename: '[name].js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 配置
  module: {
    rules: []
  }
};
```

> 以上是 `webpack.config.js` 文件初始化内容

**NPM 脚本(NPM Scripts)**

**package.json**

```diff
{
  "name": "demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
+	"build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.17.2",
    "webpack-cli": "^3.1.0"
  }
}
```

> 现在，可以使用 `npm run build` 命令了

### 1.自动生成html

**设定 HtmlWebpackPlugin**

首先安装插件，并且调整 `webpack.config.js` 文件：

```diff
cnpm install --save-dev html-webpack-plugin
```

**webpack.config.js**

```diff
  const path = require('path');
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/js/index.js',
    },
+   plugins: [
+     new HtmlWebpackPlugin({
+     template: './src/page/index.html', // 模版路径（可先在index.html里写些测试内容）
+     })
+   ],
    output: {
      filename: '[name].js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```

### 2.清理 `/dist` 文件夹

```diff
cnpm install clean-webpack-plugin --save-dev
```

**webpack.config.js**

```diff
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
+ const CleanWebpackPlugin = require('clean-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/js/index.js',
    },
    plugins: [
+     new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      })
    ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```

> 现在执行 `npm run build`，再检查 `/dist` 文件夹。如果一切顺利，你现在应该不会再看到旧的文件，只有构建后生成的文件！

### 3. 本地服务器

使用`webpack-dev-server` 为你提供了一个简单的 web 服务器，并且能够实时重新加载(live reloading)。

```diff
cnpm install --save-dev webpack-dev-server	//安装
```

修改配置文件，告诉开发服务器(dev server)，在哪里查找文件：

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      })
  ],
  // 配置
  module: {
    rules: []
  },
+ devServer: {
+   contentBase: './dist'
+ },
};
```

> 以上配置告知 `webpack-dev-server`，在 `localhost:8080` 下建立服务，将 `dist` 目录下的文件，作为可访问文件。（注：现在你会看到浏览器会报一些 `mode` 警告，先不用理，后面会处理）

**package.json**

```diff
{
  "name": "demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
+	"dev": "webpack-dev-server --open",  
	"build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "clean-webpack-plugin": "^0.1.19",
    "html-webpack-plugin": "^3.2.0",
    "webpack": "^4.17.2",
    "webpack-cli": "^3.1.0",
    "webpack-dev-server": "^3.1.7"
  }
}
```

> 现在，可以使用 `npm run dev` 命令打开本地服务器，在浏览器上看到 `/src/page/index.html`里的内容了

### 4.分离css

**加载 CSS**

```diff
cnpm install --save-dev style-loader css-loader
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      })
  ],
  // 配置
  module: {
  	rules: [
+       {
+         test: /\.css$/,
+         use: [
+           'style-loader',
+           'css-loader'
+         ]
+       }
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

打开入口文件`/src/js/index.js`引入`css`

**index.js**

```diff
import '../css/css.css';
```

> 运行 `npm run dev` 命令，就可以在浏览器上看到页面样式的变化了。（注：自行在 `/src/css/css.css` 里添加对 `/src/page/index.html` 页面的样式处理）

**打包分离css**

`webpack4` 中不再使用 `extract-text-webpack-plugin` 分离 `css` 而是使用 `mini-css-extract-plugin` 插件分离css

```diff
cnpm install --save-dev mini-css-extract-plugin
```

修改配置

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
+ const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
+     new MiniCssExtractPlugin({
+        filename: "css/[name].css",
+     }),      
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
+         {
+           loader: MiniCssExtractPlugin.loader, // 使用规则 
+			options: {
+             publicPath: '../' // 处理分离路径问题
+           }
+		  }
-           'style-loader',
            'css-loader'
          ]
        }
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 现在运行 `npm run build` 命令，查看 `/dist` 文件目录，会看到一个分离出来的 `css` 文件夹，里面会有一个 `app.css` 文件，那个就是分离于 `/src/css/css.css` 文件的内容。（注：`app.css` 文件的名字是关联于入口 `app:xxxx` 中的 `app` 名字）

### 5.分离css图片

**加载图片**

```diff
cnpm install --save-dev file-loader
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),      
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader'
          ]
        },
+        {
+           test: /\.(png|svg|jpg|gif)$/,
+           use: [{
+             loader: 'file-loader',
+             options: {
+               name: '[name].[ext]',
+               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
+               outputPath: 'images' //定义输出的图片文件夹
+             }
+           }]
+         },   
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 运行 `npm run build` 命令，可以在 `dist` 目录里面看到 `images` 文件夹里面会有 `css` 里引入的图片。（注：自行在 `css` 中加入图片样式看效果）

### 6.处理html图片

现在 `css` 里引用的图片可以使用，但是在 `html` 中通过 `img` 标签引用的图片还是无法正常使用，需要进行以下配置。

**安装 `html-loader`**

```diff
cnpm install --save-dev html-loader
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),      
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader'
          ]
        },
        {
           test: /\.(png|svg|jpg|gif)$/,
           use: [{
             loader: 'file-loader',
             options: {
               name: '[name].[ext]',
               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
               outputPath: 'images' //定义输出的图片文件夹
             }
           }]
         }, 
+      {
+        test: /\.html$/,
+        use: [{
+          loader: 'html-loader',
+          options: {
+            minimize: true,
+            removeComments: false,
+            collapseWhitespace: false
+          }
+        }],
+      },
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 运行 `npm run build` 命令，可以在 `dist` 目录里面看到 `images` 文件夹里面会有 `html` 里引入的图片。（注：自行在 `html` 中加入图片样式看效果）

### 7.配置sass

**安装**

```diff
cnpm install sass-loader node-sass --save-dev
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),      
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader'
          ]
        },
        {
           test: /\.(png|svg|jpg|gif)$/,
           use: [{
             loader: 'file-loader',
             options: {
               name: '[name].[ext]',
               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
               outputPath: 'images' //定义输出的图片文件夹
             }
           }]
         }, 
        {
          test: /\.html$/,
          use: [{
            loader: 'html-loader',
            options: {
              minimize: true,
              removeComments: false,
              collapseWhitespace: false
            }
          }],
        },
+        {
+        test: /\.(sass|scss)$/,
+        use: [
+          // 分离sass
+          {
+            loader: MiniCssExtractPlugin.loader,
+            options: {
+              publicPath: "../"
+            }
+          },
+          "css-loader",
+          "sass-loader"
+        ]
+      },
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

**/src/js/index.js**

```diff
- import '../css/css.css';
+ import '../css/scss.scss';
```

> 修改 `index` 中的引入，现在运行`npm run dev` 可以在浏览器看到页面效果，同时运行 `npm run build` 可以在 `dist/css` 文件夹里看到打包分离出来的 `css` 文件（注：自行在 `scss.scss` 中添加样式以便看效果）

### 8.自动添加css3前缀

编写使用 `css3` 样式需要添加各浏览器的前缀，现在可以配置成自动添加各个浏览器前缀

**安装**

```diff
cnpm install --save-dev postcss-loader autoprefixer
```

**根目录新建 `postcss.config.js`**

```diff
module.exports = {
  plugins: {
    'autoprefixer': {
      browsers: [
        'iOS >= 7', 
        'Android >= 4.1',
        'last 10 Chrome versions',
        'last 10 Firefox versions',
        'Safari >= 6',
        'ie > 8'
      ]
    }
  }
}
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),      
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader',
+            "postcss-loader"
          ]
        },
        {
           test: /\.(png|svg|jpg|gif)$/,
           use: [{
             loader: 'file-loader',
             options: {
               name: '[name].[ext]',
               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
               outputPath: 'images' //定义输出的图片文件夹
             }
           }]
         }, 
        {
          test: /\.html$/,
          use: [{
            loader: 'html-loader',
            options: {
              minimize: true,
              removeComments: false,
              collapseWhitespace: false
            }
          }],
        },
        {
        test: /\.(sass|scss)$/,
        use: [
          // 分离sass
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "../"
            }
          },
          "css-loader",
          "sass-loader",
+          "postcss-loader"
        ]
      },
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 运行 `npm run build` 可以看到打包出来的样式文件效果如下。（注：以下只是个人测试样式）

**/dist/css/app.css**

```diff
.test {
  width: 100px;
  height: 100px;
  background: url(../images/1.jpg);
  -webkit-transform: rotate(45deg);
      -ms-transform: rotate(45deg);
          transform: rotate(45deg); }
```

### 9.消除未使用的css

如果项目中有很多写了但是没有使用的样式，打包出来的话会影响文件大小，现在配置一下，打包时自动消除未使用的样式

**安装**

```diff
cnpm install --save-dev purifycss-webpack purify-css
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
+ const glob = require('glob');
+ const PurifyCSSPlugin = require('purifycss-webpack');

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),  
+      new PurifyCSSPlugin({
+       paths: glob.sync(path.join(__dirname, 'src/page/*.html')),// 对应需要消除css的目录文件
+     }),
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader',
            "postcss-loader"
          ]
        },
        {
           test: /\.(png|svg|jpg|gif)$/,
           use: [{
             loader: 'file-loader',
             options: {
               name: '[name].[ext]',
               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
               outputPath: 'images' //定义输出的图片文件夹
             }
           }]
         }, 
        {
          test: /\.html$/,
          use: [{
            loader: 'html-loader',
            options: {
              minimize: true,
              removeComments: false,
              collapseWhitespace: false
            }
          }],
        },
        {
        test: /\.(sass|scss)$/,
        use: [
          // 分离sass
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "../"
            }
          },
          "css-loader",
          "sass-loader",
          "postcss-loader"
        ]
      },
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 现在可以在样式文件里加入一些没用的代码，然后运行 `npm run build` 在 `/dist/css` 目录里的文件查看，可以看到没用的样式都没有被打包出来。

### 10.配置babel

`es6` 书写代码十分方便，但是项目上还是需要转换成 `es5` 比较保险，现在通过配置 `babel` 打包时可以自动转换，就可以放心写 `es6` 风格代码了

**安装**

```diff
cnpm install --save-dev babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const glob = require('glob');
const PurifyCSSPlugin = require('purifycss-webpack');

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),  
      new PurifyCSSPlugin({
       paths: glob.sync(path.join(__dirname, 'src/page/*.html')),// 对应需要消除css的目录文件
     }),
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader',
            "postcss-loader"
          ]
        },
        {
           test: /\.(png|svg|jpg|gif)$/,
           use: [{
             loader: 'file-loader',
             options: {
               name: '[name].[ext]',
               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
               outputPath: 'images' //定义输出的图片文件夹
             }
           }]
         }, 
        {
          test: /\.html$/,
          use: [{
            loader: 'html-loader',
            options: {
              minimize: true,
              removeComments: false,
              collapseWhitespace: false
            }
          }],
        },
        {
            test: /\.(sass|scss)$/,
            use: [
              // 分离sass
              {
                loader: MiniCssExtractPlugin.loader,
                options: {
                  publicPath: "../"
                }
              },
              "css-loader",
              "sass-loader",
              "postcss-loader"
            ]
       },
+       {
+            test: /\.js$/,
+            exclude: /(node_modules|bower_components)/,
+            use: {
+              loader: 'babel-loader',
+              options: {
+                presets: ['@babel/preset-env']
+              }
+            }
+       }
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 现在在 `/src/js/index.js` 里面写入 `es6` 风格代码，然后 `npm run build` 后查看打包后的 `js` 可以看到代码都被转换成 `es5` 风格了。

### 11.引入jquery

由于个人项目需求，很多时候都会用上 `jquery` 所以在这里把 `jquery` 也进行了配置。

**安装**

```diff
cnpm install --save-dev jquery
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const glob = require('glob');
const PurifyCSSPlugin = require('purifycss-webpack');
+ const webpack = require('webpack');

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),  
      new PurifyCSSPlugin({
       paths: glob.sync(path.join(__dirname, 'src/page/*.html')),// 对应需要消除css的目录文件
     }),
+     // 全局暴露
+      new webpack.ProvidePlugin({
+        $: "jquery"
+      }),
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader',
            "postcss-loader"
          ]
        },
        {
           test: /\.(png|svg|jpg|gif)$/,
           use: [{
             loader: 'file-loader',
             options: {
               name: '[name].[ext]',
               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
               outputPath: 'images' //定义输出的图片文件夹
             }
           }]
         }, 
        {
          test: /\.html$/,
          use: [{
            loader: 'html-loader',
            options: {
              minimize: true,
              removeComments: false,
              collapseWhitespace: false
            }
          }],
        },
        {
            test: /\.(sass|scss)$/,
            use: [
              // 分离sass
              {
                loader: MiniCssExtractPlugin.loader,
                options: {
                  publicPath: "../"
                }
              },
              "css-loader",
              "sass-loader",
              "postcss-loader"
            ]
       },
       {
            test: /\.js$/,
            exclude: /(node_modules|bower_components)/,
            use: {
              loader: 'babel-loader',
              options: {
                presets: ['@babel/preset-env']
              }
            }
       }
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 现在可以在 `/src/js/index.js` 中随意使用 `jquery` 语法了。

### 12.分割第三方js

现在打包后你会发现，打包出来的 `js` 文件会很大，因为它把引入的 `jquery` 也打包到里面去了，我们现在希望可以单独把 `jquery` 分割出来，所以进行下面的配置

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const glob = require('glob');
const PurifyCSSPlugin = require('purifycss-webpack');
const webpack = require('webpack');

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
+ // 提取js vendor 可改名字
+  optimization: {
+    splitChunks: {
+      cacheGroups: {
+        // 打包node_modules中的文件
+        vendor: {
+          name: "vendor",
+          test: /[\\/]node_modules[\\/]/,
+          chunks: "all",
+          priority: 10
+        }
+      }
+    }
+  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),  
      new PurifyCSSPlugin({
       paths: glob.sync(path.join(__dirname, 'src/page/*.html')),// 对应需要消除css的目录文件
     }),
     // 全局暴露
      new webpack.ProvidePlugin({
        $: "jquery"
      }),
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader',
            "postcss-loader"
          ]
        },
        {
           test: /\.(png|svg|jpg|gif)$/,
           use: [{
             loader: 'file-loader',
             options: {
               name: '[name].[ext]',
               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
               outputPath: 'images' //定义输出的图片文件夹
             }
           }]
         }, 
        {
          test: /\.html$/,
          use: [{
            loader: 'html-loader',
            options: {
              minimize: true,
              removeComments: false,
              collapseWhitespace: false
            }
          }],
        },
        {
            test: /\.(sass|scss)$/,
            use: [
              // 分离sass
              {
                loader: MiniCssExtractPlugin.loader,
                options: {
                  publicPath: "../"
                }
              },
              "css-loader",
              "sass-loader",
              "postcss-loader"
            ]
       },
       {
            test: /\.js$/,
            exclude: /(node_modules|bower_components)/,
            use: {
              loader: 'babel-loader',
              options: {
                presets: ['@babel/preset-env']
              }
            }
       }
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 现在运行 `npm run build` 可以在 `/dist/js` 里面看到多出了 `vendor.js` 文件，点击打开可以看到里面是 `jquery` 的代码，而另外的 `app.js` 文件小了很多。

### 13.配置全局资源文件夹

有时项目上有一些文件或其他的东西不希望进行打包，那么可以设置一个全局资源目录，里面的文件是全部原样搬运，不做打包处理。

**安装**

```diff
cnpm install --save-dev copy-webpack-plugin
```

**webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const glob = require('glob');
const PurifyCSSPlugin = require('purifycss-webpack');
const webpack = require('webpack');
+ const copyWebpackPlugin = require("copy-webpack-plugin");

module.exports = {
  //入口
  entry: {
    app: './src/js/index.js',
  },
  //出口
  output: {
    filename: 'main.js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  optimization: {
    splitChunks: {
      cacheGroups: {
        // 打包node_modules中的文件
        vendor: {
          name: "vendor",
          test: /[\\/]node_modules[\\/]/,
          chunks: "all",
          priority: 10
        }
      }
    }
  },
  // 插件
  plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        template: './src/page/index.html',
      }),
      new MiniCssExtractPlugin({
         filename: "css/[name].css",
      }),  
      new PurifyCSSPlugin({
       paths: glob.sync(path.join(__dirname, 'src/page/*.html')),// 对应需要消除css的目录文件
     }),
     // 全局暴露
      new webpack.ProvidePlugin({
        $: "jquery"
      }),
+     // 全局资源搬运
+     new copyWebpackPlugin([{
+       from: './src/assets', // 搬运原始目录
+       to: './assets' // 搬运目标目录
+     }]),
  ],
  // 配置
  module: {
  	rules: [
       {
         test: /\.css$/,
         use: [
          {
            loader: MiniCssExtractPlugin.loader, // 使用规则 
 			options: {
              publicPath: '../' // 处理分离路径问题
            }
 		  }
            'css-loader',
            "postcss-loader"
          ]
        },
        {
           test: /\.(png|svg|jpg|gif)$/,
           use: [{
             loader: 'file-loader',
             options: {
               name: '[name].[ext]',
               limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
               outputPath: 'images' //定义输出的图片文件夹
             }
           }]
         }, 
        {
          test: /\.html$/,
          use: [{
            loader: 'html-loader',
            options: {
              minimize: true,
              removeComments: false,
              collapseWhitespace: false
            }
          }],
        },
        {
            test: /\.(sass|scss)$/,
            use: [
              // 分离sass
              {
                loader: MiniCssExtractPlugin.loader,
                options: {
                  publicPath: "../"
                }
              },
              "css-loader",
              "sass-loader",
              "postcss-loader"
            ]
       },
       {
            test: /\.js$/,
            exclude: /(node_modules|bower_components)/,
            use: {
              loader: 'babel-loader',
              options: {
                presets: ['@babel/preset-env']
              }
            }
       }
    ]
 },
 devServer: {
   contentBase: './dist'
 },
};
```

> 先在 `/src/assets/` 文件夹里放些东西，然后运行 `npm run build` 进行打包，之后可以看到 `dist` 文件夹里多出了 `assets` 文件，里面存放着直接搬运过来的文件。

### 14.配置测试和生产环境

项目上本地测试环境和打包的生产环境需求会有很多不同，比如一些代码压缩，映射之类的，先在我们来配置双环境。

**首先进行文件目录的修改（自行添加或删除）**

```diff
    demo
    |- /src               // 源码目录
        |- /assets        // 全局资源目录
        |- /css           // 样式目录
            |- css.css
            |- sass.sass
        |- /images        // 图片目录
            |- 1.png
        |- /js            // js目录
            |- index.js
        |- /page          // 页面目录
            |- index.html
    |- package.json
    |- postcss.config.js
    |- webpack.config.js
+   |- webpack.common.js  // 公共配置
+   |- webpack.dev.js     // 测试环境
+   |- webpack.por.js     // 生产环境
```

**安装**

```diff
cnpm install --save-dev webpack-merge
```

**注：以下文件配置不做详细讲解，具体都做了些注释讲解了**

**webpack.common.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const glob = require('glob');
const PurifyCSSPlugin = require('purifycss-webpack');
const webpack = require('webpack');
const copyWebpackPlugin = require("copy-webpack-plugin");

let config = {
  //入口
  entry: {
    app: './src/js/index.js'
  }, // 对应初始化目录的js文件
  //出口
  output: {
    filename: 'js/[name].js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 提取js common 可改名字
  optimization: {
    splitChunks: {
      cacheGroups: {
        // 注意: priority属性
        // 其次: 打包业务中公共代码
        // common: {
        //   name: "common",
        //   chunks: "all",
        //   minSize: 1,
        //   priority: 0
        // },
        // 首先: 打包node_modules中的文件
        vendor: {
          name: "vendor",
          test: /[\\/]node_modules[\\/]/,
          chunks: "all",
          priority: 10
        }
      }
    }
  },
  // 插件
  plugins: [
    new CleanWebpackPlugin(['dist']),
    new HtmlWebpackPlugin({
      template: './src/page/index.html', // 模版路径
    }),
    new MiniCssExtractPlugin({
      filename: "css/[name].css",
      // chunkFilename: "css/[id].css"
    }),
    new PurifyCSSPlugin({
      // paths: glob.sync(path.join(__dirname, 'src/page/*.html')),
      paths: glob.sync(path.join(__dirname, 'src/page/*.html')),
    }),
    // 全局暴露
    new webpack.ProvidePlugin({
      $: "jquery"
    }),
    // 全局资源搬运
    new copyWebpackPlugin([{
      from: './src/assets',
      to: './assets'
    }]),
  ],
  // 配置
  module: {
    rules: [
      // css
      {
        test: /\.css$/,
        use: [{
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../'
            }
          },
          "css-loader",
          "postcss-loader"
        ]
      },
      // 图片
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: [{
          loader: 'file-loader',
          options: {
            name: '[name].[ext]',
            limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
            outputPath: 'images' //定义输出的图片文件夹
          }
        }]
      },
      {
        test: /\.html$/,
        use: [{
          loader: 'html-loader',
          options: {
            minimize: true,
            removeComments: false,
            collapseWhitespace: false
          }
        }],
      },
      {
        test: /\.(sass|scss)$/,
        use: [
          // 分离sass
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "../"
            }
          },
          "css-loader",
          "postcss-loader",
          "sass-loader"
        ]
      },
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
  },
};

module.exports = config
```

**webpack.dev.js**

```diff
const path = require('path');
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
const webpack = require('webpack');

module.exports = merge(common, {
  mode: 'development', // 这里解决的是前面浏览器的警告
  devtool: 'inline-source-map',
  // 本地服务器
  devServer: {
    //设置基本目录结构
    contentBase: path.resolve(__dirname, 'dist'),
    //服务器的IP地址，可以使用IP也可以使用localhost
    host: '127.0.0.1',
    //服务端压缩是否开启
    compress: true,
    //配置服务端口号
    port: 1717,
    // 热更新
    hot: true
  },
  // 插件
  plugins: [
    // 热更新
    new webpack.HotModuleReplacementPlugin(),
    // 配置变量
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('development') // 在编译的代码里设置了`process.env.NODE_ENV`变量
    }),
  ]
});
```

> 注：以上做了简单的热更新配置，并不完善

**webpack.por.js**

```diff
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
const webpack = require('webpack');

module.exports = merge(common, {
  mode: 'production', // 这里解决的是前面浏览器的警告
  // 插件
  plugins: [
    // 配置变量
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('production') // 在编译的代码里设置了`process.env.NODE_ENV`变量
    }),
  ]
});
```

> `mode` 的配置分为 `none` , `development` , `production`

完成上面的修改后，最后还需要修改以下文件

**package.json**

```diff
{
  "name": "demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
-   "dev": "webpack-dev-server --open",  
-	"build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1",
+    "build": "webpack --config webpack.prod.js",
+    "dev": "webpack-dev-server --open --config webpack.dev.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.0.0",
    "@babel/preset-env": "^7.0.0",
    "autoprefixer": "^9.1.5",
    "babel-loader": "^8.0.0-beta.0",
    "clean-webpack-plugin": "^0.1.19",
    "copy-webpack-plugin": "^4.5.2",
    "css-loader": "^1.0.0",
    "file-loader": "^2.0.0",
    "html-loader": "^0.5.5",
    "html-webpack-plugin": "^3.2.0",
    "jquery": "^3.3.1",
    "mini-css-extract-plugin": "^0.4.2",
    "node-sass": "^4.9.3",
    "postcss-loader": "^3.0.0",
    "purify-css": "^1.2.5",
    "purifycss-webpack": "^0.7.0",
    "sass-loader": "^7.1.0",
    "style-loader": "^0.23.0",
    "webpack": "^4.17.2",
    "webpack-cli": "^3.1.0",
    "webpack-dev-server": "^3.1.7",
    "webpack-merge": "^4.1.4"
  }
}

```

现在我们就简单的分开完成了公共配置，测试环境配置，和生产环境配置了。

>  运行 `npm run build` 和 `npm run dev` 可以看到相关的效果。

### 15.配置多入口多页面

多入口和多页面，这个是我个人的一个想法配置，我希望是我的 `/src/js/*.js` 目录里面有几个 `js` 文件，那么 `webpack` 就自动给我添加几个入口，同理，我的 `/src/page/*.html` 目录里有几个 `html` 文件，那么 `webpack` 页给我打包出几个页面文件。并且我希望打包后的目录是下面这样的。

```diff
  dist
  |- /assets      // 全局资源
  |- /css         // css文件
  |- /images      // 图片文件
  |- /js          // js文件
  |- /page        // 其他页面文件夹
    |- xxx.html
  |- index.html   // 首页
```

以下写法有使用到 `node` 的一些语法函数，我仅做了简单的注释（理解并不一定正确，毕竟只是看过一点 `node` 的教程和文档而已）

**webpack.common.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const glob = require('glob');
const PurifyCSSPlugin = require('purifycss-webpack');
const webpack = require('webpack');
const copyWebpackPlugin = require("copy-webpack-plugin");
+ const fs = require('fs');

+ // 文件夹信息
+ function readFileList(thePath, filesList) {
+   let files = fs.readdirSync(thePath); // 返回一个包含“指定目录下所有文件名称”的数组对象
+   files.forEach(function (itm, index) {
+     let stat = fs.statSync(thePath + itm); // 获取文件或者文件夹信息
+     if (stat.isDirectory()) { // 是否是文件夹
+       //递归读取文件
+       readFileList(thePath + itm + "/", filesList)
+     } else {
+       let data = path.parse(thePath + itm)
+       filesList[data.name] = thePath + itm
+     }
+   })
+ }

+ // 获取文件列表
+ function getFileList(thePath) {
+   let filesList = {};
+   readFileList(thePath, filesList);
+   return filesList;
+ }

+ // 动态入口
+ let theEntry = getFileList('./src/js/');

let config = {
  //入口
-  entry: {
-    app: './src/js/index.js'
-  }, // 对应初始化目录的js文件
+  entry: theEntry
  //出口
  output: {
    filename: 'js/[name].js', // 输出的文件名
    path: path.resolve(__dirname, 'dist') // 输出的路径
  },
  // 提取js common 可改名字
  optimization: {
    splitChunks: {
      cacheGroups: {
        // 注意: priority属性
        // 其次: 打包业务中公共代码
        // common: {
        //   name: "common",
        //   chunks: "all",
        //   minSize: 1,
        //   priority: 0
        // },
        // 首先: 打包node_modules中的文件
        vendor: {
          name: "vendor",
          test: /[\\/]node_modules[\\/]/,
          chunks: "all",
          priority: 10
        }
      }
    }
  },
  // 插件
  plugins: [
    new CleanWebpackPlugin(['dist']),
-    new HtmlWebpackPlugin({
-      template: './src/page/index.html', // 模版路径
-    }),
    new MiniCssExtractPlugin({
      filename: "css/[name].css",
      // chunkFilename: "css/[id].css"
    }),
    new PurifyCSSPlugin({
      // paths: glob.sync(path.join(__dirname, 'src/page/*.html')),
      paths: glob.sync(path.join(__dirname, 'src/page/*.html')),
    }),
    // 全局暴露
    new webpack.ProvidePlugin({
      $: "jquery"
    }),
    // 全局资源搬运
    new copyWebpackPlugin([{
      from: './src/assets',
      to: './assets'
    }]),
  ],
  // 配置
  module: {
    rules: [
      // css
      {
        test: /\.css$/,
        use: [{
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../'
            }
          },
          "css-loader",
          "postcss-loader"
        ]
      },
      // 图片
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: [{
          loader: 'file-loader',
          options: {
            name: '[name].[ext]',
            limit: 5, // 表示小于xxb图片转为base64,大于xxb的是路径
            outputPath: 'images' //定义输出的图片文件夹
          }
        }]
      },
      {
        test: /\.html$/,
        use: [{
          loader: 'html-loader',
          options: {
            minimize: true,
            removeComments: false,
            collapseWhitespace: false
          }
        }],
      },
      {
        test: /\.(sass|scss)$/,
        use: [
          // 分离sass
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "../"
            }
          },
          "css-loader",
          "postcss-loader",
          "sass-loader"
        ]
      },
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
  },
};

+ let theHtml = getFileList('./src/page/')

+ for (let key in theHtml) {
+   let theFileName = ''
+   if(key == 'index'){
+     theFileName = './' + key + '.html'
+   }else {
+     theFileName = './page/' + key + '.html'
+   }
+   var conf = {
+     filename: theFileName, //生成的html存放路径，相对于path
+     template: 'src/page/' + key + '.html', //html模板路径
+     inject: false, //js插入的位置，true/'head'/'body'/false
+     minify: {
+       // 清除html注释
+       removeComments: true,
+       // 压缩html
+       collapseWhitespace: true,
+       // 是否去掉属性的双引号
+       removeAttributeQuotes: true,
+       // 省略布尔属性的值 <input checked="true"/> ==> <input />
+       collapseBooleanAttributes: true,
+       // 删除所有空格作属性值 <input id="" /> ==> <input />
+       removeEmptyAttributes: true,
+       // 删除<script>的type="text/javascript"
+       removeScriptTypeAttributes: true,
+       // 删除<style>和<link>的type="text/css"
+       removeStyleLinkTypeAttributes: true,
+     },
+     /*
+      * 压缩这块，调用了html-minify，会导致压缩时候的很多html语法检查问题，
+      * 如在html标签属性上使用{{...}}表达式，所以很多情况下并不需要在此配置压缩项，
+      * 另外，UglifyJsPlugin会在压缩代码的时候连同html一起压缩。
+      * 为避免压缩html，需要在html-loader上配置'html?-minimize'，见loaders中html-loader的配置。
+      */
+     // minify: { //压缩HTML文件
+     //  removeComments: true, //移除HTML中的注释
+     //  collapseWhitespace: false //删除空白符与换行符
+     // }
+   };
+   if (key in config.entry) {
+     conf.inject = 'body';
+     conf.chunks = [key, 'vendor'];
+     conf.hash = true;
+   }
+   config.plugins.push(new HtmlWebpackPlugin(conf));
+ }

module.exports = config
```

以上就可以配置之后就可以实现上面所讲的动态多入口和多页面效果了。

**最后说明：多入口和多页面的配置只是我个人的一些逻辑想法而已，使用的时候要遵循的是，`/src/js` 里面的 `js` 文件名字要和 `/src/page` 里面的 `html` 文件名字一一对应，打包后每个页面引入的 `js` 是本页面对应名字的 `js` 和 `vendor.js` 即分割的第三方 `js` 这里是 `jquery`（例：`index.html` 引入的是 `index.js` 和 `vendor.js` ,而 `test.html` 引入的是 `test.js` 和 `vendor.js`,）**

**最最最后注意：`js` 和 `page` 名字一定要一一对应，一定要，一定要，一定要！**

> 以上参考资料有 [webpack官方文档](https://webpack.docschina.org/concepts/)，[node官方文档](http://nodejs.cn/api/fs.html)

> 以上源码已经放 [Github](https://github.com/rason00/Practice/tree/master/Webpack)