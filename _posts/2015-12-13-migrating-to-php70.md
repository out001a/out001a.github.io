---
layout: post
category: "php"
title: "迁移到PHP7.0的一些事项（扩展兼容方面）"
tags: ["php7"]
summary: "我只是一条摘要"

---

自PHP7在2015年12月3日正式发布以来，很多公司和同学已经享受到了它的性能提升（相比PHP5.4+）所带来的强烈快感。作为一个还有那么点技术追求的小程序员，我也已经迫不及待的跃跃欲试了。

大神们的很多工作帮助我们少走了很多弯路：

* [迁移PHP5.6到PHP7指南](https://github.com/pangee/Migrating-from-PHP5.6.x-to-PHP7.0.x)
* [让PHP7达到最高性能的几个Tips](http://www.laruence.com/2015/12/04/3086.html)
* [让你的PHP7更快(GCC PGO)](http://www.laruence.com/2015/06/19/3063.html)
* [关于PHP7在手机微博服务端上的应用](http://weibo.com/p/1001603918975720381528)

可是由于底层变化的原因，有不少5.x的扩展要么来不及支持要么不打算支持，如果想让用到这些扩展的项目迁移到7就必须要找到好的替代方案了。

### mysql
老旧的[mysql扩展](http://pecl.php.net/package/mysql)早已不建议使用，并且现在终于被彻底废弃了。可是在一些老的项目中还会用到它。幸好还可以在官方的git仓库中找到它，但新的项目一定要使用mysqli或者pdo_mysql哦。

    git clone https://git.php.net/repository/pecl/database/mysql.git

### mongo
官方已经不再更新[mongo扩展](https://pecl.php.net/package/mongo)转而开始新的[mongodb](https://pecl.php.net/package/mongodb)，就不指望使用多年的mongo扩展能支持PHP7了。坑爹的是mongodb扩展与之前mongo扩展的语法并不兼容。再次幸好，有个用PHP实现的mongofill可以代替。唯一的不同是使用composer来安装和管理依赖，而不是安装.so或者.dll扩展。

{% include github-widget.html git-repo="mongofill/mongofill" %}

### redis
PECL官方最新的[redis扩展](https://pecl.php.net/package/redis)（3.0.0及以上版本）已经支持了PHP7。
