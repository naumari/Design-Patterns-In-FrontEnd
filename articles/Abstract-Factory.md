#### 抽象工厂模式（Abstract Factory）

之前介绍工厂方法模式的时候，介绍它的缺点在于需要实例的越多，相应的子类工厂方法也会越多。那有没有一种模式可以将实例聚合在同一个工厂模式中？

抽象工厂模式（Abstract Factory Pattern）能够解决这个问题。

> 抽象工厂模式能够提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式又称为 Kit模式。（来源设计模式）

您可能会想起来在工厂方法模式实例中介绍的工厂方法模式的变形也解决了这个问题，为什么要引入抽象工厂模式呢？

工厂方法模式针对的是**继承于同一父类的实例**，可以在结构上将其视为同一个等级，而抽象工厂模式则需要面对多个**继承于不同父类但相互关联的实例**。

> 抽象工厂模式（Abstract Factory Pattern）：提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式又称为 Kit 模式。

之前已经创建了 `Cake`，相应的应该存在 `Pack`，它们在数量上应该是一一对应的，且具有一定的关联性（可以视为同一产品族）。

> **产品族**：在抽象工厂模式中，产品族是指由同一个工厂生产的，位于不同产品等级结构中的一组产品。
>
> ----设计模式

```typescript
class Pack {
    private color: string;

    constructor(color: string) {
        this.color = color;
    }
}

```

同时实现抽象工厂类

```typescript
abstract class Factory {
    public abstract produceCake(): Cake;
    public abstract producePack(): Pack;
}

```

考虑具体 `Birthday` 的场景可实现 `BirthdayCakeFactory`

```typescript
class BirthdayCakeFactory extends Factory {
    public produceCake(): BirthdayCake {
        return new BirthdayCake();
    }
    public producePack(): BirthdayCakePack {
        return new BirthdayCakePack();
    }
}

```

如果使用之前的工厂方法模式，你可能需要创建相关的 `CakeFactory` 以及 `PackFactory` 来处理这种情景。

##### 什么时候使用

当你涉及到**不同的实例**，但它们之前**存在一定的关联需要联合使用**时，可以优先考虑抽象工厂设计模式。

##### 优劣势

- 优点：
  - 不同工厂之间虽然都实现了抽象工厂中定义的公共接口，但是每个具体工厂将创建不同实例的创建逻辑维护在了内部，这就使得抽象工厂模式可以实现高内聚低耦合的设计目的。
- 缺点：
  - 不难看出，如果需要增加新的产等级结构会导致抽象工厂的子类都需要进行更改。

##### 实际场景

在前端业务中，抽象工厂模式使用的场景太多了。例如当你需要实现一套通用组件库需要兼顾不同的游览器时，可以选择通过抽象工厂模式来对功能、逻辑进行抽象，在利用不同的工厂函数去处理。

##### 总结

抽象工厂模式可以视作多个工厂方法模式的组合，来处理不同等级产品的问题。

> 当抽象工厂模式中每一个具体工厂类只创建同一等级结构时，抽象工厂模式退化成工厂方法模式。
>
> 而当工厂方法模式中抽象工厂与具体工厂合并，提供一个统一的工厂来创建产品对象，并将创建对象的工厂方法设计为静态方法时，工厂方法模式退化成简单工厂模式。
>
> ----图解设计模式