---
title: Typecho文章字数计算
abbrlink: 131
tags:
  - Typecho
date: 2017-02-18 00:30:00
---
炒鸡简单的，只需要在当前使用的主题的根目录的 <cat>functions.php</cat> 插入如下代码

<!--more-->

当前仅仅统计文章中的中文字数。

```php
function  art_count ($cid){
    $db=Typecho_Db::get ();
    $rs=$db->fetchRow ($db->select ('table.contents.text')->from ('table.contents')->where ('table.contents.cid=?',$cid)->order ('table.contents.cid',Typecho_Db::SORT_ASC)->limit (1));
    $text = preg_replace("/[^\x{4e00}-\x{9fa5}]/u", "", $rs['text']);
    echo mb_strlen($text,'UTF-8');
}
```
在需要引用的地方插入

```php
<?php art_count($this->cid); ?>
```

