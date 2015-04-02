---
layout: post
title: TEXT字段想说爱你不容易
category: mysql
description: "mysql中TEXT字段和BLOB字段，不能添加默认值，添加索引有讲究，存取字段受惩罚，这个不一定有很多人注意过，可能自己试过才会发现这些个问题..."
modified: 2015-04-02
tags: [java,mysql]
image:
    background: triangular.png
---

开篇明意，TEXT系列不能设置默认值。这个是我遇到最恶心的地方。

TEXT和BLOB字段作为mysql中较为奇葩，用的也不算多的字段确实是很神奇的存在。

###类型

先说说有哪几个类型吧。BLOB分为*TINYBLOB*,*BLOB*,*MEDIUMBLOBL*,*LONGBLOB*。同样的TEXT字段分为*TINYTEXT*,*TEXT*,*MEDIUMTEXT*,*LONGTEXT*。TEXT与BLOB对应的存储大小是一样的，不过BLOB字段所存储的value是被作为二进制串进行处理，不涉及到字符集等等，在比较时也是作为二进制串进行
比较。TEXT字段则不同，作为字符串被处理。

###存储

在存储的时候BLOB与TEXT不与他们对应的记录存在同一行里，而是新建一个对象存储，在改行纪录里面只存储指向这个纪录的*“指针”*。

>The internal representation of a table has a maximum row size of 65,535 bytes, even if the storage engine is capable of supporting larger rows. This figure excludes BLOB or TEXT columns, which contribute only 9 to 12 bytes toward this size. For BLOB and TEXT data, the information is stored internally in a different area of memory than the row buffer.

看到这里时为什么不能够设置默认值似乎就已经得到了答案。在给TEXT和BLOB设置默认值的时候必然会申请一块新的空间来保存*“默认值”*，这样看来允许设置默认值而大量浪费空间的做法就非常不划算。所以保护存储空间应该是不允许设默认值的第一个考虑。

在存储TEXT和BLOB字段的时候会进行**截断**，多于可存储范围的数据将会被截断并且抛出一个warn，如果被截断的是TEXT字段并且在截断处是连续的字符而不是空格的时候会抛出一个error。这个不是我们乐意看到的，我们最好在存储这两种字段的时候对存储的内容进行一个检测，是不是会超出存储极限。

###索引

mysql索引大法好，什么字段都能无差别加索引，不过给这货加索引还是要注意点。首先要给这货明确要比较的前缀长度。

>When you index a BLOB or TEXT column, you must specify a prefix length for the index.

这么长～的字段不指定比较前缀长度，你确定不是要玩死mysql索引？对于前缀长度内的字段，空格也被算在比较范围内。例如：`"mysql" 与 "mysql "`是不同的。如果你的索引是唯一索引，这个是很需要注意的～

###读取

在读取这个字段的时候，如果你不确定一定需要，请不要去读这个字段。为啥呢？

>Instances of BLOB or TEXT columns in the result of a query that is processed using a temporary table causes the server to use a table on disk rather than in memory because the MEMORY storage engine does not support those data types

原来，每次读取这个字段的时候都要生成一张在***硬盘***里面的临时表。这就是第二个不让设置默认值的原因。因为习惯上往往喜欢用`select * from XXX`这样的做法，如果没有注意到这个字段导致读取时的区别，可能会因为这一个字段的短短的默认值去硬盘里面建临时表并读取。

这下看到不给TEXT和BLOB字段设置默认值，没脾气了。原来你爱我爱的这么深沉。


