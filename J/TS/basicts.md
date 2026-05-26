**统一标准** TS / JS 单行行尾加 ;

#### 数据类型

```
let name: string = "张三";
let age: number = 18;
let isStudent: boolean = true;
let list: string[] = ["reading", " coding"];
let tuple: [string, number] = ["hello", 10];
let n: null = null;
let u: undefined = undefined;
//any表示任意类型
let notSure: any = 4;
//联合类型 可以是多种类型之一 如下述代码后续可修改为number类型
let id: string | number = "abc123";
```

**注** js中包含var、let、const，var变量为函数作用域，let变量和const常量为块级作用域，实际开发尽量减少var的使用
