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
        // ...
        "strict": true,
        "paths": {
            "@/*": ["./src/*"],
            "@@/*": ["./src/.umi/*"]
        }
    },
    "files": [
        "core.ts",
        "sys.ts"
        // ...
    ],
    "include": ["src/**/*"],
    "exclude": ["node_modules", "**/*.spec.ts"]
}
```

## 二、`tsconfig.json`文件

`tsconfig.json`的存在表明当前目录是一个 TypeScript 项目的根目录。该文件用于指出当前项目的顶层文件和编译 TypeScript 项目时需要的编译选项。

TypeScript 是如何决定使用哪个配置文件的呢？

1. 执行`tsc`命令时，若未传入文件，TypeScript 会从当前工作目录中查找名为`tsconfig.json`的文件，如果当前目录中没有，则在父目录中接着寻找，这样逐层往外寻找，直到寻找到名为`tsconfig.json`文件。

2. 执行`tsc`命令时，若未传入文件，但是通过`--project`或者`-p`参数指定了某个具体路径或者某个`.json`文件，这种情况下：如果指定的是目录，TypeScript 会使用指定目录下的`tsconfig.json`文件；如果指定的是具体的`.json`文件，TypeScript 会使用这个`.json`文件。

3. 执行`tsc`命令时，如果传入了具体的文件，TypeScript 会忽略`tsconfig.json`文件。

## 三、属性介绍

`compileOptions`属性可以省略，如果省略，会使用默认值。该属性下具体的选项可以查看[compileOptions](https://www.typescriptlang.org/docs/handbook/compiler-options.html)。

`files`属性可以接收一组文件地址列表，列表中的文件地址可以是绝对地址，也可以是相对地址。

`include`和`exclude`属性可接收一组文件/目录地址列表。地址中可以使用下面这些特殊标记（**_glob-pattern_**）：

-   `*`：表示 0 个或多个字符（不包括目录间的分隔符和目录与文件之间的分隔符，如`path1/path2`中的`/`和`path1/fileName.ext`中的`/`，下同）。

-   `?`：表示任意一个字符（不包括目录间的分隔符）。

-   `**/`：表示任意连续的 0 个或多个目录。

如果 glob pattern 中有末段仅包括`*`或`.*`，那么只会匹配被支持的拓展名对应的文件。默认支持的文件拓展名有`.ts`、`.tsx`和`.d.ts`。如果`allowJs`选项设置为`true`，那么`.js`和`.jsx`也是支持的。

如果`files`和`include`属性都被省略，TypeScript 编译器默认会囊括当前目录和子目录中的所有 typescript 文件（后缀名：`.ts`、`.d.ts`、`.tsx`），但不包括`exclude`属性指定的要排除的文件。如果`allowJs`设为 true 的话，JS 文件（后缀名：`.js`、`.jsx`）也会默认被囊括在内。

如果`files`和`include`属性中有一个被省略了，另一个没被省略，或者都没被省略，TypeScript 编译器会囊括二者的并集。

如果`exclude`属性没有显示地指定那些目录、文件要排除，那么默认会将`outDir`编译选项设置的值进行排除。

`include`属性设置的值，可以通过`exclude`属性进行过滤。但是在`files`属性里设置的值，则不会受到`exclude`属性值的影响。

`exclude`属性在被省略的情况下，默认会忽略`node_modules`、`bower_components`、`jspm_packages`目录，以及在`outDir`编译选项中设置的目录。

被`files`和`include`中囊括的文件所引用的其他文件，即便不属于`files`和`include`，也会被囊括。相似地，如果文件`A.ts`引用了文件`B.ts`，且文件`A.ts`没有被排除，那么文件`B.ts`也不会被排除。

需要注意的是，TypeScript 编译器会自动忽略一些它认为可能是编译产物的文件。举个例子，比如说项目囊括了`index.ts`文件，那么同目录层级下的`index.d.ts`和`index.js`文件就会被忽略（这个例子中其他的文件的扩展名彼此不同，但去掉扩展名之后都是相同的`index`）。通常来说，最好不要在同一个目录中存放多个仅扩展名不同的**_“同名”_**文件。

还需要注意的是，如果同时在命令行和`tsconfig.json`文件中指定了同一个配置项，那么命令行中设置的值的优先级更高。

**`@types`、`typeRoots`和`types`**

默认情况下，所有*“可见的”*`@types`包都会被 TypeScript 编译器囊括在内。当前项目外层目录中的`node_modules/@types`目录也被认为是*“可见的”*。具体点的话，就是说`./node_modules/@types/`、`../node_modules/@types`、`../../node_modules/@types/`等目录中的 npm 包，都是*“可见的”*。

如果在`compilerOptions.typeRoots`中指定了具体的目录名，那么上面说的`@types`就需要换成相应的指定名。比如下面这个例子：

```json
{
    "compilerOptions": {
        "typeRoots": ["./typings"]
    }
}
```

在上面这个配置下，TypeScript 编译器只会囊括`./typings`目录下的 npm 包，`./node_modules/@types`目录下的包就不会被囊括了。

如果配置了`compilerOptions.types`选项，则只有在该选项汇总指定的包名才会被囊括，即这是一个包名白名单的配置项。比如：

```json
{
    "compilerOptions": {
        "types": ["node", "lodash", "express"]
    }
}
```

在这个配置项下，TypeScript 编译器只会去囊括`./node_modules/@types/node`、`./node_modules/@types/lodash`和`./node_modules/@types/express`。其他`node_modules/@types/*`包不会被囊括在内。

类型定义包（types package）是一个含有名为`index.d.ts`文件的文件夹，或是一个含有带`types`字段的*package.json*文件的文件夹。

配置`"types": []`，可以禁掉自动囊括各种`@types`包的特性。

**使用`extends`属性进行配置的继承**

`extends`和`compilerOptions`、`files`、`include`、`exclude`选项一样，都是最外层的配置属性。它的属性值是一个字符串，这个字符串应当是一个指向其他配置文件（被继承的配置文件）的路径。如果配置文件 A 通过`extends`属性继承了配置文件 B 的配置，那么配置文件 A 中如果有和配置文件 B 中相同的配置项，配置文件 A 中的配置项会被配置文件 B 中的配置项覆盖。`files`、`include`、`exclude`也是一样。

## 四、参考资料

-   [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)
