---
layout: post
title: "C# Decorator Pattern"
author: "Iverson Hong"
modified: 2019-11-03
tags: [C#, Design Pattern]
---

### 定義/使用時機

* 動態的對物件增加額外的職責，且不修改既有的程式。

----------

### UML

![]({{ "/Images/Decorator Pattern/2019-11-03-21-23-50.png" | relative_url }})

<div class="mermaid">
classDiagram

class Component{
    +Operation()
}

class ConcreteComponent{
    +Operation()
}

class Decorator{
    -component: Component
    +Operation()
}

class ConcreteDecoratorA{
    +Operation()
}

class ConcreteDecoratorB{
    +Operation()
}

Component <|-- ConcreteComponent
Component <|-- Decorator
Component <--o  Decorator
Decorator <|-- ConcreteDecoratorA
Decorator <|-- ConcreteDecoratorB
</div>

----------

### 優點

* 使用**合成**的方式動態地增職責，而非只用繼承。

* 具體類別與裝飾類別獨立設計，可依據需求動態增加裝飾的職責，且不需修改原有具體類別，符合開放封閉原則。

* 把類別的核心職責與裝飾功能區分開來。

----------

### 缺點

* 若使用過多的裝飾類別會增加理解難度。

* 使用合成的方式增加職責(多次裝飾對象)，除錯時需逐步檢查，增加除錯難度。

* 若裝飾的順序會影響功能，較不建議使用，理想情況是裝飾類別彼此之間獨立，這樣就可以任意排列組合使用。

----------

### 範例

寫一漢堡類別，允許可動態地增加其多種配料，並計算出最後價錢。

#### UML：

![]({{ "/Images/Decorator Pattern/2019-11-03-23-45-35.png" | relative_url }})

#### 定義一漢堡虛擬類別：
~~~c#
public abstract class Hamburger
{
    public abstract string Name { get; }

    public abstract int Price { get; }
}
~~~

#### 定義兩個基本款漢堡：起司堡、大麥克：
~~~c#
public class Cheeseburger : Hamburger
{
    public override string Name
    {
        get => "Cheeseburger";
    }

    public override int Price
    {
        get => 80;
    }
}

class BigMac : Hamburger
{
    public override string Name
    {
        get => "BigMac";
    }

    public override int Price
    {
        get => 100;
    }
}
~~~

#### 裝飾類別，繼承漢堡類別：
~~~c#
public abstract class BurgerDecoratorBase : Hamburger
{
    protected Hamburger _hamburger;

    public BurgerDecoratorBase(Hamburger hamburger)
    {
        _hamburger = hamburger;
    }
}
~~~

#### 實際要加入漢堡的東西，繼承裝飾類別：
~~~c#
// 加牛肉
public class BeefDecorator : BurgerDecoratorBase
{
    public BeefDecorator(Hamburger hamburger) : base(hamburger)
    {
    }

    public override string Name
    {
        get => _hamburger.Name + ", add Beef";
    }

    public override int Price
    {
        get => _hamburger.Price + 20;
    }
}

// 加生菜
public class LettuceDecorator : BurgerDecoratorBase
{
    public LettuceDecorator(Hamburger hamburger) : base(hamburger)
    {
    }

    public override string Name
    {
        get => _hamburger.Name + ", add Lettuce";
    }

    public override int Price
    {
        get => _hamburger.Price + 5;
    }
}

// 加酸黃瓜
public class PicklesDecorator : BurgerDecoratorBase
{
    public PicklesDecorator(Hamburger hamburger) : base(hamburger)
    {
    }

    public override string Name
    {
        get => _hamburger.Name + ", add Pickles";
    }

    public override int Price
    {
        get => _hamburger.Price + 5;
    }
}
~~~

#### client 端：
~~~c#
Hamburger burger1 = new Cheeseburger(); // 80
burger1 = new BeefDecorator(burger1); // +20
burger1 = new LettuceDecorator(burger1); // +5
burger1 = new PicklesDecorator(burger1); // +5
burger1 = new BeefDecorator(burger1); // +20

Console.WriteLine("burger1 Name: " + burger1.Name);
Console.WriteLine("burger1 Price: " + burger1.Price);
Console.WriteLine("======================================");

Hamburger burger2 = new BigMac(); // 100
burger2 = new LettuceDecorator(burger2); // +5
burger2 = new PicklesDecorator(burger2); // +5

Console.WriteLine("burger2 Name: " + burger2.Name);
Console.WriteLine("burger2 Price: " + burger2.Price);
Console.WriteLine("======================================");
~~~

#### 輸出結果：
```
burger1 Name: Cheeseburger, add Beef, add Lettuce, add Pickles, add Beef
burger1 Price: 130
======================================
burger2 Name: BigMac, add Lettuce, add Pickles
burger2 Price: 110
======================================
```

----------

[[C# 系列文章]](https://yu-qiao-hong.github.io/tags/#C%23)

[[Design Pattern 系列文章]](https://yu-qiao-hong.github.io/tags/#Design+Pattern)