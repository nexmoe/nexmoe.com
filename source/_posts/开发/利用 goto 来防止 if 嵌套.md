---
title: 利用 goto 来防止 if 嵌套
abbrlink: 1280
cover: https://cdn.jsdelivr.net/gh/nexmoe/image@latest/QQ图片20210415151105.jpg
date: 2018-11-24 18:04:00
---
流程控制中的 if 是比较简单的逻辑判断，但是在多条逻辑判断中很容易造成 if 嵌套，逻辑复杂度较高，让人感到十分混乱。

<!--more-->

万恶的 if 嵌套

```php
<?php
$a = array(
    'state' => '1',
    'notice' => '注册成功！',
);

if ($_GET['username'] != '') {
    if (strlen($_GET['username']) > 4 or !strlen($_GET['username']) < 11) {
        $name = array('root','admin','GETmaster','master','webmaster','mixcm','administrator','sb','shabi');
        if (!in_array($_GET['username'], $name)) {
            if (preg_match("/^[a-zA-Z\s]+$/", $_GET['username'])) {
                $a['state'] = '0';
                $a['notice'] = '用户名必须为英文！';
            }
        } else {
            $a['state'] = '0';
            $a['notice'] = '非法用户名！';
        }
    } else {
        $a['state'] = '0';
        $a['notice'] = '请输入大于4字符，且小于11个字符的用户名！';
    }

} else {
    $a['state'] = '0';
    $a['notice'] = '用户名不能为空！';
}

echo json_encode($a,JSON_UNESCAPED_UNICODE);
```

于是在实际操作中就会想方设法避免以上格式。便有了 goto 和 表数据。

- 逻辑清晰
- 后期修改容易
- 但需要防止 goto 滥用，建议只定义一个，列如本例就只定义了 end

利用 goto 解决（以下代码中有使用到表数据）

```php
<?php
$a = array(
    'state' => '1',
    'notice' => '注册成功！',
);

if ($_GET['username'] == '') {
    $a['state'] = '0';
    $a['notice'] = '用户名不能为空！';
    goto end;
}

if (strlen($_GET['username']) < 4 or strlen($_GET['username']) > 11) {
    $a['state'] = '0';
    $a['notice'] = '请输入大于4字符，且小于11个字符的用户名！';
    goto end;
}

$name = array('root','admin','GETmaster','master','webmaster','mixcm','administrator','sb','shabi');
if (in_array($_GET['username'], $name)) {
    $a['state'] = '0';
    $a['notice'] = '非法用户名！';
    goto end;
}

if (!preg_match("/^[a-zA-Z\s]+$/", $_GET['username'])) {
    $a['state'] = '0';
    $a['notice'] = '用户名必须为英文！';
    goto end;
}

end:
echo json_encode($a,JSON_UNESCAPED_UNICODE);
```
