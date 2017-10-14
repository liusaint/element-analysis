
#### 起因

因为准备看element-ui的源码，会做一些笔记，在源码中写一些注释，甚至对源码进行一些自定义的修改来查看运行效果。
所以element-ui的文件夹不能直接npm install放在node_modules文件夹下面，node_modules文件夹一般是不同步到svn上的，放在里面的话在另一个电脑上可能重新安装项目自己的修改就没有了。
对引入的外部组件，如果要自定义修改，那么都不能放在node_modules下。
过程中遇到的一些问题记录一下。

element-ui贡献指南：
https://github.com/ElemeFE/element/blob/master/.github/CONTRIBUTING.zh-CN.md

#### vue脚手架安装
vue init webpack element-learn

cd element-learn

npm install

npm run dev

一步步来，建好初始的文件夹。

#### 引入element-ui
https://github.com/ElemeFE/element/tree/master
下载一个master分支。在src文件夹下建立一个element-ui文件夹，将下载的分支解压到该文件夹下。

此时尝试在main.js中引入element-ui：
import elementUI from './element-ui';
Vue.use(elementUI);

报错：This relative module was not found:
 ./element-ui in ./src/main.js

这是因为我们直接下载过来的element-ui没有经过编译。
node模块机制从element-ui包的package.json中的读到的入口文件不存在。所以首先要自己编译element-ui源代码。

#### 到element-ui中的操作

cd E:\vue\element-learn\src\element-ui

npm install

npm run dev

然后访问http://localhost:8085
可以看到示例。
跟官网http://element.eleme.io/#/zh-CN
基本是一样的。我们也可以直接到这个示例中去修改组件，查看效果。毕竟实时编译。

然后是编译，也就是生成我们项目中会使用到的文件。

npm run dist

有可能会报错`package.json` does not exist, have you run `lerna init`?
npm install lerna一下。
有时候还会有些别的错误，把node_modules删除了重新npm install就好。

编译完之后我们就看到多了一个lib文件夹。里面就是编译生成的文件。

然后

cd E:\vue\element-learn

npm run dev

报这个：
[BABEL] Note: The code generator has deoptimised the styling of "E:/vue/element-learn/src/element-ui/lib/element-ui.common.js" as it exceeds the max of "500KB".
不过不用管它，这个不影响。
后面还有报错
```JavaScript
* element-ui/lib/button in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/button-group in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/checkbox in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/checkbox-group in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/input in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/input-number in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/locale in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/mixins/emitter in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/mixins/locale in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/mixins/migrating in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/option in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/progress in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/scrollbar in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/select in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/tag in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/tooltip in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/transitions/collapse-transition in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/clickoutside in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/date in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/dom in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/merge in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/popup in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/resize-event in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/scroll-into-view in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/scrollbar-width in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/shared in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/util in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/vdom in ./src/element-ui/lib/element-ui.common.js
* element-ui/lib/utils/vue-popper in ./src/element-ui/lib/element-ui.common.js
```

说明两点：1.element-ui的入口文件找到了。2.入口文件中路径配置可能有问题。

#### 路径问题分析
首先我们到入口文件lib/element-ui.common.js，
发现里面有很多类似这样的模块引入：module.exports = require("element-ui/lib/select");
这个路径比较奇怪的是element-ui这一段是哪里来的，它怎么就知道我的目录是叫这个名字。于是我修改了目录名重新编译，发现这个文件里的路径还是这样。

然后我把名字改回来，把文件夹移动到node_moudule下，import elementUI from 'element-ui';     
发现能正常运行。所以编译的包应该是没有问题的。问题在路径上。

然后文件夹移回去，到element-ui的build文件夹搜索一下element-ui。果然发现了不少element-ui。                                                          

关键在config.js中这里
```JavaScript
exports.alias = {
  main: path.resolve(__dirname, '../src'),
  packages: path.resolve(__dirname, '../packages'),
  examples: path.resolve(__dirname, '../examples'),
  'element-ui': path.resolve(__dirname, '../')
};
```

可以看到element-ui是这个路径的别名。所以element-ui这里的webpack是可以识别它的。而我们项目的的webpack是识别不了它的。

那么放到node_modules文件夹下，为什么可以正常识别这个路径呢，根据nodejs的模块搜索机制，npm install下来的它的文件夹名字也是element-ui。这个时候element-ui不是一个别名。
直接引入node_modules中的模块，引入方式是import elementUI from 'element-ui',那么lib/element-ui.common.js中的'element-ui'就直接对应到element-ui的根目录了。

所以在我们项目的webpack配置webpack.base.conf.js中增加别名配置element-ui，把'element-ui'指向我们项目中element-ui根目录的位置src/element-ui:
```JavaScipt
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
      'element-ui':resolve('src')+'/element-ui'
    }
```

然后重新运行 npm run dev。运行成功。
于是我们的webpack也能正常地识别lib文件夹里文件中'element-ui'的路径了。

#### 结尾
然后如果我们修改了element-ui的源码，只要编译一下就可以正常在我们自己的项目中应用了。
不过还有一点不好的地方就是我们没有对element-ui源码进行实时的编译，我们项目引用的是它编译过后的而不是真正的源代码，也就是没有把它自己的实时编译过程写到我们的项目webpack的配置中来。所以我们项目中npm run dev的时候并不能实时监测到我们对element-ui源码的修改。这一点后面再优化。


对于一些常见项目的脚手架，其实应该好好学习一下。以后再遇到文件路径问题，各种配置问题会得心应手很多。
顺便推书《深入浅出nodejs》《深入理解es6》。






