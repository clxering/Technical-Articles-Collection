# Java Custom Exception Example

**Java 自定义异常案例**

> 转译自：https://examples.javacodegeeks.com

In this example we will look briefly（短暂的） at the basics of `Exception`, in Java Programming Language. We will also see, how to create a custom `Exception` Class.

在这个案例中，我们将使用 Java 语言来简要介绍“异常”的基础知识。我们同样也能够了解如何创建一个自定义的 Exception 类。

## 1、Basics of Exception

**异常的基础知识**
As per（按照） oracle docs, An exception is an event, which occurs during the execution of a program, that disrupts（扰乱） the normal flow of the program’s instructions（指令）.

按照 oracle 的文档介绍，异常是一个在程序运行期间发生的事件，会扰乱正常程序流程的指令。

In laymen（非专业人员） terms, when a condition occurs, in which the routine（程序） is not sure how to proceed in an ordinary way it creates an object of exception and hands it over to the runtime system to find an appropriate handler for the exception object. In case, the runtime system does not find an appropriate handler in the call hierarchy the runtime system terminates.

用非专业的话来说，当满足异常发生条件时，程序不确定能以普通的方式进行时，它创建一个异常对象并递交给运行时系统，以查找处理异常对象的合适的处理程序。在这种情况下，运行时系统在运行时系统终止的调用层次结构中没有找到合适的处理程序。

The exceptions have `java.lang.Throwable` as their superclass.The three main categories of Exceptional conditions are :

异常以 java.lang.Throwable 类作为父类。异常主要有三大类：

（1）Error(represented by `java.lang.Error` and its sub-classes)

错误（以 java.lang.Error 类及其子类表示）

（2）Checked Exception(represented by direct subclasses of `java.lang.Exception` except `java.lang.RuntimeException`)

Checked 异常（由 java.lang.Exception 除了 java.lang.RuntimeException 类的子类表示）

（3）Unchecked or Runtime Exceptions (represented by `java.lang.RuntimeException` and its sub-classes)

Unchecked 或运行时异常（由 java.lang.RuntimeException 类及其子类表示）

**Errors :** Errors denote serious abnormal（不正常的） condition with the application like `OutOfMemoryError` or `VirtualMachineError`. Any reasonable application should not try to recover from such a condition.

错误表示严重的异常情况，如 OutOfMemoryError（内存溢出）或 VirtualMachineError（虚拟机错误）。任何理由都不能使应用程序从这种情况中恢复过来。

**Runtime Exception/Unchecked Exception :** These types of exceptions usually indicate programming errors like `NullPointerException` or `IllegalArgumentException`. The application may or may not choose to recover from the condition.

这些类型的异常通常表示程序错误，如 NullPointerException（空指针异常）或 IllegalArgumentException（参数异常）。应用程序可以选择从该类异常中恢复，也可以选择不恢复。

**Checked Exception :** An application is supposed to catch these types of exceptions and recover reasonably from them. Examples include `FileNotFoundException` and `ParseException`.

应用程序应该捕获这类异常并合理恢复。例如 FileNotFoundException（文件未找到异常）和 ParseException（解析异常）。

## 2、Creating custom Exception

**创建自定义异常**
The first thing before creating a custom exception, the developer should be able to justify（证明） the creation（创建，n.）. As per（按照） Java Docs, You should write your own exception classes if you answer yes to any of the following questions; otherwise, you can probably use someone else’s.

创建自定义异常前，开发者应为此（行为）找到依据。按照 Java 文档，如果你对以下任何一个问题回答 yes，你应该编写自己的异常类；否则，你（最好）用别人的。

（1）Do you need an exception type that isn’t represented by those in the Java platform?

你需要一个 Java 平台中没有（定义）的异常类型吗？

（2）Would it help users if they could differentiate（区分） your exceptions from those thrown by classes written by other vendors（供应商）?

如果用户可以将你的异常与其他供应商编写的类抛出的异常区分开来，那么这会对用户有帮助吗?

（3）Does your code throw more than one related exception?

您的代码是否会抛出多个相关异常？

（4）If you use someone else’s exceptions, will users have access to those exceptions? A similar question is, should your package be independent and self-contained?

如果你使用其他人的异常，用户是否可以访问这些异常？类似的问题是，你的包应该是独立的、自包含的吗？

Now, that you are sure that you really do want a create a custom Exception class we will begin writing the actual program.To create a custom checked exception, we have to sub-class from the `java.lang.Exception` class. And that’s it! Yes, creating a custom exception in java is simple as that!

现在，您确信您确实想要创建一个自定义的异常类，我们将开始编写实际的程序。要创建一个自定义异常，我们必须从 java.lang.Exception 派生子类。是的，在 java 中创建一个自定义异常很简单！

```
public class CustomException extends Exception{}
```

Sometimes though, the `CustomException` object will have to be constructed from another exception. So its important that we create a constructor for such a scenario.

但有时，CustomException 对象需要通过另一个异常来构造。因此，为这样的场景创建构造函数是很重要的。

So a more complete class would be as below :

因此，一个更完整的类应如下所示：

**CustomException.java:**

```
package com.javacodegeeks.examples.exception;
public class CustomException extends Exception
{
    private static final long serialVersionUID = 1997753363232807009L;
		public CustomException()
		{}
		public CustomException(String message)
		{
			super(message);
		}
		public CustomException(Throwable cause)
		{
			super(cause);
		}
		public CustomException(String message, Throwable cause)
		{
			super(message, cause);
		}
		public CustomException(String message, Throwable cause,
                         boolean enableSuppression, boolean writableStackTrace)
		{
			super(message, cause, enableSuppression, writableStackTrace);
		}
}
```

