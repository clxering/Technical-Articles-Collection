# How to design Classes and Interfaces（如何设计类和接口）

> 转译自：https://examples.javacodegeeks.com

## 1、Introduction

**介绍**

Whatever programming language you are using (and Java is not an exception here), following good design principles is a key factor to write clean, understandable, testable code and deliver（交付） long-living, easy to maintain solutions. In this part of the tutorial we are going to discuss the foundational building blocks which the Java language provides and introduce a couple of design principles, aiming to help you to make better design decisions.

无论你使用什么编程语言（Java 也不例外），遵循良好设计原则是编写干净的、可理解的、可测试的并且交付后能长期使用的代码，以及易于维护的解决方案的关键因素。

More precisely, we are going to discuss interfaces and interfaces with default methods (new feature of Java 8), abstract and final classes, immutable classes, inheritance, composition and revisit a bit the visibility (or accessibility) rules we have briefly touched in part 1 of the tutorial, How to create and destroy objects.

更准确的说，我们准备讨论接口和带有默认方法的接口（Java 8 的新特性），抽象类和终态类，不可变类、继承、组合，并且重新讨论一下本教程第 1 部分中简要介绍的可见性（或可访问性）规则，即如何创建和销毁对象。

## 2、Interfaces

**接口**

In object-oriented programming, the concept（概念） of interfaces forms the basics of contract-driven (or contract-based) development. In a nutshell（简而言之）, interfaces define the set of methods (contract) and every class which claims to support this particular interface must provide the implementation of those methods: a pretty simple, but powerful idea.

在面向对象编程中，接口的概念形成了契约驱动（或基于契约的）开发的基础。简而言之，接口定义了一组方法（契约），并且每个声称支持该特定接口的类都必须提供这些方法的实现：这是一个非常简单但功能强大的想法。

Many programming languages do have interfaces in one form or another, but Java particularly provides language support for that. Let take a look on a simple interface definition in Java.

许多编程语言都有这样或那样的接口，但是 Java 特别提供了语言支持。让我们看一下 Java 中一个简单接口的定义。

```
package com.javacodegeeks.advanced.design;

public interface SimpleInterface {
    void performAction();
}
```

In the code snippet（片段） above, the interface which we named SimpleInterface declares just one method with name performAction. The principal（主要的） differences of interfaces **in respect to**（关于） classes is that interfaces outline what the contact is (declare methods), but do not provide their implementations.

在上面的代码片段中，我们命名为 SimpleInterface 的接口仅仅声明了一个名为 performAction 的方法。接口与类的主要区别在于接口概括了两者的联系是什么（声明方法），但没有提供它们的实现。

However, interfaces in Java can be more complicated（复杂的） than that: they can include nested interfaces, classes, enumerations, annotations (enumerations and annotations will be covered in details in part 5 of the tutorial, How and when to use Enums and Annotations) and constants. For example:

然而，Java 中的接口可能比这更复杂：它们可以包括嵌套的接口、类、枚举、注释（枚举和注释将在本教程的第 5 部分中详细介绍，如何以及何时使用枚举和注释）和常量。例如：

```
package com.javacodegeeks.advanced.design;

public interface InterfaceWithDefinitions {
    String CONSTANT = "CONSTANT";

    enum InnerEnum {
        E1, E2;
    }

    class InnerClass {
    }

    interface InnerInterface {
        void performInnerAction();
    }

    void performAction();
}
```

With this more complicated example, there are a couple of constraints which interfaces implicitly impose with respect to the nested constructs and method declarations, and Java compiler enforces（强制执行） that. First and foremost, even if it is not being said explicitly, every declaration in the interface is public (and can be only public, for more details about visibility and accessibility rules, please refer to section Visibility). As such, the following method declarations are equivalent:

对于这个更复杂的示例，有两个约束，接口隐式地对嵌套构造和方法声明施加约束，Java 编译器强制执行这些约束。首先，也是最重要的，即使没有明确说明，接口中的每个声明都是 public 的（并且只能是 public 的，关于可见性和可访问性规则的更多细节，请参阅可见性部分）。因此，以下方法声明是等价的：

```
public void performAction();
void performAction();
```

Worth to mention that every single method in the interface is implicitly declared as abstract and even these method declarations are equivalent:

值得一提的是，接口中的每个方法都被隐式地声明为抽象的，甚至这些方法声明也是等价的：

