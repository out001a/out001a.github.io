---
layout: post
category: "php"
title: "快速排序的PHP实现"
tags: ["php,quick sort,algorithm,array"]
summary: "我只是一条摘要"

---

今天去百度知道面试，被让写一个排序的东西。

面试官的原题：

    给定一个数组$arr，按其中元素的长度排序。

我给出的答案：

    $lens = array_map('strlen', $arr);
    array_multisort($lens, $arr);

多么简洁明快的答案！面试官猛敲键盘，查找`array_multisort`的用法😂，还试图把我带到坑里去（也怪我不太熟，一开始把两个参数写反了）。

在他终于查到之后还不满意，非让我自己写个排序算法实现一遍。
当时脑抽直接脱口而出自己好多年没写过都不会写了😢，进入大公司的大好机会就这样丢掉了。

狼厂的尊严就靠着几道算法题来维护了。就算八辈子用不到，也要让人觉得很牛逼的样子才行。
可是在一家做搜索的公司里接触不到搜索核心的PHP程序员，还是先把PHP本身用好，就不要装逼了吧。

现在正好写个简单的快速排序，复习一下。

{% include github-widget.html git-repo="out001a/php-algorithm" %}
