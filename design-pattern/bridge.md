# Bridge Pattern
_update Oct 29, 2019_

--

## 1. Introduction
Bridge 起到桥梁作用，用来连接类的**功能层次结构**和**实现层次结构**。

### i. 类的层次结构的两个作用
1. 希望增加新功能时
  > 假设有一个类 Something，当我们想要在 Something 中增加新功能时（想增加一个具体方法），会编写一个Something类的子类，即SomethingGood类，这样就有了一个类的层次结构：
  > ```
  > Something
  >      |
  >      |--SomethingGood
  > ```
  > 这就是为了增加新功能而产生的层次结构。父类具有基本功能，在子类中增加新的功能。  
  > 如果我们想要在 SomethingGood 的基础上增加新的功能，我们可以同样编写一个 SomethigBetter类，这样层次结构就更深了：
  > ```
  > Something
  >    |
  >    |--SomethingGood
  >            |
  >            |--SomethingBetter
  > ```
  > 通常来说，类的层次结构不易过深。

2. 希望增加新实现时
> 在 Template Method 模式讲了抽象类的作用，抽象类声明了一些方法，定义了一些API，然后由子类负责去实现这些抽象方法。父类的任务是通过声明抽象方法的方式定义API，子类的任务是实现抽象方法，这样我们才能编写出具有高可替换性的类。</br>
> 这里其实也可以存在层次结构，例如，当子类 ConcreteClass 实现了父类 AbstractClass 类的抽象方法时，他们之间就构成了一个层次结构：
> ```
>  AbstractClass
>      |
>      |--ConcreteClass
> ```
> 但是这里的类的层次结构并非用于增加新功能，它真正的作用是实现上面所说的父类和子类不同的任务分组，这就是类的实现层次结构。当我们想要以其它方式实现AbstractClass时，例如要实现一个 AnotherConcreteClass 时，类的层次结构会发生一些变化：
> ```
> AbstractClass
>      |
>      |--ConcreteClass
>      |
>      |--AnotherConcreteClass
> ```
