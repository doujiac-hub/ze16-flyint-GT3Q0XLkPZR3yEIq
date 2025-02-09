
## 抽象工厂模式（Abstract Factory Pattern）


**抽象工厂模式**是一种创建型设计模式，它提供了一个接口，用于创建`一组`相关或依赖的对象，而无需指定具体类。它涉及到多个工厂，每个工厂负责`创建一类`相关产品的对象，确保客户端在不需要了解具体类的情况下，能够通过抽象工厂来获得所需的一系列产品。


**组成结构**


* **工厂**：提供创建产品的接口。
* **产品族**：一组相关或依赖的产品。
* **抽象工厂**：定义创建产品的接口。
* **具体工厂**：实现抽象工厂接口，具体地创建产品。
* **抽象产品**：定义一类产品的接口。
* **具体产品**：实现抽象产品接口的具体类。


**意图**：用于创建一系列相关或依赖的对象家族，而无需指定具体的类。


**抽象工厂模式和工厂方法模式的区别**


* **工厂方法模式**是抽象工厂模式的一部分，更加专注于单一产品的创建，且每个具体工厂只负责创建一个产品。
* **抽象工厂模式**则在工厂方法模式的基础上，扩展了创建多个相关产品的功能，并确保这些产品在同一产品族内的一致性和兼容性。


## 简单案例


假设我们需要支持两种家具风格：


* **现代风格**（Modern）
* **经典风格**（Classic）


每种风格下有不同的产品（椅子和沙发）。抽象工厂模式允许我们通过工厂方法来创建这些产品，但具体的产品实现由不同的工厂来处理。


### 类图


