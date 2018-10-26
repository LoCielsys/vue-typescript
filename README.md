# vue-typescript
> A Vue.js project 学习使用vue-typescript

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

---

### 总原则

#### 1. 最小依赖引入

由于我个人从Javascript到Typescript的升级，更倾向于平滑顺移，因此，我对新依赖的引入整体保持克制原则，只引入了必要项，以尽量贴近原生vue写法：

- typescript
- ts-loader

以下依赖均未引入：

- vue-class-component：官方维护，学习成本小，但与vuex融合性较差，计划等官方完善对vuex支持后再考虑引入
- vue-property-decorator：非官方维护，一定学习成本
- vuex-class：非官方维护，在 vue-class-component 基础上补充一定vuex支持（支持有限）
- vuex-ts-decorators/vuex-typescript等：非官方维护，学习成本极高

> PS: 后总结，vue官方维护的 vue-class-component 还是很有必要引入的，文末有详细说明。  
PS: tslint-loader由于对vue的支持尚不完美，作为可选项文末有详细说明。

#### 2. 既然用了 Typescript，不到万不得已不用 any!

any 任意类型的存在，在我看来就是个潘多拉魔盒，一旦开启，很容易养成偷懒的习惯，碰到难题就上 any。因此，我的建议是，尽量不要去碰它，除非你无路可走。

---

### Vue-cli 生成项目启用 Typescript

#### 1. 初始化项目

```
vue init webpack <项目名称>
```

#### 2.依赖安装，安装必要依赖。推荐使用 npm 8及以上版本。

```
npm i --save-dev typescript ts-loader

<!--注意：typescript的版本最好与webpack的版本对应-->
// npm i webpack@3 typescript@3 ts-loader@3 awesome-typescript-loader@3 -D
```
#### 3. Webpack 配置
./build/webpack.base.conf.js，作如下改动：  

- entry入口文件main.js改为main.ts：
```
entry: {
  app: './src/main.ts'
}
```

- resolve.extensions添加.ts：
```
resolve: {
  extensions: ['.js', '.ts', '.vue', '.json']
}
```

- module.rules添加.ts解析规则：
```
module: {
  rules: [
    {
      test: /\.tsx?$/,
      loader: 'ts-loader',
      exclude: /node_modules/,
      options: {
        appendTsSuffixTo: [/\.vue$/]
      }
    }
  ]
}
```

#### 4. tsconfig.json

项目根路径下添加文件tsconfig.json，官方推荐配置如下：

```
// tsconfig.json
{
  "compilerOptions": {
    // 与 Vue 的浏览器支持保持一致
    "target": "es5",
    // 这可以对 `this` 上的数据属性进行更严格的推断
    "strict": true,
    // 如果使用 webpack 2+ 或 rollup，可以利用 tree-shake:
    "module": "es2015",
    "moduleResolution": "node"，
    // "experimentalDecorators": true,
  }
}
```

#### 5. vue-shim.d.ts

src目录下添加文件vue-shim.d.ts：

```
declare module "*.vue" {
  import Vue from "vue";
  export default Vue;
}
```

意思是告诉TypeScript *.vue后缀的文件可以交给vue模块来处理。

#### 6. .js 文件重命名为 .ts 文件

从src/main.js开始，包括src/router/index.js等逐一从.js重命名为.ts

> 注意：重命名后对vue文件的import，需添加.vue后缀

因为Typescript默认只识别*.ts文件，不识别*.vue文件

之前：

```
import App from './App'
import HelloWorld from '@/components/HelloWorld'
```

需改为：

```
import App from './App.vue'
import HelloWorld from '@/components/HelloWorld.vue'
```
#### 7. .vue 文件改造

要点：

- <script>标签添加lang="ts"声明
- 使用Vue.extend定义组件

示例：

```
// src/components/HelloWorld.vue
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
})
</script>
```

#### 8. npm run dev


参考：
[由浅入深 webpack （一） 编译 Typescript](http://www.imooc.com/article/details/id/68413)

[Vue2.5+ Typescript 引入全面指南](https://segmentfault.com/a/1190000011853167#articleHeader7)