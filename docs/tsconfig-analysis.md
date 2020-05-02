# tsconfig.json 文件分析

## 一、配置内容示例

```json
{
  "compilerOptions": {
    "outDir": "build/dist",
    "module": "esnext",
    "target": "esnext",
    "lib": ["esnext", "dom"],
    "sourceMap": true,
    "baseUrl": ".",
    "jsx": "react",
    "allowSyntheticDefaultImports": true,
    "moduleResolution": "node",
    "forceConsistentCasingInFileNames": true,
    "noImplicitReturns": true,
    "suppressImplicitAnyIndexErrors": true,
    "noUnusedLocals": true,
    "allowJs": true,
    "skipLibCheck": true,
    "experimentalDecorators": true,
    "strict": true,
    "paths": {
      "@/*": ["./src/*"],
      "@@/*": ["./src/.umi/*"]
    }
  },
  "exclude": ["node_modules", "build", "dist", "scripts", "src/.umi/*", "webpack", "jest"]
}
```

## 二、`tsconfig.json`文件

`tsconfig.json`的存在表明当前目录是一个 TypeScript 项目的根目录。该文件用于指出当前项目的顶层文件和编译 TypeScript 项目时需要的编译选项。

TypeScript 是如何决定使用哪个配置文件的呢？

1. 执行`tsc`命令时，若未传入文件，TypeScript 会从当前工作目录中查找名为`tsconfig.json`的文件，如果当前目录中没有，则在父目录中接着寻找，这样逐层往外寻找，直到寻找到名为`tsconfig.json`文件。

2. 执行`tsc`命令时，若未传入文件，但是通过`--project`或者`-p`参数指定了某个具体路径或者某个`.json`文件，这种情况下：如果指定的是目录，TypeScript 会使用指定目录下的`tsconfig.json`文件；如果指定的是具体的`.json`文件，TypeScript 会使用这个`.json`文件。

3. 执行`tsc`命令时，如果传入了具体的文件，TypeScript 会忽略`tsconfig.json`文件。

## 三、参考资料

- [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)
