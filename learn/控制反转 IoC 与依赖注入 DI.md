# 控制反转 IoC 与依赖注入 DI

## 引入

讨论之前，先来简述一下软件对象中的**耦合**

在软件开发过程中，对象之间的耦合关系是一个常见的问题。传统的面向对象设计中，对象之间的依赖通常是硬编码在代码中的，这导致了代码的高耦合度和低灵活性。当一个对象依赖于另一个对象时，它通常会**直接实例化或调用该对象**，这使得两个对象之间的关系非常紧密，难以进行单元测试和代码重用。

例如，考虑一个电商网站的购物车功能。在购物车中直接实例化商品对象可能会导致购物车模块与商品模块之间的紧密耦合，使得**如果需要修改商品类的实现或者替换为其他商品类时，就必须要修改购物车模块的代码**。这种紧耦合的设计不仅使代码难以维护和扩展，还降低了代码的可测试性和可重用性。

将这个例子缩小到类对象之间的调用：对象 A 依赖于对象 B，那么对象 A 在初始化或需要调用 B 的时候，自己必须主动去创建对象 B 或者使用已经创建的对象 B。无论是创建还是使用对象 B，控制权都在自己手上。

为了解决这一问题，控制反转（IoC）和依赖注入（DI）应运而生。它们提供了一种解耦的方法，通过将对象之间的依赖关系转移到外部容器（loC 容器）或框架中管理，实现了对象之间的松耦合。

简言之，通过 IoC 与 DI，调用者将依赖对象的创建初始化逻辑从自身抽离解耦出来，调用者只负责声明调用，依赖对象只负责实现自己，而创建依赖对象的控制权交给了“第三方”（IoC容器）

## 控制反转 loC 

案例：假设你现在需要与外部客户对接某业务，这个业务随着外部客户的不同，对接流程也不同，于是每个客户，都需要制定相应的业务对接流程，每当有了新的客户，不得不继续制定该客户的业务对接流程。显然，这严重耦合并依赖各个不同客户。最终你受不了了，自己制定一个对接方案，开发一个对接平台，各个客户必须遵守你制定对接方案才能进行业务对接，于是各个客户反过来依赖你的对接标准，反转了控制，倒置了依赖。

这个过程中，你就是调用者，客户就是依赖对象，你制定的标准、对接平台就是 IoC 容器。从调用者不得不自身一个个实现对接业务到将对接业务解耦到统一标准中的这一过程，可以理解为控制反转

控制反转是一种思想，其基本思想是：**借助于“第三方”实现具有依赖关系的对象之间的解耦**。

## 依赖注入 DI

以 python 代码为例

未使用依赖注入之前：

```python
class Dependency:
    def __init__(self):
        pass
    
    def operation(self):
        return "Dependency operation"

class Client:
    def __init__(self):
        pass

    def do_something(self):
        # 手动创建 Dependency 对象
        dependency = Dependency()
        return dependency.operation()
```

此时，若 Dependency 的初始化函数发生变化，Client 中手动创建 Dependency 对象的代码也需要改动！

使用依赖注入之后：

```python
class Dependency:
    def __init__(self):
        pass
    
    def operation(self):
        return "Dependency operation"

class Client:
    def __init__(self, dependency):
        # 通过参数的形式将 Dependency 实例传入
        self.dependency = dependency

    def do_something(self):
        return self.dependency.operation()

dependency = Dependency()
client = Client(dependency) # 在 Client 对象中注入 Dependency 对象依赖 
result = client.do_something()
print(result)  
```

此时，Dependency 的初始化函数改变，并不会影响调用者，因为 Dependency 是以对象的形式注入到调用者的



## 应用

后端框架 Spring（java），midway（node.js）都应用了 IoC 思想及 DI 依赖注入。



## 总结

控制反转是一种**借助于“第三方”实现具有依赖关系的对象之间的解耦**的思想。

依赖注入是**实现控制反转的一种方式**，一种设计模式。还有其他的方式如：Service Locator

