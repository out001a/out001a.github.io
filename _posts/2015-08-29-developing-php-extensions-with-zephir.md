---
layout: post
category: "php"
title: "使用zephir语言开发PHP扩展"
tags: [php,extension,zephir]
summary: "我只是一条摘要"

---

[Zephir](http://www.zephir-lang.com/install.html)是专门用来开发PHP扩展的一种领域特定语言（DSL），是[Phalcon](https://phalconphp.com/zh)团队为他们的第二版框架开发而专门编写的。废话不多说，直接进入正题。

### 安装
安装过程在其[官网页面](http://www.zephir-lang.com/install.html)有很详细的描述，这里就不再赘述。

### 开发第一个扩展
在Zephir安装完成后，命令行执行`zephir help`可以确认其是否安装成功，以及查看一些常用的选项。  

#### 初始化
进行项目开发的第一件事就是初始化，生成项目的基本骨架。
要开发一个名为`utils`的扩展，就需要在工作空间下执行以下命令来生成所需的项目文件。
```sh
zephir init utils
```

这个命令会在当前文件夹下生成一个`utils`目录以及很多初始化的文件和文件夹。
* `utils/ext/`目录下包含的是编译器生成扩展所需的代码。
* `utils/utils/`目录下放置我们要编写的Zephir源码。
* `utils/config.json`是用来控制Zephir和这个扩展的行为的配置文件，可以这以后慢慢了解。

```
utils/
├── config.json
├── ext
│   └── kernel
│       ├── array.c
│       ├── array.h
│       ├── assert.c
│       ├── assert.h
│       ├── backtrace.c
│       ├── backtrace.h
│       ├── concat.c
│       ├── concat.h
│       ├── debug.c
│       ├── debug.h
│       ├── exception.c
│       ├── exception.h
│       ├── exit.c
│       ├── exit.h
│       ├── extended
│       │   ├── array.c
│       │   ├── array.h
│       │   ├── fcall.c
│       │   └── fcall.h
│       ├── fcall.c
│       ├── fcall.h
│       ├── file.c
│       ├── file.h
│       ├── filter.c
│       ├── filter.h
│       ├── globals.h
│       ├── hash.c
│       ├── hash.h
│       ├── iterator.c
│       ├── iterator.h
│       ├── main.c
│       ├── main.h
│       ├── memory.c
│       ├── memory.h
│       ├── object.c
│       ├── object.h
│       ├── operators.c
│       ├── operators.h
│       ├── output.c
│       ├── output.h
│       ├── persistent.c
│       ├── persistent.h
│       ├── README.md
│       ├── require.c
│       ├── require.h
│       ├── session.c
│       ├── session.h
│       ├── string.c
│       ├── string.h
│       ├── time.c
│       ├── time.h
│       ├── variables.c
│       └── variables.h
└── utils

4 directories, 53 files
```

#### 编写
进入`utils/utils/`目录，创建一个名为`greeting.zep`的文件。
```greeting.zep
namespace Utils;

class Greeting
{
    public static function say()
    {
        return "Hello world!";
    }
}
```

#### 构建
进入`utils/`目录（也就是有`config.json`文件的项目根目录），执行以下命令编译和生成扩展。
```sh
zephir build
```
如果一切顺利，在命令执行完毕后就会看到下面的输出：
```
...
Extension installed!
Add extension=utils.so to your php.ini
Don't forget to restart your web server
```
把`extension=utils.so`这一行加到cli的`php.ini`中，执行`php -m`就应该可以看到有`utils`这个扩展了。  
执行`php -r 'echo utils\Greeting::say(),"\n";'`，如果可以输出`Hello world!`就说明扩展已经安装成功。
