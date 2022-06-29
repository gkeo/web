# php多态的理解 - 简书
# php 多态的理解

[金星 show](/u/13c67fee988d)关注

2017.12.26 17:31:34 字数 438 阅读 2,019

php 是面向对象的脚本语言，而我们都知道，面向对象的语言具有三大特性：封装，继承，多态。php 理应具有这三大特性。

封装是类的构建过程，php 具有；php 也具有继承的特性。唯独这个多态，php 体现的十分模糊。原因是 php 是弱类型语言。

java 的多态体现的十分清晰，大体分两类：父类引用指向子类对象；接口引用指向实现接口的类对象。java 声明变量时都要给变量设定类型，所以存在什么父类引用和接口引用。而 php 则没有这点体现，php 声明变量不需要给变量设定类型，一个变量可以指向不同的数据类型。所以，php 不具有像 java 一样的多态。

php 不具有像 java 那种清晰的多态，不是代表 php 不具有多态性。看下面一个例子：

    abstract class animal{
        abstract function fun();
    }
    class cat extends animal{
        function fun(){
            echo "cat say miaomiao...";
        }
    }
    class dog extends animal{
        function fun(){
            echo "dog say wangwang...";
        }
    }
    function work($obj){
        if($obj instanceof animal){
            $obj -> fun();
        }else{
            echo "no function";
        }
    }
    work(new dog()); 
    work(new cat()); 

抽象类的实现即可  
上面通过一个关键字 instanceof 来判断，变量指向的对象是否是 animal 类的一个实例，下面 new cat()，new dog()都是 animal 子类的对象，而输出了 “dog say wangwang...” 和“cat say miaomiao...”，说明子类对象是父类的一个实例，从而达到了 java 多态的功能。  
接口类的实现即可：interface implements  
　　上边的类是抽象类，也表明了接口与实现接口的类对象同样可以适用。  
　　至此，得出 php 虽然多态体现模糊，但还是具有多态特性的。

 [https://www.jianshu.com/p/d2fcc7404879](https://www.jianshu.com/p/d2fcc7404879)
