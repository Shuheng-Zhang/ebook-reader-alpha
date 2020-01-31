# ebook-reader-alpha

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


# Vue + epub.js - 电子书阅读器学习笔记

## ❉ 关于ePub

ePub（Electronic Publication，电子出版），一种电子图书标准，由国际数位出版论坛（IDPF）提出，扩展名为`*.epub`。

## ❉ 关于epub.js库

用于对 *\*.epub* 类型电子书文件进行解析和渲染。

1. 安装

   `npm install epubjs`

2. 基础用法

   - 读取电子书文件：

     ```javascript
     let book = new Epub(EPUB_BOOK_FILE_PATH)
     ```

   - 将电子书内容渲染到页面DOM上：

     ``` javascript
     book.renderTo('dom-id', {
         // 设定内容页面宽高
         // 当前取窗口大小（铺满全屏）
         width: window.innerWidth,
         height: window.innerHeight
     })
     ```

   - 显示渲染内容：

     ````javascript
     book.rendition.display()
     ````

3. 相关模块与功能

   - `Rendition`：渲染模块，负责内容的展示，如翻页等
   - `Themes`：主题样式模块，负责内容的样式，如设定背景、字体大小、字体样式等
   - `Locations`：位置模块，负责阅读器对内容进行定位，如按照百分比跳转等
   - `Navigation`：导航模块，负责解析电子书的目录

## ❉ 关于Vue

Vue：一套用于构建用户界面的渐进式JavaScript框架。

Vue Router：由Vue官方提供的路由管理器，负责应用的页面跳转索引。

Vuex：专用于Vue应用的状态管理模式，采用集中式存储管理应用的所有组件状态，并以相应的规则保证状态以一种可预测的方式发生变化。

1. 安装Vue：

   ``` bash
   npm i vue -g # 全局安装Vue
   ```

   ``` bash
   npm i @vue/cli -g # 全局安装vue-cli工具
   ```

2. 创建Vue应用：、

   ``` bash
   vue create appname # 创建名为"appname"的应用，根据交互提示选择需要的依赖和其他配置
   ```

   ````bash
   vue gui # 启动Vue的图形化工具（可选）
   ````

3. Vue基础使用

   - `@click`：绑定点击事件
   - `v-if`：判断语句
   - `v-for`：循环遍历，需要`:key` 索引绑定
   - `v-show`：控制组件显示属性
   - `@change`：处理动态变更事件，如当进度条拖动被释放时的事件处理
   - `@input`：处理输入事件，如当进度条被拖动时的事件处理
   - `:class`：动态class绑定，`{'class_name': conditon}`
   - `:style`：动态样式绑定，`{property: variable}`

4. Vue父子组件通信

   - 父组件通过 `:variale = "variable" ` 绑定需要传出的变量；子组件通过 `props` 属性设定接收的变量的变量名和类型以接受数据
   - 父组件通过 `@function_name = "function_name"` 向子组件传出函数；子组件调用 `this.$emit('function_name', variable)` 方法调用父组件中的函数
   - 父组件通过 `this.$refs.component.function_name()` 调用子组件中的函数；子组件通过 `ref = "ref_name"` 绑定索引名称以供父组件获取自身

5. Vue动画

   - 使用 *\<transition>* 组件实现
   - `enter` 、`leave-to`：分别对应于组件进入前和组件移出后的状态
   - `enter-to` 、`leave`：分别对应于组件进入后和组件移出前的状态（用于组件复位）
   - `active`：用于动画过程，可配置动画持续时间、动画类型等

## ❉ 其他坑

1. 关于用户屏幕缩放

   因应用可能用于移动设备，为防止用户误操作导致页面被缩放，可采用如下设置：

   ``` html
   <!-- index.html -->
   
   
   <meta name="viewport" content="width=device-width,initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
   ```

2. 关于页面字体大小

   实现动态处理页面字体大小，防止字体过小或过大的问题，提高用户体验

   ``` javascript
   // App.vue //
   
   // 配置页面字体大小
   document.addEventListener('DOMContentLoaded', () => {
     const html = document.querySelector('html');
   
     let fontSize = window.innerWidth / 10;
     fontSize = fontSize > 50 ? 50 : fontSize;
     console.log(fontSize);
     html.style.fontSize = `${fontSize}px`;
   })
   ```

3. 关于像素

   像素（Pixel）在不同设备中的真实大小是不一致的，为了解决样式在不同设备中可能出现渲染错误的问题，引入了 *rem* 这种相对大小单位，并采用如下函数实现 `px -> rem ` 的转换：

   ```scss
   // SCSS //
   
   // 1rem = fontSize px
   // 1px = (1 / fontSize)rem
   $fontSize: 37.5;
   @function px2rem($px) {
     @return ($px / $fontSize) + rem;
   }
   ```

4. 关于代码规范

   使用 *Eslint* 进行代码规范检查，对于部分规则进行关闭，以符合个人编码习惯：

   ``` javascript
   // .eslintrc.js //
   
   'space-before-function-paren': 'off',
   'quote-props': 'off',
   'comma-dangle': 'off'
   ```

   