```
public abstract void performAction();
public void performAction();
void performAction();
```

As for the constant field declarations, additionally to being public, they are implicitly static and final so the following declarations are also equivalent:

对于常量字段声明，除了标记为 public 外，它们是隐式的静态的和最终态的，所以下面的声明也是等价的：

```
String CONSTANT = "CONSTANT";
public static final String CONSTANT = "CONSTANT";
```

And finally, the nested classes, interfaces or enumerations, additionally to being public, are implicitly declared as static. For example, those class declarations are equivalent as well:

最后，嵌套的类、接口或枚举，除了标记为 public 之外，还被隐式地声明为静态的。例如，这些类声明也是等价的：

```
class InnerClass {
}
static class InnerClass {
}
```

Which style you are going to choose is a personal preference, however knowledge of those simple qualities of interfaces could save you from unnecessary typing.

选择哪种风格是个人的喜好，但是了解这些简单的接口特性可以避免不必要的输入。

## 3、Marker Interfaces

**标记接口**

Marker interfaces are a special kind of interfaces which have no methods or other nested constructs defined. We have already seen one example of the marker interface in part 2 of the tutorial Using methods common to all objects, the interface Cloneable. Here is how it is defined in the Java library:

标记接口是一种特殊的接口，它没有定义任何方法或其他嵌套构造。在本教程的第 2 部分中，我们已经看到了一个标记接口的示例，它使用了所有对象都通用的方法，即接口 Cloneable。下面是它在 Java 库中的定义：

```
public interface Cloneable {
}
```

Marker interfaces are not contracts per se but somewhat useful technique to “attach” or “tie” some particular trait to the class. For example, with respect to Cloneable, the class is marked as being available for cloning however the way it should or could be done is not a part of the interface. Another very well-known and widely used example of marker interface is Serializable:

标记接口本身不是契约，而是某种有用的技术，可以“附加”或“绑定”类的某些特定特性。例如，对于 Cloneable，类被标记为可用于克隆，但是它应该或可能的方式不是接口的一部分。另一个非常著名和广泛使用的标记接口示例是 Serializable:

```
public interface Serializable {
}
```

This interface marks the class as being available for serialization and deserialization, and again, it does not specify the way it could or should be done.

该接口将类标记为可用于序列化和反序列化，并且它没有指定它可以或应该执行的方式。

The marker interfaces have their place in object-oriented design, although they do not satisfy the main purpose of interface to be a contract.

标记接口在面向对象设计中有自己的位置，尽管它们不满足接口作为契约的主要目的。

## 4、Functional interfaces, default and static methods

**功能接口，默认和静态方法**

With the release of Java 8, interfaces have obtained new very interesting capabilities: static methods, default methods and automatic conversion from lambdas (functional interfaces).

随着 Java 8 的发布，接口获得了非常有趣的新功能:静态方法、默认方法和 lambdas（函数接口）的自动转换。

In section Interfaces we have emphasized（强调） on the fact that interfaces in Java can only declare methods but are not allowed to provide their implementations. With default methods it is not true anymore: an interface can mark a method with the default keyword and provide the implementation for it. For example:

在部分接口中，我们强调了一个事实，即 Java 中的接口只能声明方法，但不能提供它们的实现。对于默认方法就不再适用：接口可以用 default 关键字标记方法并为其提供实现。例如：

```
package com.javacodegeeks.advanced.design;
public interface InterfaceWithDefaultMethods {
    void performAction();

    default void performDefaulAction() {
        // Implementation here
    }
}
```

Being an instance（实例） level, defaults methods could be overridden by each interface implementer, but from now, interfaces may also include static methods, for example:

作为一个实例，默认方法可以被每个接口实现者覆盖，但是从现在开始，接口也可以包括静态方法，例如:

```
package com.javacodegeeks.advanced.design;

public interface InterfaceWithDefaultMethods {
    static void createAction() {
        // Implementation here
    }
}
```

One may say that providing an implementation in the interface defeats the whole purpose of contract-based development, but there are many reasons why these features were introduced into the Java language and no matter how useful or confusing they are, they are there for you to use.

有人可能会说，在接口中提供一个实现违背了基于契约开发的全部目的，但是这些特性被引入 Java 语言有很多原因，而且无论它们多么有用或令人困惑，它们都是可供使用的工具。

