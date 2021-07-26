1. 安装typescript模板

```powershell
 npx create-react-app react-ts-tailcwindcss --template typescript
```

2. 安装tailwindcss相关依赖

```powershell
 yarn add  tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9 --dev
```

3. 安装carco,这个是用于覆盖cra的配置的工具.

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

4. 创建tailwindcss的配置文件

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

5. 在你的css中导入

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
