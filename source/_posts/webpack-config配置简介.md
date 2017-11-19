---
title: webpack config配置简介
date: 2016-05-21 09:06:36
categories: 前端构建工具
tags: [webpack, config, 前端构建工具]
---
本文只对webpack的一些简单配置（webpack.config.js）做介绍，并不深究。
### webpack是什么
webpack 简单说就是模块加载器。在webpack中，所有的资源文件如js,css,图片等都看作是模块。
webpack 支持AMD,CommonJs以及ES6语法的模块加载系统。
### webpack安装
首先你要安装node.js和npm
其次为你的项目新建一个文件夹，然后输入 npm init，然后填写相关问题。
这样会为你创建了 package.json。
接下来把webpack 安装在本地
```
npm i webpack --save-dev
```
### 项目目录
```
webpack-demo
|-node_modules
|-dist
|-build
|-src
    |-app.js
    |-component.jsx
|-package.json
|-webpack.config.js
|-webpack.dist.config.js
```
### webpack.config.js 开发环境配置文件
```
var path = require("path");
var webpack = require("webpack");
var node_modules = path.resolve(__dirname, 'node_modules');

module.exports = {
    entry: {    //入口文件配置
        js: './src/app.js',
        vendor: ['react', 'react-dom'] 
        //引用的功能代码，这样配置可以把功能代码打包在一个文件中
    },
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: './bundle.js'
    },
    module: {
        loaders: [{
            test: /\.jsx?$/,
            exclude: /node_modules/,
            loader: 'babel',
            query: {
                presets: ['react', 'es2015'] //设置支持的ES6语法
            }
        },{
            test: /\.less$/,
            loader: 'style!css!less'
        },{
            test: /\.(eot|woff|woff2|ttf|svg)$/,
            loader: 'url-loader'
                // loader: 'url-loader'
        },{
            test: /\.(png|jpg|gif)$/,
            loader: "url-loader?limit=250000"
        }],
        noParse: ['react', 'readct-dom'] //忽略掉这些目录
    },
    plugins: [
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NoErrorsPlugin(),
        new webpack.optimize.CommonsChunkPlugin('vendor', 'vendor.bundle.js') 
        //设置支持把公用代码打包到一个问题就中
    ]
};
```
### webpack.dist.config.js 生产环境配置文件
```
var webpack = require('webpack');
var path    = require('path');
var config  = require('./webpack.config');

config.output = {   //修改输出地址
  filename: './src/app.js',
  publicPath: '',
  path: path.resolve(__dirname, 'dist')
};

module.exports = config;
```
### 打包命令
把webpack的打包命令写在package.json中的script配置中，这样就不用每次输入很长的webpack命令。
配置如下：
```
"scripts": {
    "build": "webpack --config webpack.config.js",
    "dev": "webpack-dev-server",
    "deploy": "NODE_ENV=production webpack -p --config webpack.dist.config.js"
}
```