The functional interfaces are a different story and they are proven to be very helpful add-on to the language. Basically, the functional interface is the interface with just a single abstract method declared in it. The Runnable interface from Java standard library is a good example of this concept:

函数式接口是一个不同的故事，它们被证明是对语言非常有用的附加组件。基本上，函数式接口就是在接口中仅声明一个抽象方法。Java 标准库中的 Runnable 接口就是这个概念很好的例子：

```
@FunctionalInterface
public interface Runnable {
    void run();
}
```

The Java compiler treats functional interfaces differently and is able to convert the lambda function into the functional interface implementation where it makes sense. Let us take a look on following function definition:

Java 编译器以不同的方式对待函数式接口，并能够将 lambda 函数转换为有意义的函数式接口实现。让我们来看看下面的函数定义：

```
public void runMe( final Runnable r ) {
    r.run();
}
```

To invoke this function in Java 7 and below, the implementation of the Runnable interface should be provided (for example using Anonymous classes), but in Java 8 it is enough to pass run() method implementation using lambda syntax:

要在 Java 7 和以下版本中调用此方法，应该提供 Runnable 接口的实现（如使用匿名类），但在 Java 8 中，使用 lambda 语法传递 run()方法就足够了:

```
runMe( () -> System.out.println( "Run!" ) );
```

Additionally, the @FunctionalInterface annotation (annotations will be covered in details in part 5 of the tutorial, How and when to use Enums and Annotations) hints（暗示，提示） the compiler to verify that the interface contains only one abstract method so any changes introduced to the interface in the future will not break this assumption（假设）.

此外，@FunctionalInterface 注释（注释将在本教程的第 5 部分中详细介绍，如何以及何时使用枚举和注释）提示编译器验证该接口只包含一个抽象方法，将来接口引入的任何更改都不能打破这个假设。

## 5、Abstract classes

**抽象类**

Another interesting concept（概念） supported by Java language is the notion（概念） of abstract classes. Abstract classes are somewhat（有些，稍微） similar to the interfaces in Java 7 and very close to interfaces with default methods in Java 8. By contrast to regular（常规的） classes, abstract classes cannot be instantiated but could be subclassed (please refer to the section Inheritance for more details). More importantly, abstract classes may contain abstract methods: the special kind of methods without implementations, much like interfaces do. For example:

Java 语言支持的另一个有趣的概念是抽象类的概念。抽象类有点类似于 Java 7 中的接口，与 Java 8 中具有默认方法的接口非常接近。与常规类不同的是，抽象类不能被实例化，但可以被子类化（有关更多细节，请参阅继承部分）。更重要的是，抽象类可能包含抽象方法：没有实现的特殊类型的方法，就像接口一样。例如：

```
package com.javacodegeeks.advanced.design;

public abstract class SimpleAbstractClass {
    public void performAction() {
        // Implementation here
    }

    public abstract void performAnotherAction();
}
```

In this example, the class SimpleAbstractClass is declared as abstract and has one abstract method declaration as well. Abstract classes are very useful when most or even some part of implementation details could be shared by many subclasses. However, they still leave the door open and allow customizing（定制） the intrinsic（内在） behavior of each subclass by means of abstract methods.

在本例中，SimpleAbstractClass 类被声明为抽象类，并且有一个抽象方法声明。抽象类非常有用，因为少部分、甚至大部分实现细节都可以由多个子类共享。然而，它们仍然开放，允许通过抽象方法定制每个子类的内在行为。

One thing to mention, in contrast（对比） to interfaces which can contain only public declarations, abstract classes may use the full power of accessibility rules to control abstract methods visibility (please refer to the sections Visibility and Inheritance for more details).

值得一提的是，与只能包含 public 声明的接口不同，抽象类可以使用可访问性规则的全部功能来控制抽象方法的可见性（有关详细信息，请参阅可见性和继承部分）。

## 6、Immutable classes

**不可变类**

Immutability is becoming more and more important in the software development nowadays. The rise of multi-core systems has raised a lot of concerns related to data sharing and concurrency (in the part 9, Concurrency（并发性） best practices, we are going to discuss in details those topics). But the one thing definitely emerged: less (or even absence of) mutable state leads to better scalability（可伸缩性） and simpler reasoning（推理） about the systems.

不可变性在当今软件开发中变得越来越重要。多核系统的兴起引起了许多与数据共享和并发相关的关注（在第 9 部分，并发的最佳实践中，我们将详细讨论这些主题）。但有一件事是肯定的：较少（甚至没有）的可变状态会带来更好的可伸缩性和更简单的系统演绎。

