# NPM package.json 说明文档

*基于[`package.json(5)`](https://github.com/npm/npm/blob/latest/doc/files/package.json.md)官方文档翻译版本*

## DESCRIPTION 概要

本文档主要介绍`pakage.json`文件需要怎么写。文件格式必须是`JSON`，而不是`JavasScript`里面的`Object`对象。

本文档描述的大部分行为都受[`npm-config(7)`](https://docs.npmjs.com/misc/config)里面的设置影响。

## name - 名称
`name`和`version`是`package.json`中*最重要*的字段。这两个字段的值必须填写，否则（依赖）包就无法被安装（你的包也不能发布）。它们组成了npm包的唯一标识符。包内容变更的同时，`version`值也应该一起变更（不然你的包也发布不了）。

`name`就是你这个包（项目）叫啥名字。

一些规则：
- 必须小于等于214字节，包括作用域（前缀名称）, 例如：`@angular/compiler-cli`；
- 不能以`_`或`.`开头；
- 不能含有大写字母
- `name`会成为`url`的一部分、命令行的参数或者文件夹的名字，所以不能含有`non-URL-safe`字符

一些建议：
- 不要使用和`node`核心模块一样的名称
- 不要在名称中含有"`js`"或者"`node`"，这会假定这是js文件，因为你在写的是`package.json`文件，你可以在`engines`字段中声明引擎。（见下文）
- `name`属性也许会被写在`require()`的参数中(模块引入)，所以最好取一个简短且具有语义化的名字
- 你最好在取名前先访问[npm官网](https://www.npmjs.com/)查看你取的名字是否已被占用（译者注：被占用的情况下你的包是没法发布成功的）

`name`值（译者注：包名）也可以添加一些前缀，例如：`@myorg/mypackage`。详情参见[`npm-scope(7)`](https://docs.npmjs.com/misc/scope)


## version - 版本
`name`和`version`是`package.json`中*最重要*的字段。这两个字段的值必须填写，否则（依赖）包就无法被安装（你的包也不能发布）。它们组成了npm包的唯一标识符。包内容变更的同时，`version`值也应该一起变更（不然你的包也发布不了）。

`version`必须能被[node-semver](https://github.com/isaacs/node-semver)解析，它被捆绑在`npm`的依赖中。（译者注：自己可以执行`npm install semver`）

更多关于版本号和范围的信息参见[`semver(7)`](https://docs.npmjs.com/misc/semver)


## description - 描述
写入一段字符串描述，方便别人使用`npm search`时找到你的包。


## keywords - 关键词
写入一些字符串数组类型的关键词，方便别人使用`npm search`时找到你的包。


## homepage - 主页
项目官网的`url`地址。

## bugs - 错误
填写一个bug提交的`url`地址和（或者）一个邮箱地址，当那些用到你写的包的人遇到问题时，这可以帮助到他们。

它是长这样的：
```
{ 
  "url" : "https://github.com/owner/project/issues",
  "email" : "project@hostname.com"
}
```

`url`和`email`可以任意填或不填，如果只填一个，那`bugs`的值可以直接写成一个字符串而不是对象。

如果填写了`url`地址，那`npm bugs`命令会使用这个`url`地址。


## license - 协议
你应该为你的包指定一个协议，让用户知道他们是如何允许使用的，以及有哪些使用限制。

如果你使用的是类似于`BSD-2-Clause`或者`MIT`这样的通用协议，那么写入一个当前SPDX协议即可。像这样：
```
{ "license" : "BSD-3-Clause" }
```
你可以查阅[SPDX的协议列表中](https://spdx.org/licenses/)。理论上你应该选择一个[OSI](https://opensource.org/licenses/alphabetical)认可的协议。

如果你的包有多个通用许可协议，你可以像这样写一个[SPDX license expression syntax version 2.0 string](https://npmjs.com/package/spdx)
```
{ "license" : "(ISC OR GPL-3.0)" }
```

如果你使用的协议没有被写入到SPDX标识符里面，或者你使用的是一个私有协议，那么你可以像这个一样写入一段字符串：
```
{ "license" : "SEE LICENSE IN <filename>" }
```
然后在包的根目录提供一个名为`<filename>`的文件。

一些旧的包使用license对象或一个`license`属性来包含一个license的数组：

```js
// Not valid metadata
{
  "license" : {
    "type" : "ISC",
    "url" : "http://opensource.org/licenses/ISC"
  }
}

// Not valid metadata
{ 
  "licenses" : [
    { 
      "type": "MIT",
      "url": "http://www.opensource.org/licenses/mit-license.php"
    },
    {
      "type": "Apache-2.0",
      "url": "http://opensource.org/licenses/apache2.0.php"
    }
  ]
}
```
这种风格的写法现在被废弃了，取而代之的是SPDX表达式，像这样：
```
{ "license": "ISC" }
```
```
{ "license": "(MIT OR Apache-2.0)" }
```

最后，如果你不希望授权别人以任何形式使用私有包或未发布的包：
```
{ "license": "UNLICENSED"}
```
还要考虑设置`"private": true`来防止意外发布。


## people fields: author, contributors
"author"是一个人, "contributors"一些人的数组。`person`是一个包含`name`属性和可选`url`和`email`属性的对象，像这样：
```
{
  "name" : "Barney Rubble",
  "email" : "b@rubble.com",
  "url" : "http://barnyrubble.tumblr.com/"
}
```
或者你可以简写成一个字符串，npm会帮你解析它：
```
"Barney Rubble <b@rubble.com> (http://barnyrubble.tumblr.com/)"
```
`email`和`url`都是可选的。

npm也会根据你提供的npm用户信息来设置一个顶级的`maintainers`字段。


## files - 文件
* 译者注：就是你的包包含哪些文件，即以数组的形式写没有`.gitignore`里面的列出的内容。如果你写了`.gitignore`或者`.npmignore`，那么这个字段可以不需要。*

`files`属性的值是一个数组，它包含了你项目中的文件。如果你在数组中写的是一个文件夹名称，那么它将包含改文件夹中的所有文件。（除非它们被另外一个规则所忽略）。

你也可以在包的根目录或者子目录提供一个`.npmignore`文件，这将阻止这些文件被包含进包，即使这些文件被写到了`files`属性值的数组里面。`.npmignore`文件和`.gitignore`的作用一样。

无论你怎么设置，始终会包含这些文件：

* `package.json`
* `README`
* `CHANGES` / `CHANGELOG` / `HISTORY`
* `LICENSE` / `LICENCE`
* `NOTICE`
* `main`属性值里面列出的文件

`README`, `CHANGES`, `LICENSE` & `NOTICE` 这些文件可以有任意的大小写的文件名和拓展名。

相反地，这些文件经常被忽略：

* `.git`
* `CVS`
* `.svn`
* `.hg`
* `.lock-wscript`
* `.wafpickle-N`
* `.*.swp`
* `.DS_Store`
* `._*`
* `npm-debug.log`
* `.npmrc`
* `node_modules`
* `config.gypi`
* `*.orig`

## main - 主入口
`main`属性是一个模块ID，它是程序的主要入口点。也就是说，如果你的包名为`foo`，用户安装了它，并通过`require("foo")`来使用这个模块，那么主模块的`exports`对象将被返回（译者注：`require`返回的内容就是`main`文件里面的`module.exports` 或者 `export`）。

它应该指向模块根目录下的一个模块ID。

对大对数模块而言，它最大的意义就是让包有一个主脚本，通常没有其他意义。

译者举例：
```
{"main": "./src/index.js"}

// src文件夹里面的index.js
export Dialog from './dialog';
export Table from './table';
```

## bin
许多包都有一个或者多个可执行文件希望被安装到`PATH`（译者注：系统路径）。npm使这很容易（事实上，`npm`命令能够被执行也是使用了这个特性）。

要使用它，你需要在`package.json`文件中提供一个`bin`字段，它是一个命令名称和本地文件名称的映射。在安装时，如果是全局安装，npm会将该文件符号链接到`prefix/bin`中，如果是本地安装，它会链接的`./node_modules/.bin/`中。

例如，`myappp`命令可以这样：
```
{ "bin" : { "myapp" : "./cli.js" } }
```
所以，当你安装myapp时，它会创建一个符号链接从`cli.js`脚本到`/usr/local/bin/myapp`。

如果你有一个可执行文件，它的名称应该是包的名称，那么你可以提供它作为一个字符串（译者注：文件路径）。
例如：

```
{
  "name": "my-program",
  "version": "1.2.5",
  "bin": "./path/to/program"
}
```
这和下面的一样：
```
{
  "name": "my-program",
  "version": "1.2.5",
  "bin" : { "my-program" : "./path/to/program" }
}
```
请确保在`bin`中引用的文件以`#!/usr/bin/env node`开头，否则脚本会在没有执行node的情况下启动！

## man

指定一个单独的文件或一个文件名数组，以便被`man`程序找到。

如果只提供了一个文件，那么不管它的实际文件名是什么，它被安装后的名称都是`man <pkgname>`的结果。例如：

```
{
  "name" : "foo",
  "version" : "1.2.3",
  "description" : "A packaged foo fooer for fooing foos",
  "main" : "foo.js",
  "man" : "./man/doc.1"
}
```
那么执行`man foo`命令会链接到`./man/doc.1`这个文件。
如果指定的文件名并未以包名开头，那么他会被加上前缀，像这样：
```
{
  "name" : "foo",
  "version" : "1.2.3",
  "description" : "A packaged foo fooer for fooing foos",
  "main" : "foo.js",
  "man" : [ "./man/foo.1", "./man/bar.1" ]
}
```
那么执行`man foo`和`man foo-bar`会创建文件。
man文件必须以数字结尾，如果它被压缩了，那它有一个可选的`.gz`后缀。这个数字说明了这个文件被安装到了哪个章节中。

```
{
  "name" : "foo",
  "version" : "1.2.3",
  "description" : "A packaged foo fooer for fooing foos",
  "main" : "foo.js",
  "man" : [ "./man/foo.1", "./man/foo.2" ]
}
```
那么执行`man foo`和`man 2 foo`会创建相应的条目。

## directories - 目录
CommonJS [Packages](http://wiki.commonjs.org/wiki/Packages/1.0)规范说明了几种你可以使用一个`directories`对象来标识包的结构，如果你去看[npm's package.json](https://registry.npmjs.org/npm/latest)，你会看到它有doc, lib 和man目录。

将来，这些信息可能会以其他创意方式使用。

### directories.lib
告诉用户库的文件夹位置。lib目录没有什么特别的用处，但是它是有用的元信息。

### directories.bin
如果你在`directories.bin`中声明了一个`bin`目录，那么所有在这个目录的文件都会被添加。

由于`bin`指令的工作方式，同时指定一个`bin`路径和设置`directories.bin`是一个错误。如果你想指定独立的文件，使用`bin`，如果想执行某个文件夹里的所有文件，使用`directories.bin`。

### directories.man
一个包含手册页的文件夹。系统通过遍历这个文件夹来生成一个man的数组。

### directories.doc
把markdown文件放在这里。最终，也许有一天这些将会很好地展示出来。

### directories.example
把示例脚本放在这里。也许某一天它会以一些聪明的方式暴露。

### directories.test
把你的测试放在这里。它目前没有暴露，但可能在未来会。


## repository 仓库 
指定一个你的代码存放地址。这样能帮助到那些想贡献代码的人。如果git仓库是存放在 GitHub上，那么`npm docs`命令能找到你。

像这样写：
```
// git 仓库
"repository": {
  "type": "git",
  "url": "https://github.com/npm/npm.git"
}
```

```
// svn 仓库
"repository": {
  "type": "svn",
  "url": "https://v8.googlecode.com/svn/trunk/"
}
```
URL应该是一个公开可访问的url地址（可能是只读的），它可以直接递交到VCS程序而无需进行任何修改（译者注：即URL是版本管理系统的url地址）。它不应该是一个HTML项目页面的网址。

对于GitHub，GitHub gist，Bitbucket或GitLab仓库，你可以使用与`npm install`相同的快捷方式：
```
"repository": "npm/npm"
```
```
"repository": "gist:11081aaa281"
```
```
"repository": "bitbucket:example/repo"
```
```
"repository": "gitlab:another/repo"
```


## scripts 脚本 
`scripts`属性是一个字典，它包含了包生命周期中的各个环节需要执行的脚本命令。`key`是生命周期时间，`value`是要执行的命令。

更多关于写包脚本的信息参见[`npm-scripts(7)`](https://docs.npmjs.com/misc/scripts)

## config 配置
`config`属性值是一个对象，用于设置在升级过程中持续的包脚本中使用的配置参数 例如，如果一个包以下：

```
{ 
  "name" : "foo",
  "config" : { 
    "port" : "8080"
  } 
}
```
`start`命令就可以引用`npm_package_config_port`这个环境变量，然后用户也可以通过`npm config set foo：port 8001`改写这个配置。

译者举例：
```js
// start 命令对应的js文件（server.js）
const server = http.createServer(function (request, response) {...});
server.listen(process.env.npm_package_config_port)
```

更多关于包设置的信息参见[`npm-config(7)`](https://docs.npmjs.com/misc/config) 和 [`npm-scripts(7)`](https://docs.npmjs.com/misc/scripts)

## dependencies 依赖关系（依赖包）
`dependencies`属性被声明在一个简单的对象中，用来控制（依赖包）包名在一定的版本范围内。版本范围是一个字符串，可以被一个或多个空格分割。`dependencied`也可以被指定为一个压缩包地址或者一个 `git URL` 地址。

**不要把测试工具或transpilers转义器(babel, webpack, gulp, postcss...)写到`dependencies`中。** （这些应该写到`devDependencies`）配置中，参见下文的`devDependencies`

更多关于怎样声明版本范围的详细信息可以参考[semver(7)](https://docs.npmjs.com/misc/semver)

* `version` 精确匹配指定版本
* `>version` 大于版本
* `>=version` 大于等于版本
* `<version` 小于版本
* `<=version` 小于等于版本
* `~version` "约等于版本"，具体规则详见[semver(7)](https://docs.npmjs.com/misc/semver)
* `^version` "兼容版本"，具体规则详见[semver(7)](https://docs.npmjs.com/misc/semver)
* `1.2.x` 1.2.0, 1.2.1, etc., but not 1.3.0
* `http://...` 参见下面的`'URLs as Dependencies'`
* `*` 匹配任何版本
* `""` (空字符串) 和 `*`一样
* `version1 - version2` 等同于 `>=version1 <=version2`.
* `range1 || range2` 范围1或者范围2满足任意一个都行
* `git...` 参见下面的 `'Git URLs as Dependencies'`
* `user/repo` 参见下面的 `'GitHub URLs'`
* `tag` 指定打了发布标签的某个`tag`版本  参见[`npm-dist-tag(1)`](https://docs.npmjs.com/cli/tag)
* `path/path/path` 参见下面的 [Local Paths](#local-paths)

举例，下面的写法都是有效的：
```
{ 
  "dependencies" : {
    "foo" : "1.0.0 - 2.9999.9999",
    "bar" : ">=1.0.2 <2.1.2",
    "baz" : ">1.0.2 <=2.3.4",
    "boo" : "2.0.1",
    "qux" : "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0",
    "asd" : "http://asdf.com/asdf.tar.gz",
    "til" : "~1.2",
    "elf" : "~1.2.3",
    "two" : "2.x",
    "thr" : "3.3.x",
    "lat" : "latest",
    "dyl" : "file:../dyl"
  }
}
```

### URLs as Dependencies - 路径作为依赖
你可以在版本范围的地方声明一个压缩包的URL地址。

压缩包会在安装时下载下来并安装到你的包。


### Git URLs as Dependencies - `git`路径作为依赖

`Git` urls 的格式可以像下面这样：

```
git://github.com/user/project.git#commit-ish
git+ssh://user@hostname:project.git#commit-ish
git+ssh://user@hostname/project.git#commit-ish
git+http://user@hostname/project/blah.git#commit-ish
git+https://user@hostname/project/blah.git#commit-ish
```
`commit-ish`可以是任意标签，哈希值，或者是检出的分支，默认是`master`分支

## GitHub URLs - `GitHub`路径
1.1.65版本后，您可以将`GitHub` urls 写成这样 `"foo"："user / foo-project"`。 与`git` urls 一样，可以包含一个`commit-ish`后缀。 例如：

```
{
  "name": "foo",
  "version": "0.0.0",
  "dependencies": {
    "express": "expressjs/express",
    "mocha": "mochajs/mocha#4727d357ea",
    "module": "user/repo#feature\/branch"
  }
}
```

## Local Paths - 本地路径
（译者注：提供一个本地路径来安装一个本地的模块。也就是说，如果你的项目A依赖另外一个你本地的项目B，那么这个属性就可以帮到你，而没必要将本地项目发布到公共仓库（`npm publish`）。）

从2.0.0版本开始，您可以提供包文件的本地路径。 使用以下任何形式都可以使用`npm install -S`或`npm install --save`来保存本地路径：
```
../foo/bar
~/foo/bar
./foo/bar
/foo/bar
```
    
在这种情况下，它们将被标准化为相对路径并添加到你的`package.json`中。 例如：
```
{
  "name": "baz",
  "dependencies": {
    "bar": "file:../foo/bar"
  }
}
```
这个属性有助于当你不希望打开外部服务器的时候进行离线开发和创建需要npm安装的测试，但在将包发布到公共仓库的时候不应使用。


## devDependencies - 开发依赖
如果有人准备在他的项目中下载并使用你的模块，也许他们并不希望或需要下载一些你在开发过程中使用到的额外的测试或者文档框架。

在这种情况下，最好将这些额外的依赖列在`devDependencies`属性的对象里。

这些依赖会在根目录执行`npm link`或者`npm install`的时候安装，并可以像其他npm配置参数一样被管理。详见[`npm-config(7)`](https://docs.npmjs.com/misc/config)。

对于一些非特定平台的构建步骤，比如将`CoffeeScript`或者一起其他的语言编译成`JavaScript`，就可以用`prepare`脚本去实现，并把需要的依赖包就写在`devDependency`中。

例如：
```
{
  "name": "ethopia-waza",
  "description": "a delightfully fruity coffee varietal",
  "version": "1.2.3",
  "devDependencies": {
    "coffee-script": "~1.6.3"
  },
  "scripts": {
    "prepare": "coffee -o lib/ -c src/waza.coffee"
  },
  "main": "lib/waza.js"
}
```

`prepare`脚本会在发布前运行，这样用户就不用在使用之前自己去完成编译了。在开发模式下（比如本地运行`npm install`），这个脚本也会被执行，这样以便更好地进行测试。


## peerDependencies - 对等依赖
在某些情况下，当一个host不需要`require`依赖包时，你会阐述还有哪些工具或库与这个依赖包兼容。这通常被称为*插件*。值得注意的是，你的模块可能会暴露一个由host文档指定的特定接口。

例如：
```json
{
  "name": "tea-latte",
  "version": "1.3.5",
  "peerDependencies": {
    "tea": "2.x"
  }
}
```

这将确保`tea-latte`这个包只会和`2.x`版本的`tea`一起被安装。执行`npm install tea-latte`可能产生以下关系图：
```bash
├── tea-latte@1.3.5
└── tea@2.2.0
```

**注意：如果没有在依赖树中显式声明比它们更高的依赖版本，那么`npm1.x` 和 `npm2.x` 都将会自动安装`peerDependencies`。在npm的下一个大版本`npm3.x`中，情况将完全不同。你将收到一个警告，告诉你`peerDependency`还没有被安装。** 在npm1和npm2中，这个行为经常会导致混乱，新的npm版本的设计将会极力避免这种情况。

尝试安装一个具有冲突的另一个插件将会导致错误。因此，你必须确保你的插件依赖项版本范围尽可能大，而不是将其锁定到指定的补丁版本上。

假设主机遵循[semver](http://semver.org/)，只改变这个包的主版本将会导致你的插件不可用。 因此，如果你的插件的某个依赖包运行在每个1.x版本下，使用`"^1.0"`或`"1.x"`。如果你需要的功能在1.5.2版本中，使用`">= 1.5.2 < 2"`。


## bundledDependencies - 捆绑依赖
`bundledDependencies`属性定义了在发布包时将捆绑的包名称数组

如果当你需要在本地保存npm包或通过单个文件下载使它们可用时，你可以在`bundledDependencies`属性的数组中指定包名称，并执行`npm pack`命令将包文件打包在压缩包中。

例如：
如果我们定义的`package.json`是这样的：

```
{
  "name": "awesome-web-framework",
  "version": "1.0.0",
  "bundledDependencies": [
    'renderized', 'super-streams'
  ]
}
```
我们可以通过运行`npm pack`来获得`awesome-web-framework-1.0.0.tgz`文件。 这个文件包含`renderized`和`super-streams`的依赖关系，可以通过执行`npm install awesome-web-framework-1.0.0.tgz`来安装在一个新项目中。

这个属性拼写成`"bundleDependencies"`也是有效的（译者注：少了1个`d`）。


## optionalDependencies - 可选依赖
如果一个依赖可以使用，但是你希望在找不到或者无法安装它的情况下npm依然保持运行，那么你可以把这个依赖写在`optionalDependencies`对象中。和`dependencies`对象一样，它是包名称到版本或者url的集合。不同的是，构建失败不会导致安装失败（译者注：这个依赖安装失败不会导致`npm install`失败）。

不过处理缺少依赖依然是你的程序去处理。 例如，像这样：
```
try {
  var foo = require('foo')
  var fooVersion = require('foo/package.json').version
} catch (er) {
  foo = null
}

if ( notGoodFooVersion(fooVersion) ) {
  foo = null
}

// .. 然后在你的代码里 ..
if (foo) {
  foo.doFooThings()
}
```
`optionalDependencies`中的配置将覆盖`dependencies`中同名的配置，因此通常最好只放在一个地方。


## engines - 引擎
你可以指定项目运行的node版本范围，如：
```
{
  "engines": {
    "node" : ">=0.10.3 <0.12"
  }
}
```
    
和`dependencies`一样，如果你不指定版本范围（或者指定为`\*`），那么任何版本的node都可以。

如果你声明了一个`engines`字段，那么npm需要`node`字段在其里面，如果`engines`被省略，npm会假定它在node上运行。

你也可以用`engines`字段来指定哪一个npm版本能更好地安装你的程序，如：
```
{
  "engines" : {
    "npm" : "~1.0.20"
  }
}
```
除非你设置了`engine-strict`属性，否则这个字段仅在你的包作为依赖包时才会发出警告。


## engineStrict
**在npm 3.0.0被干掉了**

所以不用翻译了。

## os - 操作系统
你可以指定你的模块要运行在哪些操作系统中：
```
"os" : [ "darwin", "linux" ]
```

你也可以用黑名单代替白名单，在系统名字前面加上`!`就可以了：
```
"os" : [ "!win32" ]
```

主机的操作系统由`process.platform`判定。

尽管没有这个必要，这个属性还是允许白名单和黑名单同时存在。

## cpu - 处理器
如果你的代码只运行在某些特定cpu架构上，那么你可以指定哪个`cpu`。
```
"cpu" : [ "x64", "ia32" ]
```

和`os`选项一样，你也可以设置黑名单：
```
"cpu" : [ "!arm", "!mips" ]
```

主机的`cpu`由`process.arch`判定。

## preferGlobal - 全局
如果你的包是需要全局安装的命令行应用程序，那么`preferGlobal`的值设置为`true`，以便本地安装的时候发出警告。

它实际上并不能阻止用户在本地安装，但是它有助于防止包不能达到预期效果时所引起一些的问题。


## private - 私有
如果你在`package.json`文件中设置`"private": true`，那么npm将会拒绝发起这个包。

这是一种防止私有模块被无意间发布出去的方法。如果你想确保一个给出的包只发布到一个特定的npm仓库（例如，一个内部仓库），那么你可以配置下面描述的`publishConfig`属性在发布的时候重写`registry`配置参数。

译者注：私有包一定要配置这个参数。

## publishConfig - 发布配置
这是一组在发布时使用的配置值。 当你想设置`tag`, `registry`, `access`，来确保给定包没有被标记为最新的、发布到公共仓库或者默认情况下作用域模块是私有的时候，这会特别方便。

任何配置值都可以被重写，但是当然只有`tag`, `registry`, `access`对发布有影响。

可以被重写的配置选项列表参见[`npm-config(7)`](https://docs.npmjs.com/misc/config)


## DEFAULT VALUES - 默认参数
`npm` 设置了一些默认参数。

* `"scripts": {"start": "node server.js"}`

  如果包的根目录有一个`server.js`文件，那么`npm start`命令将会默认运行`node server.js`。

* `"scripts":{"install": "node-gyp rebuild"}`

  如果包的根目录有一个`binding.gyp`文件，并且你没有定义`install`或者`preinstall`脚本，那么`npm install`命令将会默认使用`node-gyp`进行编译。
 
* `"contributors": [...]`
    
  如果报的根目录有一个`AUTHORS`文件，npm将把每一行视为一个`Name <email>（url）`格式，其中email和url是可选的。 以`#`开头或为空的行将被忽略。


## SEE ALSO - 参考文档

* [semver(7)](https://docs.npmjs.com/misc/semver)
* [npm-init(1)](https://docs.npmjs.com/cli/init)
* [npm-version(1)](https://docs.npmjs.com/cli/version)
* [npm-config(1)](https://docs.npmjs.com/cli/config)
* [npm-config(7)](https://docs.npmjs.com/misc/config)
* [npm-help(1)](https://docs.npmjs.com/cli/help)
* [npm-install(1)](https://docs.npmjs.com/cli/install)
* [npm-publish(1)](https://docs.npmjs.com/cli/publish)
* [npm-uninstall(1)](https://docs.npmjs.com/cli/uninstall)
