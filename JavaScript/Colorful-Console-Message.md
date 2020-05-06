# Colorful Console Message 🌈

**丰富多彩的控制台消息**

> 转译自：https://www.samanthaming.com/tidbits/40-colorful-console-message

Add some attitude to your console statement with the `%c` specifier 👩‍🎨 This is super handy to help you easily identify debug information from the console 👾

使用 `%c` 说明符向控制台语句添加一些 attitude，这非常方便，可以帮助你轻松地从控制台识别调试信息。

Especially if you have a huge application where there are tons of logs being printed out in the browser console. Styling your log message will make sure those important messages don't get buried 👍

特别是如果你有一个大型的应用程序，其中有大量的日志打印在浏览器控制台中。设计日志消息的样式将确保那些重要的消息不会被埋没。

Or use it like Facebook to tell people to stay away. Next time you visit their site, open up the Developer Tools, you will see a large "Stop!" message in the console. Well, now you know how that's being created 💥

或者像 Facebook 想告诉人们远离它。下次你访问他们的站点时，打开开发人员工具，将在控制台中看到一个很大的「Stop!」消息。现在你知道它是怎么产生的了。

```js
// Put this in your browser console
console.log("%cHello", "color: green; background: yellow: font-size: 30px");
```

## What is %c

**%c**: Applies CSS style rules to the output string as specified by the second parameter

将 CSS 样式规则应用于第二个参数指定的输出字符串

## Multiple Console Message Styles

**多种控制台消息样式**

To add multiple style, you just pre-pend the message with `%c`. The text before the directive will not be affected. Only the text after the directive will be styled using the CSS declarations in the parameter.

要添加多个样式，只需在消息前面加上 `%c` 即可。指令前的文本不受影响。只有指令之后的文本将使用参数中的 CSS 声明进行样式化。

```js
console.log(
  "Nothing here %cHi Cat %cHey Bear", // Console Message
  "color: blue",
  "color: red" // CSS Style
);
```

## Applying style to other `console` messages

**将样式应用于其他 `console` 消息**

There are 5 console types of console messages:

控制台消息有五种形式：

- `console.log`
- `console.info`
- `console.debug`
- `console.warn`
- `console.error`

Any yup, you can style the rest of them as well!

当然，你也可以设计其他的形式！

```js
console.log("%cconsole.log", "color: green;");
console.info("%cconsole.info", "color: green;");
console.debug("%cconsole.debug", "color: green;");
console.warn("%cconsole.warn", "color: green;");
console.error("%cconsole.error", "color: green;");
```

## Passing the console CSS style as an Array

**将控制台 CSS 样式作为数组传递**

As you get more styles, the string can be quite long. Here's a nifty trick you can do to clean things up. You can pass the styles as an array. And then you can use the `join()` method to turn the array style elements into a string.

当你想获得更多的样式时，字符串可能会很长。这里有一个你可以用来清理东西的小技巧。可以将样式作为数组传递。然后可以使用 `join()` 方法将数组样式元素转换为字符串。

```js
// 1. Pass the css styles in an array
const styles = [
  "color: green",
  "background: yellow",
  "font-size: 30px",
  "border: 1px solid red",
  "text-shadow: 2px 2px black",
  "padding: 10px"
].join(";"); // 2. Concatenate the individual array item and concatenate them into a string separated by a semi-colon (;)

// 3. Pass the styles variable
console.log("%cHello There", styles);
```

To learn more about `join()`, you can read my [Web Basics series](https://www.samanthaming.com/web-basics/how-to-reverse-a-string-in-js) or check out the official [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join).

要了解更多关于 `join()` 的内容，可以阅读 [Web Basics series](https://www.samanthaming.com/web-basics/how-to-reverse-a-string-in-js) 或者查看官方文档 [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join).

## Refactoring console message with `%s`

**用 `%s` 重构控制台消息**

Beside cleaning up the console message by passing the styles as an array. We can also clean up the message using the `%s` specifier.

此外，除了将样式作为数组传递来清理控制台消息。我们还可以使用 `%s` 符号清理消息。

```js
const styles = ["color: green", "background: yellow"].join(";");

const message = "Some Important Message Here";

// 3. Using the styles and message variable
console.log("%c%s", styles, message);
```

## Community Suggestions

**社区建议**

## Console Font Color in `node.js`

If you're running node.js application, you can use the color reference of text to style your messages.

如果你正在运行 node.js 应用程序，可以使用文本的颜色引用来设置消息的样式。

[Stack Overflow: Color Reference](https://stackoverflow.com/questions/9781218/how-to-change-node-jss-console-font-color)

```js
console.log("\x1b[36m%s\x1b[0m", "I am cyan"); // Cyan
console.log("\x1b[33m%s\x1b[0m", "yellow"); // Yellow
```

Thanks @danieldeepak7

## Community Feedback

- @thecodegoddess: I love this especially for projects that have a ton of logging from third party apps. My logs will get buried. I even added a snippet to my IDE so I can easily stick this in. My go to CSS debugging color? Deeppink.

## Resources

- [MDN Web Docs - Console Web APIs](https://developer.mozilla.org/en-US/docs/Web/API/console)
- [Google Developers - Console API Reference](https://developers.google.com/web/tools/chrome-devtools/console/console-reference)
- [Hackernoon - Styling logs in browser console](https://hackernoon.com/styling-logs-in-browser-console-2ec0807dc91a)
- [Make Console.log() output colorful and stylish in browser & node](http://voidcanvas.com/make-console-log-output-colorful-and-stylish-in-browser-node/)
- [Colorful console.log](https://coderwall.com/p/fskzdw/colorful-console-log)
- [Add Styles to Console Statements](https://davidwalsh.name/add-styles-console)
- [Google Developers - Styling console output with CSS](https://developers.google.com/web/tools/chrome-devtools/console/console-write#styling_console_output_with_css)
- [Enhance your JS console logging messages](https://coderwall.com/p/m2trga/enhance-your-js-console-logging-messages)