Unfortunately, the Java language does not provide strong support for class immutability. However using a combination（结合） of techniques it is possible to design classes which are immutable. First and foremost, all fields of the class should be final. It is a good start but does not guarantee immutability alone.

不幸的是，Java 语言没有提供对类不变性的强大支持。然而，结合一系列技术就可以设计出不可变类。首先，类的所有字段都应该是 final。这是一个良好的开端，但并不能保证一成不变。

```
package com.javacodegeeks.advanced.design;

import java.util.Collection;

public class ImmutableClass {
    private final long id;
    private final String[] arrayOfStrings;
    private final Collection< String > collectionOfString;
}
```

Secondly, follow the proper initialization: if the field is the reference to a collection or an array, do not assign those fields directly from constructor arguments, make the copies instead. It will guarantee that state of the collection or array will not be changed from outside.

其次，遵循适当的初始化原则：如果字段是对集合或数组的引用，不要直接从构造函数参数中对这些字段赋值，而是生成副本。它将确保集合或数组的状态不会被外部更改。

```
public ImmutableClass( final long id, final String[] arrayOfStrings,
        final Collection< String > collectionOfString) {
    this.id = id;
    this.arrayOfStrings = Arrays.copyOf( arrayOfStrings, arrayOfStrings.length );
    this.collectionOfString = new ArrayList<>( collectionOfString );
}
```

And lastly, provide the proper accessors (getters). For the collection, the immutable view should be exposed（暴露） using Collections.unmodifiableXxx wrappers.

最后，提供适当的访问器（getter）。对于集合，不可变视图应该使用 Collections.unmodifiableXxx 之类的包装器公开。

```
public Collection<String> getCollectionOfString() {
    return Collections.unmodifiableCollection( collectionOfString );
}
```

With arrays, the only way to ensure true immutability is to provide a copy instead of returning reference to the array. That might not be acceptable from a practical standpoint as it hugely depends on array size and may put a lot of pressure on garbage collector.

对于数组，确保真正的不变性的唯一方法是提供一个副本，而不是返回对数组的引用。从实践的角度来看，这可能是不可接受的，因为它很大程度上依赖于数组大小，可能会给垃圾收集器带来很大压力。

```
public String[] getArrayOfStrings() {
    return Arrays.copyOf( arrayOfStrings, arrayOfStrings.length );
}
```

Even this small example gives a good idea that immutability is not a first class citizen in Java yet. Things can get really complicated if an immutable class has fields referencing another class instances. Those classes should also be immutable however there is no simple way to enforce that.

即使是这个小示例也给出了一个好主意：在 Java 中，不可变性还不是一等的公民。如果一个不可变的类有引用另一个类实例的字段，那么事情就会变得非常复杂。这些类也应该是不可变的，但是没有简单的方法来实现它。

There are a couple of great Java source code analyzers like FindBugs) and PMD) which may help a lot by inspecting your code and pointing to the common Java programming flaws. Those tools are great friends of any Java developer.

有一些很棒的 Java 源代码分析程序，比如 FindBugs 和 PMD，它们可以通过检查您的代码并指出常见的 Java 编程缺陷来帮助您。这些工具是任何 Java 开发人员的好朋友。

## 7、Anonymous classes

**匿名类**

In the pre-Java 8 era, anonymous classes were the only way to provide in-place class definitions and immediate instantiations. The purpose of the anonymous classes was to reduce boilerplate（样板） and provide a concise and easy way to represent classes as expressions. Let us take a look on the typical old-fashioned way to spawn（大量生产） new thread in Java:

java 8 之前的时代，匿名类是提供 in-place class 定义和立即实例化的唯一方法。匿名类的目的是减少样板文件，并提供一种简洁而简单的方法来将类表示为表达式。让我们看看在 Java 中产生新线程的典型旧式方法：

```
package com.javacodegeeks.advanced.design;

public class AnonymousClass {
    public static void main( String[] args ) {
        new Thread(
            // Example of creating anonymous class which implements
            // Runnable interface
            new Runnable() {
                @Override
                public void run() {
                    // Implementation here
                }
            }
        ).start();
    }
}
```