**2.1、Testing the custom Exception（测试自定义异常） :**
**CustomExceptionTest.java:**

```
package com.javacodegeeks.examples.exception;
public class CustomExceptionTest
{
    public static void main(String[] args)
    {
	try
        {
	      testException(null);
        }
        catch (CustomException e)
        {
	      e.printStackTrace();
        }
    }
    public static void testException(String string) throws CustomException
    {
	      if(string == null)
		    throw new CustomException("The String value is null");
    }
}
```

Similarly, an unchecked exception can be created by sub-classing from the `java.lang.RuntimeException` Class.

类似地，unchecked 异常可以通过 java.lang.RuntimeException 的子类来创建。

```
public class CustomException extends RuntimeException{...}
```

## 3、Points to note

**注意事项**
（1）An unchecked exception should be preferred（首选） to a checked exception. This will help programmes to be free of unnecessary `try..catch blocks`.

相比一个 unchecked 异常，应首选 checked 异常。这有助于程序摆脱不必要的 try..catch 块。

（2）Exceptions are abnormal conditions, and as such should not be used for controlling the flow of execution of the programmes(as demonstrated by example below).

异常是异常情况，不应该用于程序的流程控制（如下面示例所示）。

```
try
{
	for (int i = 0;; i++)
	{
		System.out.println(args[i]);
	}
}
catch (ArrayIndexOutOfBoundsException e)
{
		// do nothing
}
```

（3）A `catch block` should not be empty as shown in the example above. Such exceptions are very difficult to track（跟踪）, since there is no logging.

catch 块不应该是空的，如上面的示例所示。由于没有日志记录，因此很难跟踪这些异常。

（4）The name of a custom exception class should end with Exception. It improves the readability of the code. Instead of naming the class InvalidSerial, the class should be named InvalidSerialException.

自定义异常类的名称应该以 Exception 结尾。它提高了代码的可读性。类不应该命名为 InvalidSerial，而应该命名为 InvalidSerialException。

（5）When using ARM/try-with-resources blocks, if exception gets suppressed by the try-with-resources statement, we can use `Throwable#getSuppressed()` method.

当使用 ARM/try-with-resources 块时，如果异常被 try-with-resources 语句抑制，我们可以使用 Throwable# getrepression()方法。

## 4、Closing Words

**结束语**

Here,we tried to understand the basics of exception and how to create a custom exception of our own. We learned the best practices while creating a custom exception class and also how to handle（处理） an exception in a reasonable manner.

在这里，我们尝试理解异常的基本原理，以及如何创建我们自己的自定义异常。我们在创建自定义异常类时学习了最佳实践，以及如何以合理的方式处理异常。

## 附录，相关资料 1

**可与上述转译内容互参**
Throwable 是所有 Java 中所有异常类的父类，有两种子类：**Error**和**Exception**

**1、Error：** 表示由 JVM 所侦测到的无法预期的错误，由于这是属于 JVM 层次的严重错误，导致 JVM 无法继续执行，这是不可捕捉到的，无法采取任何恢复的操作，只能显示错误信息。Error 类体系描述了 Java 运行系统中的内部错误以及资源耗尽的情形，应用程序不应该抛出这种类型的对象（一般是由虚拟机抛出）。假如出现这种错误，除了尽力使程序安全退出外，在其他方面是无能为力的。

**2、Exception：** 表示可恢复的异常，这是可捕捉到的。Java 提供了两类主要的异常：Runtime Exception 和 checked exception。

（2-1）checked 异常是经常遇到的，如：IO 异常、SQL 异常。对于这种异常，JAVA 编译器强制要求我们对出现的这些异常进行 catch。所以，面对这种异常只能编写 catch 块去处理。这类异常一般是外部错误，例如试图从文件尾后读取数据等，这并不是程序本身的错误，而是在应用环境中出现的外部错误。

（2-2）Runtime Exception 也称运行时异常，我们可以不处理。当出现这样的异常时，总是由虚拟机接管。比如：我们从来没有人去处理过 NullPointerException 异常，它就是运行时异常，并且 NullPointerException 还是最常见的异常之一。Runtime Exception 体系包括错误的类型转换、数组越界访问和试图访问空指针等等。如果出现 Runtime Exception，那么一定是程序员的错误。例如：可以通过检查数组下标和数组边界来避免数组越界访问异常。

出现运行时异常后，系统会把异常一直往上层抛，一直遇到处理代码。如果到最上层还没有处理块，多线程程序就由 Thread.run()抛出，这个线程就退出；如果是单线程就被 main()抛出，整个程序也就退出了。运行时异常是 Exception 的子类，也有一般异常的特点，是可以被 catch 块处理的。只不过往往我们不对他处理罢了。如果不对运行时异常进行处理，那么出现运行时异常之后，要么是线程中止，要么是主程序终止。

如果不想终止线程或程序，则必须捕捉所有的运行时异常，决不让这个处理线程退出。队列里面出现异常数据了，正常的处理应该是把异常数据舍弃，然后记录日志。不应该由于异常数据而影响下面对正常数据的处理。但并不代表在所有的场景你都应该如此，如果在其它场景，遇到了一些错误，如果退出程序比较好，这时你就可以不太理会运行时异常，或者是通过对异常的处理显式的控制程序退出。异常处理的目标之一就是为了把程序从异常中恢复出来。
