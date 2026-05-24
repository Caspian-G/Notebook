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
//-g指全局安装 安装在nodejs文件夹下的node_modules 无-g则指项目安装 仅安装在
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