In this example, the implementation of the Runnable interface is provided in place as anonymous class. Although there are some limitations associated（相关的） with anonymous classes, the fundamental（基本的） disadvantages（缺点） of their usage are a quite verbose（详细的） syntax constructs which Java imposes（强加） as a language. Even the simplest anonymous class which does nothing requires at least 5 lines of code to be written every time.

在本例中，以匿名类的形式提供了 Runnable 接口的实现。虽然匿名类有一些限制，但是它们的使用的基本缺点是 Java 作为一种语言强加了相当详细的语法结构。即使最简单的匿名类什么也不做，每次至少需要编写 5 行代码。

```
new Runnable() {
   @Override
   public void run() {
   }
}
```

Luckily, with Java 8, lambdas and functional interfaces all this boilerplate is about to gone away, finally making the Java code to look truly concise.

幸运的是，因为有了 Java 8 的 lambdas 表达式和函数式接口，所有这些样板文件都将消失，最终使 Java 代码看起来非常简洁。

```
package com.javacodegeeks.advanced.design;

public class AnonymousClass {
    public static void main( String[] args ) {
        new Thread( () -> { /* Implementation here */ } ).start();
    }
}
```

## 8、Visibility

**可见性**

We have already talked a bit about Java visibility and accessibility rules in part 1 of the tutorial（教程）, How to design Classes and Interfaces. In this part we are going to get back to this subject again but in the context of subclassing

我们已经在本教程的第 1 部分讨论了 Java 可见性和可访问性规则，以及如何设计类和接口。在这一部分中，我们将在子类化的背景下再次回到这个主题。

| **Modifier**            | **Package**    | **Subclass**   | **Everyone Else** |
| ----------------------- | -------------- | -------------- | ----------------- |
| **public**              | accessible     | accessible     | accessible        |
| **protected**           | accessible     | accessible     | not accessible    |
| **&lt;no modifier&gt;** | accessible     | not accessible | not accessible    |
| **private**             | not accessible | not accessible | not accessible    |

Different visibility levels allow or disallow the classes to see other classes or interfaces (for example, if they are in different packages or nested in one another) or subclasses to see and access methods, constructors and fields of their parents.

不同的可见性级别允许或不允许类查看其他类或接口（例如，如果它们位于不同的包中或嵌套在另一个包中）或子类，以查看和访问父类的方法、构造函数和字段。

In next section, Inheritance, we are going to see that in action.

在下一节“继承”中，我们将看到它的实际应用。

## 9、Inheritance

**继承**

Inheritance is one of the key concepts of object-oriented programming, serving as a basis of building class relationships. Combined together with visibility and accessibility rules, inheritance allows designing extensible and maintainable class hierarchies.

继承是面向对象编程的关键概念之一，是构建类关系的基础。结合可见性和可访问性规则，继承允许设计可扩展和可维护的类层次结构。

Conceptually, inheritance in Java is implemented using subclassing and the extends keyword, followed by the parent class. The subclass inherits all of the public and protected members of its parent class. Additionally, a subclass inherits the package-private members of the parent class if both reside in the same package. Having said that, it is very important no matter what you are trying to design, to keep the minimal set of the methods which class exposes publicly or to its subclasses. For example, let us take a look on a class Parent and its subclass Child to demonstrate different visibility levels and their effect:

从概念上讲，Java 中的继承是使用子类，即 extends 关键字后加父类实现的。子类继承其父类的所有公共和受保护的成员。此外，如果两个类都驻留在同一个包中，则子类继承父类的包私有成员。尽管如此，无论您试图设计什么，保持类公开或公开其子类的方法的最小集合都是非常重要的。例如，让我们看看一个类父类及其子类，以演示不同的可见性级别及其效果：

```
package com.javacodegeeks.advanced.design;

public class Parent {
    // Everyone can see it
    public static final String CONSTANT = "Constant";

    // No one can access it
    private String privateField;
    // Only subclasses can access it
    protected String protectedField;

    // No one can see it
    private class PrivateClass {
    }

    // Only visible to subclasses
    protected interface ProtectedInterface {
    }

    // Everyone can call it
    public void publicAction() {
    }

    // Only subclass can call it
    protected void protectedAction() {
    }

    // No one can call it
    private void privateAction() {
    }

    // Only subclasses in the same package can call it
    void packageAction() {
    }
}
```

