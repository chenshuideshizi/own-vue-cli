# 自己实现一个 Vue Cli

#### 创建目录及 package.json


#### bin

创建 bin 文件夹，及 x-build.js

此时项目目录
```
project
  |-  bin
  |     |- own-cli.js
  |-  package.json
```

配置 package.json

```js
"bin": {
  "own-cli": "./bin/own-cli.js"
}
```


**需要的依赖**

- commander 可以解析用户输入的命令。
- download-git-repo  拉取github上的文件。
- chalk 改变输出文字的颜色
- ora  小图标（loading、succeed、warn等）

**优化**

此时下载的文件与github一致，我想改变package.json，将name改为初始化的项目名，将version改为1.0.0。
此时就使用node自己的api就可以做到：

```js
const fs = require('fs');

fs.readFile(`${process.cwd()}/${program.init}/package.json`, (err, data) => {
  if (err) throw err;
  let _data = JSON.parse(data.toString())
  _data.name = program.init
  _data.version = '1.0.0'
  let str = JSON.stringify(_data, null, 4);
  fs.writeFile(`${process.cwd()}/${program.init}/package.json`, str, function (err) {
    if (err) throw err;
  })
});
```
复制代码通过readFile读取文件，writeFile写入文件，写入时注意要传入字符串JSON.stringify(_data, null, 4)，通过这样的方式可以输出格式化的json文件。

