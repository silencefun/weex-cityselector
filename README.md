 

## getting start

```bash
npm install
```

## file structure

* `src/*`: all source code
* `app.js`: entrance of the Weex page
* `build/*`: some build scripts
* `dist/*`: where places generated code
* `assets/*`: some assets for Web preview
* `index.html`: a page with Web preview and qrcode of Weex js bundle
* `weex.html`: Web render
* `.babelrc`: babel config (preset-2015 by default)
* `.eslintrc`: eslint config (standard by default)

## npm scripts

```bash
# build both two js bundles for Weex and Web
npm run build

# build the two js bundles and watch file changes
npm run dev

# start a Web server at 8088 port
npm run serve

# start weex-devtool for debugging with native
npm run debug
```

## notes
改进部分：
1.增加是否显示县区级单位控制。
2.增加直辖市返回是否 显示三级样式。
3.修改数据源排列，直辖市放到最前面。
4.补全数据源中台湾省（只有市，没有县）。
5.修改源码中一些可能空指针错误（某些市下面没有区县行政区划）。
6.完善半透明遮罩，而非全局半透明。
7.删除无效引用，补充缺少的变量声明，完善更换上级索引下级索引默认为第一个。
参见 ：https://www.jianshu.com/p/e4880dc1c3ee 

