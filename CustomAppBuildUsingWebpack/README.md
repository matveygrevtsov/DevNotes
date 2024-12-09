# Кастомная сборка приложения с использованием webpack

## Шаг№1: инициализируем npm
```console
npm init
```

## Шаг№2: создаём файл .gitignore
```console
/dist
/node_modules
```

## Шаг№3: устанавливаем webpack
```console
npm install webpack -D -E
npm install webpack-cli -D -E
npm install webpack-dev-server -D -E
npm install html-webpack-plugin -D -E # Чтобы вебпак мог работать с html-файлами
```

## Шаг№4: устанавливаем React
```console
npm install react -E
npm install react-dom -E
npm install @types/react -D -E
npm install @types/react-dom -D -E
```

## Шаг№5: устанавливаем TypeScript
```console
npm install typescript -D -E
npm install ts-loader -D -E # Чтобы вебпак мог билдить ts-файлы
```

## Шаг№6: создаём tsconfig.json
```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "jsx": "react",
    "typeRoots": ["./node_modules/@types", "./src/types/index.d.ts"],
  },
  "include": ["src"]
}
```

## Шаг№7: создаём public/index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <base href="/" />
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

## Шаг№8: создаём src/App.tsx
```ts
import React from "react";

export const App = () => {
  return <h1>Hello, world!</h1>;
};
```

## Шаг№9: создаём src/bootstrap.tsx
```ts
import React from "react";
import ReactDOM from "react-dom/client";
import { App } from "./App";

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

## Шаг№10: создаём src/index.tsx
```ts
import("./bootstrap");
```

## Шаг№11: создаём webpack.config.js
```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const OUTPUT_FOLDER_NAME = path.resolve(__dirname, "dist"); // Папка, куда всё заливаться сбилженный проект.

module.exports = {
  mode: "development",
  entry: "./src/index.tsx",
  output: {
    path: OUTPUT_FOLDER_NAME,
    filename: "bundle.js",
  }, // выходной файл
  resolve: {
    extensions: [".tsx", ".ts", ".js", "jsx"],
  },
  module: {
    rules: [
      {
        test: /\.(ts|tsx|js|jsx)$/,
        exclude: /node_modules/,
        use: "ts-loader",
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
  devServer: {
    static: {
      directory: OUTPUT_FOLDER_NAME,
    },
    port: 3000,
    historyApiFallback: true,
    compress: true,
  },
};
```

## Шаг№12: Добавляем в package.json скрипты для билда и для запуска
```json
{
  "build": "npx webpack --mode production",
  "start": "npx webpack server"
}
```