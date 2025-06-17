---
date: '2025-06-06T12:10:20+08:00'
draft: false
title: 'Factory Pattern'
math: true
cascade:
  type: docs
---

## Example

When we meet code like this: We’ve got several concrete classes being instantiated, and the decision of which to instantiate is made **at runtime** depending on some set of conditions.

```java
Pizza orderPizza(String type) {
  Pizza pizza;
  if (type.equals("cheese")) {
    pizza = new CheesePizza();
  } else if (type.equals("greek")) {
    pizza = new GreekPizza();
  } else if (type.equals("pepperoni")) {
    pizza = new PepperoniPizza();
  } else {
    pizza = null; // or handle unknown type
  }
  if (pizza != null) {
    ...
  }
  return pizza;
}
```

{{% details title="What’s wrong with “new”?" closed="true" %}}
问题不在于 new 操作符本身，而在于它会将代码与“具体”的类实现紧密地耦合在一起。

1. 真正的罪魁祸首是“变化” (CHANGE)：当你在代码中直接使用 new 来创建一个具体类的实例时（例如 new Dog()），你的代码就焊死在了 Dog 这个具体的实现上。
2. 违反“开闭原则”：理想的设计应该是“对扩展开放，对修改关闭”。但如果将来系统需要变化，比如要换成使用 Cat 类，你就必须找到所有 new Dog() 的地方，然后修改代码。这就意味着你的代码对于“修改”并不是关闭的。为了扩展功能（增加一个新的类），你被迫要修改旧有的代码。
3. 缺乏灵活性：这种硬编码（hard-coding）的方式使得系统难以适应变化。而文中的解决方案是提倡“面向接口编程”，通过多态来隔离变化，从而让系统在不修改现有代码的情况下，就能适应新的具体类。
{{% /details %}}

We can use the **Factory Pattern** to encapsulate the instantiation logic in a separate class.

```java
public class SimplePizzaFactory {
  public Pizza createPizza(String type) {
    Pizza pizza = null;

    if (type.equals("cheese")) {
      pizza = new CheesePizza();
    } else if (type.equals("pepperoni")) {
      pizza = new PepperoniPizza();
    } else if (type.equals("clam")) {
      pizza = new ClamPizza();
    } else if (type.equals("veggie")) {
      pizza = new VeggiePizza();
    }

    return pizza;
  }
}
```

{{% details title="What’s the advantage of this?"%}}
SimplePizzaFactory may have many clients. All of them can use the same factory to create pizzas, and they don’t need to know about the concrete classes. If we want to add a new type of pizza, we just need to modify the factory class without changing the client code.
{{% /details %}}

{{% details title="Factory need use static method?"%}}
Depends on the context.

If the factory is a utility class that provides a simple way to create objects without maintaining any state, then a static method is appropriate.
However, it can’t be subclassed or have the behavior of the `create(...)` method changed through inheritance.

If the factory needs to maintain state or configuration, then it should be an instance method.
{{% /details %}}