![image](https://img2024.cnblogs.com/blog/1209017/202501/1209017-20250106202429606-1515488250.png)


### 1\. 定义抽象产品接口


首先，我们定义家具产品的抽象接口，如**椅子**和**沙发**。



```
// 抽象椅子
public interface Chair {
    void sitOn();
}

// 抽象沙发
public interface Sofa {
    void lieOn();
}

```

### 2\. 定义具体产品


然后我们创建具体产品，它们实现了上述接口，代表不同风格的椅子和沙发。



```
// 经典风格的椅子
public class ClassicChair implements Chair {
    @Override
    public void sitOn() {
        System.out.println("经典风格的椅子");
    }
}
// 经典风格的沙发
public class ClassicSofa implements Sofa {
    @Override
    public void lieOn() {
        System.out.println("经典风格的沙发");
    }
}

// 现代风格的椅子
public class ModernChair implements Chair {
    @Override
    public void sitOn() {
        System.out.println("现代风格的椅子");
    }
}
// 现代风格的沙发
public class ModernSofa implements Sofa {
    @Override
    public void lieOn() {
        System.out.println("现代风格的沙发");
    }
}

```

### 3\. 定义抽象工厂


定义一个抽象工厂，提供创建产品的方法，这些方法分别返回椅子和沙发的实例。



```
// 抽象家具工厂
public interface FurnitureFactory {
    Chair createChair();
    Sofa createSofa();
}

```

### 4\. 定义具体工厂


定义具体工厂，分别实现现代风格工厂和经典风格工厂，它们负责创建对应风格的椅子和沙发。



```
// 现代风格家具工厂
public class ModernFurnitureFactory implements FurnitureFactory {
    @Override
    public Chair createChair() {
        return new ModernChair();
    }

    @Override
    public Sofa createSofa() {
        return new ModernSofa();
    }
}

// 经典风格家具工厂
public class ClassicFurnitureFactory implements FurnitureFactory {
    @Override
    public Chair createChair() {
        return new ClassicChair();
    }

    @Override
    public Sofa createSofa() {
        return new ClassicSofa();
    }
}

```

### 5\.工厂生成器


为了更好管理系列产品和简化使用而进行的进一步封装



```
// 工厂生成器
public class FactoryGenerator {
    private Chair chair;
    private Sofa sofa;

    /**
     * 通过工厂创建家具
     * @param factory 指定生成哪一系列的家具
     */
    public FactoryGenerator(FurnitureFactory factory) {
        chair = factory.createChair();
        sofa = factory.createSofa();
    }

    public void sitOnChair() {
        chair.sitOn();
    }

    public void lieOnSofa() {
        sofa.lieOn();
    }
}

```

### 6\. 测试代码


客户端代码无需关心具体的家具实现，它只依赖于抽象工厂接口，通过不同的工厂来创建所需风格的家具。



```
// 测试类
public class AbstractFactoryDemo {
    public static void main(String[] args) {
        // 创建现代风格的系列家具
        FurnitureFactory modernFactory = new ModernFurnitureFactory();
        FactoryGenerator generator = new FactoryGenerator(modernFactory);
        System.out.println("现代风格家具:");
        generator.sitOnChair();
        generator.lieOnSofa();
        System.out.println("--------------------------------------------");
        // 创建经典风格的系列家具
        FurnitureFactory classicFactory = new ClassicFurnitureFactory();
        generator = new FactoryGenerator(classicFactory);
        System.out.println("经典风格家具:");
        generator.sitOnChair();
        generator.lieOnSofa();
    }

}

```

输出结果



> 现代风格家具:
> 
> 
> 现代风格的椅子
> 
> 
> 现代风格的沙发
> 
> 
> 
> 
> ---
> 
> 
> 经典风格家具:
> 
> 
> 经典风格的椅子
> 
> 
> 经典风格的沙发


客户端只需要依赖于抽象工厂接口，通过调用不同的工厂来创建所需风格的家具。无论是现代风格还是经典风格，客户端的代码都不需要知道家具的具体实现，只需要通过工厂方法来获得所需的家具产品。这种方式保证了系统的灵活性和可扩展性，你可以很容易地增加新的产品类型或新的产品风格，而不需要修改现有代码。


## 抽象工厂的优缺点


### 优点


* **产品族的统一管理**：可以在不修改客户端代码的情况下，轻松切换不同的产品族，例如两种产品的切换只需要更改创建时的类型即可，后面的代码无需变动。
* **隔离具体类**：客户端与具体类解耦，客户端只依赖抽象工厂和抽象产品接口，面向抽象编程所带来的优势。
* **扩展性强**：如果需要增加新的产品类型，只需要增加新的具体工厂和具体产品，而不需要修改客户端代码。
* **保证产品一致性**：抽象工厂确保每个产品族中的产品都兼容，避免了不兼容的产品组合。


### 缺点


* **增加类的数量**：对于每一类产品族，都需要创建一个具体工厂和多个具体产品类，这可能会导致类的数量增加。
* **难以实现灵活的产品组合**：如果产品之间的组合关系比较复杂，抽象工厂模式可能会显得不够灵活。


## 何时使用抽象工厂模式


* 需要创建一系列相关的产品，且这些产品根据需要而有不同的实现时。
* 系统有多种不同风格的产品，但这些产品需要遵循某些共同的接口或父类。
* 需要确保产品族中的对象之间的兼容性，避免不一致的产品组合。


## 总结


该设计模式的特点为**创建一类“产品”和切换不同的产品系列时不影响**使用。


关于对象的具体创建逻辑可以使用`反射`、`动态代理`或者`结合其他创建型设计模式`来完成对象的实例化。引入了“中间者”和面向抽象编程使用了多态的特性，起到了解耦的作用。


抽象工厂模式符合以下设计原则：


* **开闭原则**：通过增加新的产品族来扩展，而不修改现有代码。
* **单一职责原则**：每个工厂和产品只负责创建特定产品族的对象。
* **依赖倒置原则**：客户端依赖于抽象工厂和抽象产品接口，而不是具体实现。
* **里氏替换原则**：可以替换不同的具体工厂，而不影响客户端。
* **接口隔离原则**：每个工厂只提供它所负责的产品接口，避免客户端依赖不需要的接口。


设计模式是面向对象程序设计的一种便于升级和维护的`软件设计思想`，是通过抽象和概念来描述`通用的解决方案`。


使用设计模式为得也是写出好的代码，**好的代码我觉得是这样的**：面向抽象编程可以降低对象之间的耦合度（耦合度），多态带来的灵活切换具体实现（灵活性），在新增功能时不需要修改原有代码（扩展性），每个对象只负责单一工作（高内聚、可重用）等等，这些特性使得的代码易于阅读、维护起来也会很轻松，系统在一次次的变更中依旧保持稳定。


![image](https://img2024.cnblogs.com/blog/1209017/202501/1209017-20250106202453040-980309081.gif)


 [什么是设计模式？](https://github.com)


[单例模式及其思想](https://github.com)


[设计模式\-\-原型模式及其编程思想](https://github.com)


[掌握设计模式之生成器模式](https://github.com)


[掌握设计模式之简单工厂模式](https://github.com)


[掌握设计模式之工厂方法模式](https://github.com)


[掌握设计模式\-\-装饰模式](https://github.com)


[掌握设计模式\-\-组合模式](https://github.com):[豆荚加速器官网](https://baitenghuo.com)




---


[超实用的SpringAOP实战之日志记录](https://github.com)


[2023年下半年软考考试重磅消息](https://github.com)


[通过软考后却领取不到实体证书？](https://github.com)


[计算机算法设计与分析（第5版）](https://github.com)


[Java全栈学习路线、学习资源和面试题一条龙](https://github.com)


[软考证书\=职称证书？](https://github.com)


[软考中级\-\-软件设计师毫无保留的备考分享](https://github.com)


