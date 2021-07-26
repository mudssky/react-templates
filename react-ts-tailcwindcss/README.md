## 01.安装typescript模板

```powershell
 npx create-react-app react-ts-tailcwindcss --template typescript
```

## 02.安装tailwindcss相关依赖

```powershell
 yarn add  tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9 --dev
```

## 03.安装carco,这个是用于覆盖cra的配置的工具.

```shell
npm install @craco/craco
```

在package.json里进行如下的修改

```diff-json
    // ...
    "scripts": {
-     "start": "react-scripts start",
-     "build": "react-scripts build",
-     "test": "react-scripts test",
+     "start": "craco start",
+     "build": "craco build",
+     "test": "craco test",
      "eject": "react-scripts eject"
    },
  }
```

创建`craco.config.js`配置文件

```js
// craco.config.js
module.exports = {
  style: {
    postcss: {
      plugins: [
        require('tailwindcss'),
        require('autoprefixer'),
      ],
    },
  },
}
```

## 04.创建tailwindcss的配置文件

可以用tailwindcss-cli这个命令行程序生成初始的模板

```shell
npx tailwindcss-cli@latest init
```

进行如下的修改

```diff-js
  // tailwind.config.js
  module.exports = {
-   purge: [],
+   purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
    darkMode: false, // or 'media' or 'class'
    theme: {
      extend: {},
    },
    variants: {
      extend: {},
    },
    plugins: [],
  }
```

## 05.在你的css中导入

```css
/* ./src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```



```diff-js
  // src/index.js
  import React from 'react';
  import ReactDOM from 'react-dom';
+ import './index.css';
  import App from './App';
  import reportWebVitals from './reportWebVitals';

  ReactDOM.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
    document.getElementById('root')
  );
```

接下来就可以在react组件中使用了.

## 06.添加@import构建时导入

css本身存在@import语法,不过那个是在浏览器运行时导入的,我们这里引入的插件是为了让css在构建阶段就拼接在一起.

```shell
yarn add postcss-import
```

然后我们添加插件

```js
// craco.config.js
module.exports = {
  style: {
    postcss: {
      plugins: [
        require('postcss-import'),
        require('tailwindcss'),
        require('autoprefixer'),
      ],
    },
  },
}
```

注意,@import只能在文件顶部使用.



## 07.使用未来的css

[postcss-preset-env](https://github.com/csstools/postcss-preset-env) 这个插件包含了css未来的新特性,如果不进行选项的配置,这个插件默认支持到stage2的特性,并且支持所有浏览器.

```shell
# yarn
yarn add postcss-preset-env --dev
```

因为这个插件已经包含了autoprefixer,还有变量定义和嵌套之类的功能,所以那些插件就不用单独安装了

修改后我们的配置文件如下

```js
// craco.config.js
module.exports = {
  style: {
    postcss: {
      plugins: [
        require('postcss-import'),
        require('tailwindcss'),
        require('postcss-preset-env'),
      ],
    },
  },
}

```



## 08.生产优化

TailwindCSS 开发版本是3739.4kB未压缩,

我们通过设置Purge选项,这个选项包含的文件在构建的时候用来匹配用到的类名.

[PurgeCSS](https://purgecss.com/)他会通过正则 (这基本上可以匹配任何由空格、引号或角括号分隔的字符串，包括 HTML 标签、属性、类，甚至是您标记中的实际书面内容。)

```js
  /[^<>"'`\s]*[^<>"'`\s:]/g
```

把其中匹配到的类名保留.

这样最后打包的文件就会变小很多.



所以不要使用连接字符串的形式创建类名,因为那样就会导致匹配不到类名.

## 09.浏览器支持

Tailwind CSS 2.0 不支持任何版本的ie



