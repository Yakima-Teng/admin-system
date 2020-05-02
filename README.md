# admin-system

> 该项目为一个使用[Ant Design Pro](https://pro.ant.design)创建的后台项目模板。项目技术栈为 ES2015+、React、UmiJS、dva、g2 和 antd4+.

**_请注意，Ant Design Pro 支持范围为 IE10+，开发时需要确保 Node 10+环境。_**

## 一、常用方法

1、安装依赖

```bash
# 在项目根目录下执行
npm install
```

2、本地开发

```bash
npm run start
```

3、正式打包

```bash
npm run build
```

4、检测代码风格

```bash
# 仅检查代码风格
npm run lint

# 检查代码风格的同时，自动修复部分可修复的问题
npm run lint:fix
```

5、跑测试用例

```bash
npm run test
```

## 二、技术栈介绍

- [ant-design-pro](https://pro.ant.design)：一套可用于生产环境的一揽子管理系统模板。

- [react](https://reactjs.org/)：用于构建用户界面的 JavaScript 库。

- [UmiJS](https://umijs.org/)：可拓展的企业级前端应用框架。

- [dva](https://dvajs.com/)：基于 react 和 redux 的轻量级框架。

- [g2](https://g2.antv.vision/)：是一套面向常规统计图表，以数据驱动的高交互可视化图形语法。

- [ant design](https://ant.design/)：企业级产品设计体系。

## 三、常见问题

1. 问题：安装依赖时提示`Unexpected end of JSON input while parsing near`

解决方案：先执行`npm cache clean --force`清除缓存，再重新`npm install`安装依赖。还不行的话尝试切换镜像地址进行安装，如：`npm install --registry=https://registry.npm.taobao.org`。还不行就换成用`yarn`安装。因为 ant design pro 本身模板没有带`package-lock.json`或者`yarn.lock`文件，所以不清楚是依赖没锁定导致我安装时最新的依赖彼此之间有冲突还是因为我用了 nvm 导致的，最终我是通过 yarn 来成功安装的。我把`yarn.lock`文件纳入了 git 版本控制，方便各位以后使用（不可用的话删掉即可）。

## 四、协议/License

无协议，可随意使用，但因使用造成的风险请自行承担。一旦使用本项目，即视为接受本条款。