```
package com.javacodegeeks.advanced.design;

// Resides in the same package as parent class
public class Child extends Parent implements Parent.ProtectedInterface {
    @Override
    protected void protectedAction() {
        // Calls parent's method implementation
        super.protectedAction();
    }

    @Override
    void packageAction() {
        // Do nothing, no call to parent's method implementation
    }

    public void childAction() {
        this.protectedField = "value";
    }
}
```

Inheritance is a very large topic by itself, with a lot of subtle details specific to Java. However, there are a couple of easy to follow rules which could help a lot to keep your class hierarchies concise. In Java, every subclass may override any inherited method of its parent unless it was declared as final (please refer to the section Final classes and methods).

继承本身就是一个非常大的主题，有许多 Java 特有的细节。然而，有一些易于遵循的规则可以帮助您保持类层次结构的简洁。在 Java 中，每个子类都可以重写其父类的任何继承方法，除非它被声明为 final（请参阅 final 类和方法一节）。

However, there is no special syntax or keyword to mark the method as being overridden which may cause a lot of confusion. That is why the @Override annotation has been introduced: whenever your intention is to override the inherited method, please always use the @Override annotation to indicate that.

但是，没有特殊的语法或关键字将方法标记为被覆盖，这可能会导致很多混乱。这就是为什么引入了@Override 注释:当您打算重写继承的方法时，请始终使用@Override 注释来表示。

Another dilemma Java developers are often facing in design is building class hierarchies (with concrete or abstract classes) versus interface implementations. It is strongly advised to prefer interfaces to classes or abstract classes whenever possible. Interfaces are much more lightweight, easier to test (using mocks) and maintain, plus they minimize the side effects of implementation changes. Many advanced programming techniques like creating class proxies in standard Java library heavily rely on interfaces.

Java 开发人员在设计中经常面临的另一个难题是构建类层次结构(使用具体的或抽象的类)和接口实现。强烈建议尽可能选择接口而不是类或抽象类。接口更轻量，更易于测试(使用模拟)和维护，并且它们最小化了实现更改的副作用。许多高级编程技术，如在标准 Java 库中创建类代理，严重依赖于接口。

## 10、Multiple inheritance

**多重继承**

In contrast to C++ and some other languages, Java does not support multiple inheritance: in Java every class has exactly one direct parent (with Object class being on top of the hierarchy as we have already known from part 2 of the tutorial, Using methods common to all objects). However, the class may implement multiple interfaces and as such, stacking interfaces is the only way to achieve (or mimic) multiple inheritance in Java.

与 c++和其他一些语言不同的是，Java 不支持多重继承：在 Java 中，每个类都有一个直接的父类（正如我们在本教程的第 2 部分中已经知道的那样，对象类位于层次结构的顶部，使用所有对象共有的方法）。然而，类可能实现多个接口，因此，堆叠接口是在 Java 中实现（或模拟）多重继承的唯一方法。

```
package com.javacodegeeks.advanced.design;

public class MultipleInterfaces implements Runnable, AutoCloseable {
    @Override
    public void run() {
        // Some implementation here
    }

    @Override
    public void close() throws Exception {
       // Some implementation here
    }
}
```

Implementation of multiple interfaces is in fact quite powerful, but often the need to reuse an implementation leads to deep class hierarchies as a way to overcome the absence of multiple inheritance support in Java.

实际上，多接口的实现非常强大，但是重用实现的需求常常导致类层次结构的深入，从而克服 Java 中缺乏多继承支持的问题。

```
public class A implements Runnable {
    @Override
    public void run() {
        // Some implementation here
    }
}
```

```
// Class B wants to inherit the implementation of run() method from class A.
public class B extends A implements AutoCloseable {
    @Override
    public void close() throws Exception {
       // Some implementation here
    }
}
```

```
// Class C wants to inherit the implementation of run() method from class A
// and the implementation of close() method from class B.
public class C extends B implements Readable {
    @Override
    public int read(java.nio.CharBuffer cb) throws IOException {
       // Some implementation here
    }
}
```

And so on… The recent Java 8 release somewhat addressed the problem with the introduction of default methods. Because of default methods, interfaces actually have started to provide not only contract but also implementation. Consequently, the classes which implement those interfaces are automatically inheriting these implemented methods as well. For example:

最近的 Java 8 发行版在一定程度上解决了引入默认方法的问题。由于默认的方法，接口实际上已经开始提供契约和实现。因此，实现这些接口的类也会自动继承这些实现的方法。例如：

