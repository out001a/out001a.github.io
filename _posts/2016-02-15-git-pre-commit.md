---
layout: post
category: "git"
title: "在git中使用pre-commit"
tags: ["git,pre-commit,php"]
summary: "我只是一条摘要"

---

pre-commit提供了许多有用的钩子，这些钩子在git commit之前被调用，可以做一些提交之前的检查或自动化操作。

* 官网 [http://pre-commit.com/](http://pre-commit.com/)
* php钩子 [https://github.com/hootsuite/pre-commit-php](https://github.com/hootsuite/pre-commit-php)

## 一、安装
* 使用pip （需要python >= 2.7）  
    `pip install pre-commit`

* 用python直接安装  
    `curl https://bootstrap.pypa.io/get-pip.py | sudo python - pre-commit`

* 使用homebrew  
    `brew install pre-commit`

## 二、使用
用pre-commit可以检查php文件的语法、按psr规范格式化文件。以此为例：  

1. 安装PHP_CodeSniffer

    `pear install PHP_CodeSniffer`

2. 在git项目的根目录下创建文件 `.pre-commit-config.yaml`

    ```yaml
    -  repo: https://github.com/hootsuite/pre-commit-php
       sha: 1.2.0
       hooks:
       -   id: php-lint
       -   id: php-cbf
           files: \.(php)$
           args:
           - --standard=PSR2
    ```

    其中包含了2个钩子，php-lint检查php文件的语法，php-cbf按指定的规范格式化文件。

3. 在该目录下执行

    `pre-commit install`

4. 编辑一个php文件，并提交

    ```php
    test.php
    <?php
    if (1)
        {
        echo 3;
     }
     echo 124;
      class Abc {
          $abc= 123;
    }
    ```

    ```bash
    git add .
    git commit -m 'test pre-commit'
    ```

    可以看到pre-commit在终端上输出一段信息，自动修改了不符合规范的php文件，而且本次提交失败。

    ```
    PHP Codesniffer (Code Beutifier and Formatter)...........................Failed
    hookid: php-cbf
    Files were modified by this hook. Additional output:
    Begin PHP Code Beautifier and Fixer ...
    Running command phpcbf --standard=PSR2 test.php
    ```

    打开test.php看看，确实有一些地方被修改了，但是第4和第7行的格式还是有点问题，所以这只是一个辅助检查，我们在写代码的时候还是要注意格式。

    ```php
    test.php
    <?php
    if (1) {
        echo 3;
    }
     echo 124;
    class Abc
    {
        $abc= 123;
    }
    ```

    修改好格式后，再执行git commit，就可以成功提交啦！
