# yarn2-workspaces-typescript4-webpack5-react16

## Getting Started

```bash
yarn
yarn dev
```

open http://localhost:8080 in your browser

Note: although this project uses Yarn 2, I have disabled the zero-installs functionality for now in order to limit the size of the repo. You can re-enable that funcitonality, if you want, by swapping certain commented out lines in .gitignore.

## DIY:

If you'd like to know how to rebuild this project from scratch, here's how:

### 1. Get Yarn 2 and VSCode to Start Dating

#### 1.1 Install Yarn 2/berry/modern

```bash
npm install -g yarn
yarn set version berry
yarn --version
```

(note: I think you can also set version per vscode workspace)

#### 1.2 Initialize repo

```bash
mkdir merninator3
cd merninator3
yarn init
yarn add typescript ts-node
yarn add -D @types/node eslint prettier
```

#### 1.3 VSCode configuration

```bash
yarn add @yarnpkg/pnpify
yarn pnpify --sdk base
yarn pnpify --sdk vscode
```

1. Create some random .ts file. (`touch index.ts`)
1. Either click notifications in the bottom-right-hand corner or type Ctrl+Shift+P in the .ts file
1. Choose "Select TypeScript Version".
1. Pick "Use Workspace Version".

#### 1.4 TS config

```bash
yarn tsc --init -t ES5
```

1. open tsconfig.json
1. uncomment `"lib": []` and set array to `["dom", "dom.iterable", "esnext"]`
1. uncomment `"jsx": "preserve"`

### 2. Setup project-level worktree

add to project-level package.json:

```json
"private": true,
"workspaces": [ "packages/*" ],
"scripts": {
    "client": "yarn workspace client dev"
  },
```

### 3. Setup client workspace

#### 3.1 Get out pots and pans

```bash
mkdir packages
mkdir packages/client
cd packages/client
yarn init
```

#### 3.2 Gather ingredients

```bash
yarn add react react-dom
yarn add -D @types/react @types/react-dom @types/webpack html-webpack-plugin ts-loader typescript webpack@next webpack-cli webpack-dev-server
```

Restart VSCode so it learns about these type definitions ☹️

#### 3.3 Prepare ingredients

- add to client workspace-level package.json:

```json
"scripts": {
   "build": "webpack --config webpack.config.js",
   "webpack-dev-server": "webpack-dev-server",
   "dev": "webpack-dev-server --mode=development",
   "prod": "webpack --mode=production"
},
```

- create Webpack config file

```bash
touch webpack.config.js
```

- Do your Webpacky thing, or copy this

```javascript
const path = require('path');

module.exports = {
    mode: 'none',
    entry: {
        app: path.join(__dirname, 'src', 'index.tsx')
    },
    target: 'web',
    resolve: {
        extensions: ['.ts', '.tsx', '.js']
    },
    module: {
        rules: [
            {
                test: /\.tsx?$/,
                use: 'ts-loader',
                exclude: '/node_modules/'
            }
        ],
    },
    output: {
        filename: '[name].js',
        path: path.resolve(__dirname, 'dist')
    }
}
```

#### 3.4 Start cooking

```bash
mkdir src
touch src/index.tsx
```

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

import App from './App';

ReactDOM.render(<App />, document.querySelector('#root'));

```

```bash
touch src/App.tsx
```

```javascript
import React from 'react';

export default function App()
{
    return <h1>Hello, world!</h1>
}

```

```bash
touch src/index.html
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>React Typescript Webpack</title>
</head>
<body>
    <!-- React app root element -->
    <div id="root"></div>
</body>
</html>

```

#### 3.5 - Put food into dishes and serve

```bash
yarn build
yarn dev
```

open http://localhost:8080 in your browser :)
