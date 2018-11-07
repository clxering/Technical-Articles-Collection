## Difference between JavaScript function in href and onclick

> 转译自：https://www.codespeedy.com/difference-between-javascript-function-in-href-and-onclick/

> In this JavaScript post, I am going to clear the difference between JavaScript function in href and onclick. You will also learn where to use JavaScript function in href and onclick. In both the cases, you will get the same result but there are some behavioural differences. I am here to clear all the confusions arising in your mind regarding this.

> 在这篇JavaScript文章中，我将澄清href和onclick中的JavaScript函数之间的区别。您还将了解如何在href和onclick中使用JavaScript函数。在这两种情况下，你会得到相同的结果，但也存在一些行为差异。我来这里是为了澄清你对这件事的所有困惑。

Here I am going to show you example of JavaScript function implementation using href and onclick both

在这里，我将展示使用href和onclick两种方法实现JavaScript函数的示例

```
<a href="javascript:inHrefFunction()">Click Me!</a>
```

This one is the implementation of JavaScript function in href.

这是在href中实现的JavaScript函数。

```
<a href="#" onclick="onclickFunction()">Click Me!</a>
```

This one is the implementation of JavaScript function in onclick.Basically, there is no difference in the performance.

这是onclick中JavaScript函数的实现。在性能上基本没有区别。

[How to write inline JavaScript code in HTML file](https://www.codespeedy.com/how-to-write-inline-javascript-code-in-html/)

But the first one will fail for sure for the browsers where JavaScript is not enabled.And the second one will also fail but it will be better if the href pointed to a URL for users without JS enabled.

但是第一个（例子）在没有启用JavaScript的浏览器中肯定会失败。第二种方法也会失败，但如果href为不启用JS的用户指向URL，那就更好了。

In onclick, you can pass this as an argument but you can not do it for href.Let’s see the below example:

在onclick中，您可以将其作为参数传递，但对于href则不能这样做。让我们看看下面的例子：

```
<a href="#" onclick="document.write(this.innerHTML)">I am onclick</a>
```

Here you will get output:

将得到输出：

```
I am onclick
```

But if you call JavaScript from the href:

但如果你从href中调用函数：

```
<a href="javascript:document.write(this.innerHTML)">I am href</a>
```

Here the output will be like this:

你的输出如下所示：

```
undefined
```

If you add JavaScript in href and your browser is not JS enabled then it is a great problem.But if you use JavaScript in onclick and add return false; or return doAnythingYouWant(); it will be better for you. As in this case if your browser is not JS enabled then it will navigate to the page you wanted. And if Js is enabled then you can display something with JS.

如果你在href中添加JavaScript，而你的浏览器没有启用JS，那么这是一个大问题。但如果在onclick中使用JavaScript并添加return false;或返回doAnythingYouWant();这对你有好处。在这种情况下，如果您的浏览器没有启用JS，那么它将导航到您想要的页面。如果启用了Js，你就可以用Js来显示一些东西。

You can Also read,

你也可以阅读（下列相关文章）

[Guess The Number Game Using JavaScript（用JavaScript实现猜数字游戏）](https://www.codespeedy.com/guess-the-number-game-using-javascript/)

[How to run multiple JavaScript functions onclick（onclick如何运行多个JavaScript函数）](https://www.codespeedy.com/how-to-run-multiple-javascript-functions-onclick/)
