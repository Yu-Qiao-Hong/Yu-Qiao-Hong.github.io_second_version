---
layout: post
title: "C# Params"
author: "Iverson Hong"
modified: 2018-04-13
tags: [C#]
---
### 使用時機

有一method需帶入int陣列，並會將其陣列結果印出:

~~~c#
void PrintNum(int[] ary)
{
    foreach (int n in ary)
        Console.WriteLine(n.ToString());
}
~~~

Clint在呼叫時需寫:

~~~c#
int[] myAry = {1, 2, 3, 4, 5};
PrintNum(myAry);
~~~

每次client要呼叫method之前，都先需定義一int陣列

~~~c#
int[] myAry2 = {7, 8, 9};
PrintNum(myAry2);
~~~

每次都要重新定義好麻煩喔~~
沒關係!還有別的方法

----------

### Params

修改一下上面的method，再帶入參數前面加上"**params**"

~~~c#
void PrintNum(params int[] ary)
{
    foreach (int n in ary)
        Console.WriteLine(n.ToString());
}
~~~

這個時候client要呼叫時可直接把多個int參數帶入

~~~c#
PrintNum(7, 8, 9);
~~~

一樣可以正常印出，且原來的呼叫方式也不會有任何錯誤發生。

----------

### Params 與其他參數混著用

若是要再多帶參數，必須把params放在**最後面**，這樣compiler才知道params陣列的數量到底為多少。

~~~c#
void PrintNum(string name, params int[] ary)
{
    Console.WriteLine(name);
    foreach (int n in ary)
        Console.WriteLine(n.ToString());
}
~~~

~~~c#
int[] myAry = { 1, 2, 3, 4, 5 };
PrintNum("Iverson", myAry);

PrintNum("Hong", 7, 8, 9);
~~~

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)