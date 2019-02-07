## Java optional parameters（Java中的可选参数）

> 转译自：https://www.javacodegeeks.com/2018/11/java-optional-parameters.html

When you design a method in a Java class, some parameters may be optional for its execution. No matter it is inside a DTO, a fat model domain object, or a simple stateless service class, optional method parameters are common.

在Java类中设计方法时，一些参数可能是可选的。无论是DTO、胖模型域对象还是简单的无状态服务类，可选的方法参数都是常见的。

From this article **you will learn how to handle optional parameters in Java.** We’ll focus on regular method, class constructors with optional fields, and quickly look at bad practices of the discussed topic. We’ll stop for a moment to look at Java 8 Optional and assess（vt. 评定；估价） if it fits our needs.

从本文中，**你将了解如何在Java中处理可选参数。** 我们将重点讨论常规方法、带有可选字段的类构造函数，并快速查看所讨论主题的不良实践。我们会停下来看看Java 8中的Optional类，看看它是否符合我们的需求。

Let’s get started.

让我们开始吧。

### 1. Optional method parameters（可选的方法参数）

You can tackle（vt. 处理；抓住） Java optional parameters in method in several different ways. I’ll walk you through from the simplest to more complex.

你可以用几种不同的方法处理方法中的Java可选参数。我将引导你（理解）从最简单到更复杂的（情况）。

#### 1.1 @Nullable annotation（@Nullable注解）
Why just don’t pass the null around? It’s a simple solution which doesn’t require any extra work. You don’t have any object which is required as one of method’s parameters? No problem. Just pass the null and the compiler is happy.

为什么不传递null呢？这是一个简单的答案，不需要任何额外的工作。你没有需要作为方法参数之一的对象吗？没有问题。把null传递给编译器就好了。

**The issue here is readability.** How does the programmer who calls a method knows if he can safely pass the null? For which parameters nulls are acceptable and which are mandatory?

**这里的问题是可读性。** 调用方法的程序员如何知道是否可以安全地传递null？哪些参数是可接受null的，哪些是强制的？

To make it clear that the null is a valid input, you can use a @Nullable annotation.

为了说明null是一个有效的输入，你可以使用@Nullable注解。

```
User createUser(String name, @Nullable Email email) {
   // ...
}
```

Don’t you agree this method declaration is self-explanatory?

你不同意这个方法声明是不言自明的吗？

While simple, the issue with null passing approach is that it can easily get out of control. The team may quickly start overusing it and make the code base **hard to maintain with plenty of null check conditions.**

虽然很简单，但是null传递方法的问题是它很容易失控。团队可能很快就会开始 **过度使用它，并在大量的null检查条件下使代码库难以维护。**

There are other options, though.

不过，还有其他选择。

#### 1.2. Optional lists
Instead of null, we can sometime create an empty representation（n. 代表；表现） of a class. Think about Java collections. **If a method accepts a list or a map, you should never use nulls as the input.**

有时我们可以创建一个类的空表示，而不是null。想想Java集合。**如果方法接受List或Map（作为参数），则永远不应该使用null作为（参数）输入。**

An empty collection is always better than null because in majority of cases it doesn’t require any special treatment.

空集合总是比null好，因为在大多数情况下，它不需要任何特殊处理。

You may wonder why you should waste memory to create empty collections. After all, null doesn’t cost you anything.

你可能想知道为什么要浪费内存来创建空集合。毕竟，null不需要任何代价。

Your doubts are justified. Fortunately, there’s a simple solution.

你的怀疑是有道理的。幸运的是，有一个简单的解决方案。

You shouldn’t create a new instance of a collection whenever you need an empty representative. Reuse the same instance across the code base.

在需要空代表时，不应该创建集合的新实例。在代码库中重复使用相同的实例。

And you know what? Java already has empty instances of all collections which you can use. You’ll find them in the Collections utility class.

你知道吗？Java已经有了可以使用的所有集合的空实例。你可以在Collections实用程序类中找到它们。

```
User createUser(String name, List<Rights> rights) {
   // ...
}
```

```
import java.util.Collections;
// ...
create("bob", Collections.emptyList());
```

#### 1.3. Null object pattern（Null对象模式）
The concept of empty collections also applies to other classes. An empty collection is just a regular collection with zero elements. By the same token, you can think about other objects in your applications.

空集合的概念也适用于其他类。空集合只是一个包含零元素的常规集合。同样，你可以考虑应用程序中的其他对象。

