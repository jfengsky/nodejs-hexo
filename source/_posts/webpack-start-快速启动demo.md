---
title: webpack-start 快速启动demo
date: 2017-04-24 10:07:28
tags: webpack react
categories: javascript
---
为了避免每次配置webpack和react环境，为了节约时间，这里自己弄好了一个环境，以后其其它项目和demo直接引用这个即可

##### 项目地址及启动命令如下

    git clone https://github.com/jfengsky/webpack-start.git

    // watch并启动服务
    "start": "npm run watch & npm run server",

    // 打包代码
    "build": "webpack --progress --colors",

    // watch改动
    "watch": "webpack --progress --colors --watch",

    // 热启动服务
    "server": "webpack-dev-server --watch --hot --inline"

##### 目录

    |-- dist 打包后的文件目录
    |-- src  开发目录
       |-- index.js 主入口文件
       |-- index.html 模板文件
    |-- index.html 启动服务后的首页，该文件由src/index.html生成
    |-- webpack.config.js 打包配置文件

##### webpack配置文件

    const webpack = require('webpack')
    // 生成html用到的插件
    const HtmlWebpackPlugin = require('html-webpack-plugin')

    module.exports = {
      entry: {
        index: './src/index.js'
      },
      output: {
        path: __dirname + '/dist/',
        filename: '[name].js'
      },
      module: {
        rules: [
          {
            test: /\.js|jsx$/,
            use: [
              'babel-loader',
            ],
            exclude: /node_modules/
          }
        ]
      },
      plugins: [
        new webpack.optimize.CommonsChunkPlugin({
          name: ['vendor']
        }),
        new HtmlWebpackPlugin({
          filename: '../index.html',
          template: 'src/index.html'
        })
      ],
      devServer: {
        // contentBase: __dirname + "/dist/",
        port: 8080,
        inline:true
      }
    }