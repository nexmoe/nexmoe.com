---
title: PHP金字塔算法
abbrlink: 402536618
date: 2020-10-30
---


L1-002 打印沙漏



<!--more-->

## L1-002 打印沙漏 (20分)

本题要求你写个程序把给定的符号打印成沙漏的形状。例如给定17个“*”，要求按下列格式打印

```
*****
 ***
  *
 ***
*****
```

所谓“沙漏形状”，是指每行输出奇数个符号；各行符号中心对齐；相邻两行符号数差2；符号数先从大到小顺序递减到1，再从小到大顺序递增；首尾符号数相等。

给定任意N个符号，不一定能正好组成一个沙漏。要求打印出的沙漏能用掉尽可能多的符号。

## 输入格式:

输入在一行给出1个正整数N（≤1000）和一个符号，中间以空格分隔。

## 输出格式:

首先打印出由给定符号组成的最大的沙漏形状，最后在一行中输出剩下没用掉的符号数。

## 输入样例:

```in
19 *
```

## 输出样例:

```out
*****
 ***
  *
 ***
*****
2
```

## 我的垃圾算法

![L1-002 打印沙漏 (20分)](https://image.gslb.dawnlab.me/e19fb8eab400e17476c57f0846b5660a.png)


```php
<?php
    $arr = explode(' ',rtrim(fgets(STDIN)));
    $n = 0;
    for($x=0; $n<$arr[0]; $x++){
        $t = $x*2-1;
        if($x > 1){
            $n = $n + $t*2;
        }else{
            $n = $n + $t;
        }
    }
    $n = $n - $t*2 + 1;

    $x2 = $x - 1;
    while($x2>0){
        $x2 = $x2-1;
        $x3=$x2*2-1;
        if($x3>0){
            for($spa=$x-$x2-2; $spa>0 ; $spa--){
                echo " ";
            }
        }

        while($x3>0){
            $x3--;
            echo $arr[1];
        }
        if($x2 > 0){
            echo "\n";
        }

    }
    $x2 = 1;
    while($x2<$x-2){
        $x2 = $x2+1;

        for($spa=$x-$x2-2; $spa>0 ; $spa--){
            echo " ";
        }

        for($x3=0; $x3<$x2*2-1; $x3++){
            echo $arr[1];
        }

        echo "\n";
    }

    echo $arr[0] - $n;
?>
```