```
package com.javacodegeeks.advanced.design;

public interface DefaultMethods extends Runnable, AutoCloseable {
    @Override
    default void run() {
        // Some implementation here
    }

    @Override
    default void close() throws Exception {
       // Some implementation here
    }
}

// Class C inherits the implementation of run() and close() methods from the
// DefaultMethods interface.
public class C implements DefaultMethods, Readable {
    @Override
    public int read(java.nio.CharBuffer cb) throws IOException {
       // Some implementation here
    }
}
```

Be aware that multiple inheritance is a powerful, but at the same time a dangerous tool to use. The well known “Diamond of Death” problem is often cited as the fundamental flaw of multiple inheritance implementations, so developers are urged to design class hierarchies very carefully. Unfortunately, the Java 8 interfaces with default methods are becoming the victims of those flaws as well.

请注意，多重继承是一种强大的工具，但同时也是一种危险的工具。众所周知的“死亡钻石”问题经常被认为是多重继承实现的根本缺陷，因此开发人员需要非常仔细地设计类层次结构。不幸的是，带有默认方法的 Java 8 接口也成为这些缺陷的受害者。

```
interface A {
    default void performAction() {
    }
}

interface B extends A {
    @Override
    default void performAction() {
    }
}

interface C extends A {
    @Override
    default void performAction() {
    }
}
```

For example, the following code snippet fails to compile:

例如，以下代码片段编译失败：

```
// E is not compilable unless it overrides performAction() as well
interface E extends B, C {
}
```

At this point it is fair to say that Java as a language always tried to escape the corner cases of object-oriented programming, but as the language evolves, some of those cases are started to pop up.

在这一点上，可以说 Java 作为一种语言总是试图逃避面向对象编程的极端情况，但是随着语言的发展，其中一些不利情况也开始出现。

## 11、Inheritance and composition

**继承和组合**

Fortunately, inheritance is not the only way to design your classes. Another alternative, which many developers consider being better than inheritance, is composition. The idea is very simple: instead of building class hierarchies, the classes should be composed from other classes.

幸运的是，继承并不是设计类的唯一方法。另一种选择是组合，许多开发人员认为它比继承更好。这个想法很简单：类应该由其他类组成，而不是构建类层次结构。

Let us take a look on this example:

让我们来看看这个例子：

```
public class Vehicle {
    private Engine engine;
    private Wheels[] wheels;
    // ...
}
```

The Vehicle class is composed out of engine and wheels (plus many other parts which are left aside for simplicity). However, one may say that Vehicle class is also an engine and so could be designed using the inheritance.

Vehicle 类由引擎和车轮组成（还有很多其他的部件，为了简单起见，都被放在一边了）。然而，有人可能会说 Vehicle 类也是一个引擎，因此可以使用继承来设计它。

```
public class Vehicle extends Engine {
    private Wheels[] wheels;
    // ...
}
```

Which design decision is right? The general guidelines（指导方针） are known as IS-A and HAS-A principles. IS-A is the inheritance relationship: the subclass also satisfies the parent class specification and a such IS-A variation of parent class. Consequently, HAS-A is the composition relationship: the class owns (or HAS-A) the objects which belong to it. In most cases, the HAS-A principle works better then IS-A for couple of reasons:

哪个设计决策是正确的？一般准则被称为 IS-A 和 HAS-A 原则。IS-A 是继承关系：子类也满足父类规范，这样的 IS-A 是父类的变体。因此，HAS-A 是复合关系：类拥有（或 HAS-A）属于它的对象。在大多数情况下，HAS-A 原则比 IS-A 更好，原因如下:

- The design is more flexible（灵活的） in a way it could be changed
- The model is more stable as changes are not propagating through class hierarchies
- The class and its composites are loosely coupled compared to inheritance which tightly couples parent and its subclasses
- The reasoning about class is simpler as all its dependencies are included in it, in one place

（1）这种设计在某种程度上更加灵活，便于修改
（2）由于更改不通过类层次结构传播，因此模型更加稳定
（3）与父类及其子类紧密耦合的继承相比，类及其组合是松散耦合的
（4）类的推理更简单，因为它的所有依赖项都包含在类中

However, the inheritance has its own place, solves real design issues in different way and should not be neglected. Please keep those two alternatives in mind while designing your object-oriented models.

然而，继承有它自己的位置，同样以不同的方式解决了真正的设计问题，不应该被忽视。在设计面向对象的模型时，请记住这两个原则。