**The Null object is a special instance of a class which represents missing value.** If some method expects an object as a parameter, you can always pass the Null object representation without worry it will cause an unexpected exception at the runtime.

**Null对象是表示缺失值的类的特殊实例。** 如果某些方法希望将对象作为参数，那么总是可以传递Null对象表示，而不用担心这会在运行时导致意外异常。

You can implement the Null object pattern in two ways.

你可以通过两种方式实现Null对象模式。

For simple value objects, a default instance with predefined values assigned to properties is enough. Usually, you expose this Null object as a constant so you can reuse it multiple times. For instance:

对于简单的值对象，为属性指定预定义值的默认实例就足够了。通常，你将这个Null对象作为常量公开，以便可以多次重用它。例如：

```
public class User {

   public static final User EMPTY = new User("", Collections.emptyList());

   private final String name;
   private final List<Rights> rights;

   public User(String name, List<Rights> rights) {
       Objects.requireNonNull(name);
       Objects.requireNonNull(rights);
       this.name = name;
       this.rights = rights;
   }
   // ...
}
```

If your Null object also needs to mimic some behavior exposed via methods, a simple instance may not work. In that case, you should extend the class and override such methods.

如果你的Null对象也需要模仿一些通过方法暴露的行为，那么简单的实例就不会起作用了。在这种情况下，你应该扩展类并覆盖这些方法。

Here is an example which extends the previous one:

下面是对上一个例子的扩展：

```
public class AnonymousUser extends User {

   public static final AnonymousUser INSTANCE = new AnonymousUser();

   private AnonymousUser() {
      super("", Collections.emptyList());
   }

   @Override
   public void changeName(String newName) {
       throw new AuthenticationException("Only authenticated user can change the name");
   }
}
```

A dedicated Null object class allows you to put many corner cases in a single place which makes maintenance much more pleasant.

一个专用的Null对象类允许你将许多偏僻个案放在一个地方，这使得维护更加愉快。

#### 1.4. Method overloading（方法重载）
If your design a method with optional parameters, you can expose（vt. 揭露，揭发；使曝光；显示） overloaded versions of that method. **Each method should accept only parameters which are required.**

如果你设计的方法具有可选参数，则可以公开该方法的重载版本。**每个方法只能接受需要的参数。**

With this approach, you don’t have to expect that the caller will provide default values for optional parameters. You pass the defaults on your own inside overloaded method. In other words, you hide the default values for optional parameters from method’s callers.

使用这种方法，你不必期望调用方会为可选参数提供默认值。你可以在重载方法中自行传递默认值。换句话说，对方法调用者隐藏可选参数的默认值。

```
User createUser(String name) {
   this.createUser(name, Email.EMPTY);
}

User createUser(String name, Email email) {
   Objects.requireNonNull(name);
   Objects.requireNonNull(rights);
   // ...
}
```

The overloaded methods can call each other but it’s not mandatory. You can implement each method independently if it’s more convenient. However, usually you validate all parameters and put the logic inside the method with the longest parameter list.

重载的方法可以相互调用，但不是强制的。如果方便的话，可以单独实现每个方法。但是，通常你要验证所有参数，并将逻辑放在具有最长参数列表的方法中。

It’s worth mentioning that method overloading is widely used inside the standard Java library. When you learn how to design APIs, learn from people with greater experience.

值得一提的是，方法重载在标准Java库中被广泛使用。当你学习如何设计API时，请向具有更丰富经验的人学习。

#### 1.5. Parameter Object pattern（参数对象模式）
The majority of developers agree that when the list of method parameters grows too long it become hard to read. Usually, you handle the issue with the Parameter Object pattern. The parameter object is a named container class which groups all method parameters.

大多数开发人员都同意，当方法参数列表增长得太长时，就会变得难以读取。通常，你使用参数对象模式来处理这个问题。参数对象是一个命名容器类，它对所有方法参数进行分组。

Does it solve the problem of optional method parameters?

是否解决了方法参数可选的问题?

No. It doesn’t.

不。它不是。

It just moves the problem to the constructor of the parameter object.

它只是将问题移动到参数对象的构造函数中。

Let’s see how do we solve this more general problem with …

让我们看看如何解决这个更普遍的问题。

### 2. Optional constructor parameters（可选的构造函数参数）
In the perspective（n. 观点） of the problem with optional parameters, simple constructors don’t differ from regular member methods. You can successfully use all the techniques we’ve already discussed also with constructors.

对于可选参数的问题，简单构造函数与常规成员方法没有区别。你可以成功地使用我们已经与构造函数讨论过的所有技术。

