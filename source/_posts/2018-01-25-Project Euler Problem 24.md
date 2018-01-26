---
layout: post
title: "Project Euler Problem 24"
date: 2018-01-25
excerpt: "Project Euler 第24题的解题思路"
tags: [Project Euler, php]
comments: true
---

## Problem 24

### 字典序排列
排列指的是将一组物体进行有顺序的放置。例如，3124是数字1、2、3、4的一个排列。如果把所有排列按照数字大小或字母先后进行排序，我们称之为字典序排列。0、1、2的字典序排列是：

012   021   102   120   201   210

数字0、1、2、3、4、5、6、7、8、9的字典序排列中第一百万位的排列是什么？

---
### Lexicographic permutations
Lexicographic permutations
A permutation is an ordered arrangement of objects. For example, 3124 is one possible permutation of the digits 1, 2, 3 and 4. If all of the permutations are listed numerically or alphabetically, we call it lexicographic order. The lexicographic permutations of 0, 1 and 2 are:

012   021   102   120   201   210

What is the millionth lexicographic permutation of the digits 0, 1, 2, 3, 4, 5, 6, 7, 8 and 9?

---

## 解题过程
看到这道题时，我第一反应是把0-9数字的所有排列取出，然后取出第一百万个就行。当初在某人博客（忘记是哪个博客了，当时只记录下了代码，在这里借鉴一下）上看到过用php去写获取排列的函数，如下：
```php
//从$input数组中取$m个数的排列算法
function perm($input,$m)
{
    if($m==1)
    {
        foreach($input as $item)
        {
            $result[]=array($item);
        }
        return $result;
    }
    for($i=0;$i<count($input);$i++)
    {
        $nextinput=array_merge(array_slice($input,0,$i),array_slice($input,$i+1));
        $nextresult=perm($nextinput,$m-1);  
        foreach($nextresult as $one)
        {
            $result[]=array_merge(array($input[$i]),$one);
        }
    }
    return $result;
}
```
当我通过这个函数去获取0-9的所有排列时，问题出来了，执行脚本时会报内存溢出的错误，无法获取到第一百万位的排列。。。

可以看出，这个函数只能获取小数量的所有排列。经过我绞尽脑汁地思考后，还是没能通过php脚本得到想要的结果。花去了大半天的时间，还是没能解除这道题来。就在我要放弃这道题时，我突然想到，不如用数学方式去分析解答这道题，不一会儿功夫还真被我给解出来的，哈哈哈。。。

啰嗦半天了，开始切入正题。我的数学解题思路是，一步一步把每个位置上的数值给取出来。

第一位：
总共10个数，定下第一位

    //其他9个数的排列数有 
    echo 9*8*7*...*2*1; //362880
    echo floor(1000000/362880); //2
    echo 1000000%362880; //274240
    
根据上面计算得出了第一位数为0-9这些数中的第3位，即 2

第二位：
已确定第一位是2，剩余的数则是 0 1 3 4 5 6 7 8 9，在这些数中确定第二位。在此之前已排除了（362880*2）个排列，在剩余排列中取第274240位排列
    
    //其他8个数的排列数有
    echo 8*7*...*2*1; //40320
    echo floor(274240/40320); //6
    echo 274240%40320; //32320

根据上面计算得出第二位数为剩余数的第7位，即7

... ...

以此类推得出第六位 为1

第七位：
已确定前面6位分别为 2 7 8 3 9 1，剩余4位数为0 4 5 6，
    
    //其他3位数的排列数为 
    echo 3*2*1; //6
    echo floor(16/6); //2
    echo 16%6;  //4
    
根据上面计算得出第七位为剩余数的第3位，即 5

第8位：
剩余的数有0 4 6
    
    //其他两位的排列数为
    echo 2*1; //2
    echo floor(4/2); //2
    echo 4%2; //0
    
根据上面计算，取两次两位数的排列，刚好能取到第一百万位，即 第八位为4，最后两位则为 60


根据以上数学分析，再写php脚本就容易多了

```php
function product($num){
    if($num==1) return 1;
    return $num * product($num-1);
}

//取数组$input中数的第$n位排列数
function get_num($input,$n){
    static $fetch_arr = array();
    $len = count($input);
    $m = $len - 1;
    $tmp_num = product($m);

    $index_k = floor($n/$tmp_num);

    if($n%$tmp_num==0){
        $fetch_arr[] = $input[$index_k-1];
        $nextinput = array_merge(array_slice($input, 0,$index_k-1),array_slice($input, $index_k));
        $fetch_arr = array_merge($fetch_arr,array_reverse($nextinput));
        return $fetch_arr;
    }

    $nextinput = array_merge(array_slice($input, 0,$index_k),array_slice($input, $index_k+1));
    $fetch_arr[] = $input[$index_k];
    $nextn = $n%$tmp_num;

    return get_num($nextinput,$nextn);
}

$input = array(0,1,2,3,4,5,6,7,8,9);
$n = 1000000;

$arr = get_num($input,$n);
print_r($arr);
```
其实这道题在Project Euler 所有题目中难度是偏低的，但是想用简单粗暴的方式去解决，性能上会受到限制。当你无法突破这层限制，换种思路去思考，或许就能拨云见日。


> 题目来源网站：[Project Euler](https://projecteuler.net/)

> 题目翻译来源于：[http://pe-cn.github.io/24/](http://pe-cn.github.io/24/)