## 12、Encapsulation

**封装**

The concept of encapsulation in object-oriented programming is all about hiding the implementation details (like state, internal methods, etc.) from the outside world. The benefits of encapsulation are maintainability and ease of change. The less intrinsic details classes expose, the more control the developers have over changing their internal implementation, without the fear to break the existing code (a real problem if you are developing a library or framework used by many people).

面向对象编程中封装的概念就是将实现细节（如状态、内部方法等）对外部世界隐藏。封装的好处是可维护性和易于更改。类暴露的内部细节越少，开发人员对更改内部实现的控制就越强，无需担心破坏现有代码（如果您正在开发许多人使用的库或框架，这是一个真正的问题）。

Encapsulation in Java is achieved using visibility and accessibility rules. It is considered a best practice in Java to never expose the fields directly, only by means of getters and setters (if the field is not declared as final). For example:

Java 中的封装是使用可见性和可访问性规则实现的。在 Java 中，不直接公开字段被认为是一种最佳实践，只有通过 getter 和 setter（如果字段未声明为 final）才能公开字段。例如:

```
package com.javacodegeeks.advanced.design;

public class Encapsulation {
    private final String email;
    private String address;

    public Encapsulation( final String email ) {
        this.email = email;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getEmail() {
        return email;
    }
}
```

This example resembles what is being called JavaBeans in Java language: the regular Java classes written by following the set of conventions, one of those being allow the access to fields using getter and setter methods only.

这个示例类似于 Java 语言中的 JavaBeans：常规 Java 类是按照一组约定编写的，其中之一是只允许使用 getter 和 setter 方法访问字段。

As we already emphasized in the Inheritance section, please always try to keep the class public contract minimal, following the encapsulation principle. Whatever should not be public, should be private instead (or protected / package private, depending on the problem you are solving). In long run it will pay off, giving you the freedom to evolve your design without introducing breaking changes (or at least minimize them).

正如我们在继承部分中已经强调的，请始终按照封装原则尽量减少类 public 契约。任何不应该公开的内容都应该是私有的（或者受保护/包私有，这取决于您正在解决的问题）。从长远来看，它会给你带来回报，让你在不引入破坏性变化的情况下自由地改进你的设计（或者最小化破坏）。

## 13、Final classes and methods

**终态类和终态方法**

In Java, there is a way to prevent the class to be subclassed by any other class: it should be declared as final.

在 Java 中，有一种方法可以防止该类被任何其他类子类化：它应该声明为 final。

```
package com.javacodegeeks.advanced.design;

public final class FinalClass {
}
```

The same final keyword in the method declaration prevents the method in question to be overridden in subclasses.

方法声明中使用 final 关键字可防止在子类中覆盖该方法。

```
package com.javacodegeeks.advanced.design;

public class FinalMethod {
    public final void performAction() {
    }
}
```

There are no general rules to decide if class or method should be final or not. Final classes and methods limit the extensibility and it is very hard to think ahead if the class should or should not be subclassed, or method should or should not be overridden. This is particularly important to library developers as the design decisions like that could significantly limit the applicability of the library.

没有通用的规则来决定类或方法是否应该是终态的。终态类和方法限制了可扩展性，很难提前考虑类应该或不应该被子类化，或者方法应该或不应该被重写。这对于库开发人员来说特别重要，因为这样的设计决策可能会极大地限制库的适用性。

Java standard library has some examples of final classes, with most known being String class. On an early stage, the decision has been taken to proactively prevent any developer’s attempts to come up with own, “better” string implementations.

Java 标准库中有一些终态类的例子，其中最著名的是 String 类。在早期阶段，已经采取了积极的措施，以防止任何开发人员试图提出自己的、“更好的”字符串实现。

## 14、What’s next

**接下来是什么**

In this part of the tutorial we have looked at object-oriented design concepts in Java. We also briefly walked through contract-based development, touched some functional concepts and saw how the language evolved over time. In next part of the tutorial we are going to meet generics and how they are changing the way we approach type-safe programming.

在本教程的这一部分中，我们讨论了 Java 中的面向对象设计概念。我们还简要介绍了基于契约的开发，触及了一些功能概念，并看到了语言是如何随着时间而发展的。在本教程的下一部分中，我们将介绍泛型以及它们如何改变我们处理类型安全编程的方式。