However, when the list of constructor parameters is getting longer and many of them are optional parameters, applying constructor overloading may seem cumbersome.

然而，当构造函数参数的列表越来越长，而且其中许多参数是可选参数时，应用构造函数重载可能看起来很麻烦。

If you agree, you should check out the Builder pattern.

如果你同意，你应该检查构建器模式。

#### 2.1. Builder pattern（构建器模式）
Let’s consider a class with multiple optional fields:

让我们考虑一个具有多个可选字段的类：

```
class ProgrammerProfile {

   // required field
   private final String name;

   // optional fields
   private final String blogUrl;
   private final String twitterHandler;
   private final String githubUrl;

   public ProgrammerProfile(String name) {
       Objects.requireNonNull(name);
       this.name = name;
       // other fields assignment...
   }
   // getters
}
```

If you created a constructors to cover all possible combination with optional parameters, you would end up with a quite overwhelming list.

如果你创建了一个构造函数来涵盖所有可能的与可选参数的组合，那么你将得到一个相当庞大的列表。

How to avoid multiple constructors? Use a builder class.

如何避免多个构造函数？使用构建器类。

***译注：关于构建器的内容，可以参考《Effective-Java-3rd-edition》，Item-2: Consider-a-builder-when-faced-with-many-constructor-parameters（[Item-2](https://github.com/clxering/Effective-Java-3rd-edition-Chinese-English-bilingual/blob/master/Chapter-2-Item-2-Consider-a-builder-when-faced-with-many-constructor-parameters.md)）***

You usually implement the builder as an inner class of the class it suppose to build. That way, both classes have access to their private members.

你通常将构建器实现为它要构建的类的内部类。这样，两个类都可以访问它们的私有成员。

Take a look at a builder for the class from the previous example:

看看前面例子中类的构建器：

```
class ProgrammerProfile {

   // fields, getters, ...

   private ProgrammerProfile(Builder builder) {
       Objects.requireNonNull(builder.name);
       name = builder.name;
       blogUrl = builder.blogUrl;
       twitterHandler = builder.twitterHandler;
       githubUrl = builder.githubUrl;
   }

   public static Builder newBuilder() {
       return new Builder();
   }

   static final class Builder {

       private String name;
       private String blogUrl;
       private String twitterHandler;
       private String githubUrl;

       private Builder() {}

       public Builder withName(String val) {
           name = val;
           return this;
       }

       public Builder withBlogUrl(String val) {
           blogUrl = val;
           return this;
       }

       public Builder withTwitterHandler(String val) {
           twitterHandler = val;
           return this;
       }

       public Builder withGithubUrl(String val) {
           githubUrl = val;
           return this;
       }

       public ProgrammerProfile build() {
           return new ProgrammerProfile(this);
       }

   }
}
```

Instead of a public constructor, we only expose one single static factory method for the inner builder class. The private constructor (which the builder calls in the build() method) uses a builder instance to assign all fields and verifies if all required values are present.

我们只为内部构建器类公开一个静态工厂方法，而不是一个公共构造函数。私有构造函数(构建器在build()方法中调用)使用一个构建器实例来分配所有字段，并验证是否存在所有需要的值。

It’s a pretty simple technique once you think about it.

这是一个非常简单的技巧。

The client code of that builder which sets only a selected optional parameter may look as follows:

该生成器的客户端代码只设置一个选定的可选参数，其代码如下:

```
ProgrammerProfile.newBuilder()
       .withName("Daniel")
       .withBlogUrl("www.dolszewski.com/blog/")
       .build();
```

With the builder, you can create all possible combinations with optional parameters of the object.

使用构建器，你可以使用对象的可选参数创建所有可能的组合。

#### 2.2. Compile time safe class builder（编译时安全类构建器）
Unfortunately, just by look at the methods of the builder from the previous paragraph you can’t really tell which parameters are optional and which are required. What is more, without knowing you can omit the required parameters by accident.

不幸的是，仅通过查看前一段中构建器的方法，你无法真正判断哪些参数是可选的，哪些是必需的。更重要的是，如果不知道你可以意外地忽略所需的参数。

Check out the following example of the incorrectly used builder:

查看以下错误使用的构建器示例：

```
ProgrammerProfile.newBuilder()
       .withBlogUrl("www.dolszewski.com/blog/")
       .withTwitterHandler("daolszewski")
       .build();
```

The compiler won’t report any error. You’ll realize the problem with the missing required parameter only at the runtime.

编译器不会报告任何错误。只有在运行时，你才会意识到缺少必需参数的问题。

So how do you tackle the issue?

那么如何解决这个问题呢？

You need to slightly modify the builder factory method so that you can call it only with required parameters and left builder methods only for optional parameters.

你需要稍微修改builder factory方法，以便只能使用必需的参数调用它，而仅对可选参数使用left builder方法。

Here’s all you have to change:

以下是你需要改动的：

```
class ProgrammerProfile {

   // ...

   public static Builder newBuilder(String name) {
      return new Builder(name);
   }

   public static final class Builder {

      private final String name;
      // ...

      private Builder(String name) {
          this.name = name;
      }

      // ...

   }
}
```

#### 2.3. Builder class generation（构建器类生成）
You may think that builders require hell of a lot of code.

你可能认为构建器需要大量的代码。

Don’t worry.

不必担心。

You don’t have to type all that code on your own. All popular Java IDEs have plugins which allow to generate class builders. IntelliJ users can check the InnerBuilder plugin, while Eclipse fans can take a look at Spart Builder Generator. You can also find alternative plugins in the official repositories.

你不需要自己输入所有的代码。所有流行的Java IDE都有允许生成类构建器的插件。IntelliJ用户可以查看InnerBuilder插件，而Eclipse的粉丝可以查看Spart Builder生成器。你还可以在官方存储库中找到其他插件。

If you use the project Lombok, it also simplify working with class builders. You can check this short introduction to Lombok builders if you need a place to start.

如果你使用Project Lombok，它还可以简化与类构建器的工作。如果你需要一个开始的地方，可以查看Lombok构建器的简短介绍。

### 3. Java optional parameters anti-patterns（Java可选参数的反模式）
While browsing the web in search for approaches to work with Java optional parameters, you can find a few additional suggestions than we already covered. Let me explain why you should consider them as wrong approaches.

在浏览web以寻找使用Java可选参数的方法时，你可以找到一些我们已经介绍过的其他建议。让我解释一下为什么你应该认为它们是错误的方法。

#### 3.1. Maps
Technically speaking, method’s input is a set of key-value pairs. In Java, we have a standard built-in data structure which matches this description – the Map.

从技术上讲，方法的输入是一组key-value对。在Java中，我们有一个标准的内置数据结构来匹配这个描述，它就是Map。

The compile won’t prevent you from using HashMap<String, Object> as a container for all optional method parameters but your common sense should.

编译器不会阻止你使用HashMap<String, Object>作为所有可选方法参数的容器，这也是你应该具备的常识。

Although you can put anything in such HashMap, it’s wrong idea. This approach is hard to understand, unreadable and will quickly become your maintenance nightmare.

尽管你可以将任何内容放入这样的HashMap中，但这种想法是错误的。这种方法很难理解、不可读，并且很快就会成为你的维护噩梦。

Still not convinced（v. 使确信）?

还不相信吗？

You should definitely consider switching your career to a JavaScript developer. It’s much easier to cry in the company.

你绝对应该考虑转行到JavaScript开发人员。（和这个相比），在公司里哭要容易得多。

#### 3.2. Java varargs（Java可变参数）
Just to be clear, there’s absolutely nothing wrong in using Java varargs. If you don’t know with how many arguments your method will be called, varargs is a perfect fit.

需要明确的是，使用Java的可变参数绝对没有什么错。如果不知道方法将被调用多少个参数，那么可变参数非常适合。

But using varargs as a container for a single value, which might be present or not, is a misuse. Such declaration allows to call a method with more optional values than expected. We discussed much more descriptive approaches for handling a single optional parameters.

但是使用可变参数作为单个值的容器（可能存在也可能不存在）是一种误用。这样的声明允许调用一个方法比预期更多的可选值。我们讨论了更多的描述方法来处理单一的可选参数。

#### 3.3. Why not Optional as method argument?（为什么不将Optional对象作为方法参数呢?）
Finally, the most controversial approach – Java 8 Optional as a method input. I’ve already written a post about Optional use cases in which I also covered method parameters. Let me extend what you can find there.

最后，最具争议的方法就是将Java 8的Optional对象作为方法输入。我已经写了一篇关于Optional用例的文章，其中也涉及了方法参数。我把你能找到的扩展一下。

##### Memory usage（内存使用情况）
When you create an instance of the Optional class, you have to allocate the memory for it. While the empty optional instance accessed with Optional.empty() is a reusable singleton (just like empty collections we’ve already discussed), non empty instances will occupy the operating memory.

当你创建Optional类的实例时，你必须为它分配内存。虽然使用Optional.empty()访问的空Optional实例是可重用的单例（就像我们已经讨论过的空集合一样），但是非空实例将占用操作内存。

Wrapping objects using Optional factory methods just for the purpose of calling a method which will immediately unwrap them doesn’t make sense if you compare this approach with other possibilities.

如果你将这种方法与其他可能的方法进行比较，那么仅为了调用将立即展开它们的方法而使用可选工厂方法包装对象是没有意义的。

Yet, nowadays Garbage Collectors handle short-lived objects very well. The memory allocation isn’t a big deal. Do we have any other cons?

然而，现在垃圾收集器可以很好地处理短期对象。内存分配不是什么大问题。我们还有其他缺点吗?

##### Coding with reader in mind（为读者编写代码）
What about code readability?

那么代码可读性呢?

```
createProfile("Daniel", Optional.of("www.dolszewski.com/blog/"),
       Optional.of("daolszewski"), Optional.of("https://github.com/danielolszewski"));
```

Maybe it’s just a matter of personal preference but for many developers multiple Optional factory calls are distracting. The noise in the code for the reader. But again, it’s just a matter of taste. Let’s find something more convincing.

也许这只是个人喜好的问题，但是对于许多开发人员来说，多个Optional工厂调用会分散他们的注意力。代码中的噪声是为读者准备的。但这只是品味的问题。让我们找些更有说服力的。

##### Java language architect opinion（Java语言架构师看法）
***译注：原文的language拼写错误***

Brian Goetz, who is Java language architect at Oracle once stated that Optional was added to the standard library with methods’ results in mind, not their inputs.

Oracle的Java语言架构师Brian Goetz曾经说过，将Optional添加到标准库时考虑的是方法的结果，而不是方法的输入。

But software developers are rebels and don’t like listening to authorities. This argument may also seems weak. We have to go deeper.

但软件开发者是叛逆者，不喜欢听当局的。这一论点似乎也站不住脚。我们必须更深入。

##### Does Optional solve optional parameter problem?（可选是否解决可选参数问题？）
If you have a method declaration like this:

如果你有这样的方法声明：

```
doSomethingWith(Optional<Object> parameter);
```

How many possible inputs should you expect?

你应该期望多少可能的输入？

The parameter can be either a wrapped value or the empty optional instance. So the answer is 2, right?

参数可以是包装类的值，也可以是空的Optional实例。所以答案是2，对吧？

Wrong.

错。

The real answer is 3 because you can also pass the null as the argument. You should have a limited trust to inputs when you don’t know who will be the caller of your API.

真正的答案是3，因为你也可以传递null作为参数。当你不知道谁将是API的调用者时，你应该对输入具有有限的信任。

In that case, before processing the parameter you should check if the Optional isn’t equal to the null and then if the value is present. Quite complicated, don’t you agree?

在这种情况下，在处理参数之前，你应该检查Optional是否等于null，然后检查值是否存在。相当复杂，你同意吗?

I pretty sure I have not exhausted the topic yet. As this is one of potential holy wars of Java programming, you should form your own opinion. If you want to add something to the list of arguments against Optional as a method parameter, please share your thoughts in the comments. It’s more than welcome.

我敢肯定我还没有穷尽这个话题。由于这是Java编程的潜在圣战之一，你应该形成自己的观点。如果你想在可选参数的参数列表中添加一些内容，请在评论中分享你的想法。非常欢迎。

### Conclusion（结论）
Let’s recap what we’ve learned. Java doesn’t have a possibility to set a default value to method parameters. The language provides us we many other alternatives for handling optional parameters.

让我们回顾一下我们所学到的。Java不可能为方法参数设置默认值。该语言为我们提供了许多处理可选参数的其他选择。

These alternatives include the @Nullable annotation, null objects, method overloading, and the Parameter Object pattern. We also familiarize with class builders for objects with multiple optional fields. Lastly, we reviewed common bad practices and potential misuses.

这些替代方法包括@Nullable注释、null对象、方法重载和参数对象模式。我们还熟悉具有多个可选字段的对象的类构建器。最后，我们回顾了常见的不良实践和潜在的误用。

If you find the article helpful, I’ll be grateful for sharing it with your followers. I’d also love to know your thoughts, all comments are welcome.

如果你觉得这篇文章对你有帮助，我会很感激与你的追随者分享它。我也想知道你的想法，欢迎所有的意见。
