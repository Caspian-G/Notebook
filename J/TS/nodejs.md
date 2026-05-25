#### nvm 管理node自身版本

```
nvm --version
//查看可用版本号
nvm list available
//安装 例
nvm install 24.16.0

nvm list
nvm use <version>
nvm current
```

**机制** nvm安装文件夹下是nodejs真实路径 NVM_SYMLINK指定的软链接地址仅是指向真实路径(可切换)

#### JS运行时环境

#### node 运行JS代码引擎(基于V8)

```
node --version
```

#### npm 管理项目第三方依赖包

```
npm  --version
```

npm镜像源设置 C:\Users\lenovo\.npmrc

```
npm config set registry https://registry.npmmirror.com/
```



#### TS

```
//-g指全局安装 安装在nodejs文件夹下的node_modules 无-g则指项目安装 仅安装为项目依赖
npm install -g typescript
tsc --init  //初始化tsconfig.json

//编译为.js
tsc demo.ts
//直接编译项目
tsc

//运行.js
node demo.js
node .\dist\main.js
```

###### tsconfig.json

```
{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {
    // File Layout
    "rootDir": "./src",
    "outDir": "./dist",
    // Environment Settings
    // See also https://aka.ms/tsconfig/module
    "module": "es2022",
    "target": "es2022",
    "types": [],
    // For nodejs:
    // "lib": ["esnext"],
    // "types": ["node"],
    // and npm install -D @types/node
    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,
    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    // Style Options
    // "noImplicitReturns": true,
    // "noImplicitOverride": true,
    // "noUnusedLocals": true,
    // "noUnusedParameters": true,
    // "noFallthroughCasesInSwitch": true,
    // "noPropertyAccessFromIndexSignature": true,
    // Recommended Options
    "strict": true,
    "jsx": "react-jsx",
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  //指定编译文件夹
  "include": ["src/**/*"],
  //指定编译忽略文件夹
  "exclude": ["node_modules", "dist"]
}
```



#### Electron

需安装至项目依赖 最终不应打包进应用即开发依赖(--save-dev)

```
npm install electron --save-dev
npm install typescript --save-dev  //安装为开发项目依赖易于管理版本
```

###### package.json

```
{
  "devDependencies": {
    "electron": "^42.1.0",
    "rimraf": "^6.1.3",
    "typescript": "^6.0.3"
  },
  //定义 npm run ... 命令
  "scripts": {
    "clean": "rimraf dist",
    "build": "npm run clean && tsc",
    "start": "npm run clean && tsc && electron ."
  },
  "main": "dist\\main.js"
}
```

定义完上述.json即可

```
npm run start 运行项目
//或不定义换用npx临时使用本地依赖或联网使用一次依赖(用完即删) 即
//npx rimraf dist
//npx tsc
//npx electron .
```

**注** 安装electron空壳问题 step1 手动下载https://registry.npmmirror.com/-/binary/electron/42.1.0/electron-v42.1.0-win32-x64.zip step2 解压至项目node_module\\electron\\dist下 step3 node_module\electron下创建path.txt写入electron.exe
