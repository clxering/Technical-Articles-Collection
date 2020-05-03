# Java SimpleDateFormat Example

**Java SimpleDateFormat 使用案例**

> 转译自：https://examples.javacodegeeks.com/java-simpledateformat-example/

In this example we will show how to use `java.text.SimpleDateFormat` class so as to convert a Date into a formatted string or a string to a Date.

在这个案例中，我们将展示如何使用 `java.text.SimpleDateFormat` 类将 Date 类型转化为字符串或字符串转化为 Date 类型。

You can make this conversion using the constructors provided by `java.text.SimpleDateFormat` class and some patterns, such as dd/MM/yyyy, dd-MM-yy and so on, so as to format the Date as you wish. We will show more examples of patterns and format symbols in the following sections.

可以使用 `java.text.SimpleDateFormat` 类提供的构造方法再结合一些模板字符串来进行上述转化，例如：`dd/MM/yyyy`, `dd-MM-yy` 等等，以便按照意愿格式化 Date 类型。下面将展示更多有关模板字符串和格式符的例子。

译注：将 pattern 及其复数形式均译作「模板字符串」

## 1、SimpleDateFormat constructors

**SimpleDateFormat 类的构造方法**

There are four constructors that you can use so as to create a `java.text.SimpleDateFormat`.

创建 `java.text.SimpleDateFormat` 对象可以使用四种构造方法。

## 1.1、SimpleDateFormat()

The simplest constructor which creates a `java.text.SimpleDateFormat` with a default pattern of date and a default locale.

这个默认构造方法生成的 `java.text.SimpleDateFormat` 对象具有默认模板字符串和默认地区设置。

## 1.2、SimpleDateFormat(String pattern)

The constructor which creates a `java.text.SimpleDateFormat` with a given pattern and a default locale.

这个构造方法生成的 `java.text.SimpleDateFormat` 对象具有指定模板字符串和默认地区设置。

## 1.3、SimpleDateFormat(String pattern, DateFormatSymbols formatSymbols)

Constructs a `java.text.SimpleDateFormat` with the given pattern and specific date format symbols. DateFormatSymbols is a class for encapsulating（封装） localizable date-time formatting data, such as the names of the months, the names of the days of the week, and the time zone data.

构造一个具有指定模板字符串和具体的日期格式符的 `java.text.SimpleDateFormat` 对象。DateFormatSymbols 类封装了本地化的 date-time 格式数据，例如月份的名称、星期的名称和时区数据。

## 1.4、SimpleDateFormat(String pattern, Locale locale)

Constructs a `java.text.SimpleDateFormat` with the given pattern and a specific locale.

构造一个具有指定模板字符串和具体地区设置的 `java.text.SimpleDateFormat` 对象。

## 2、Example of SimpleDateFormat

**SimpleDateFormat 类的例子**

Create a java class named SimpleDateFormatExample.java with the following code:

创建一个名为 `SimpleDateFormatExample.java` 的文件，内容如下：

```
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;

public class TestModule {
    @Test
    public void main() {

        Date currentDate = new Date();

        SimpleDateFormat format = new SimpleDateFormat();
        String DateToStr = format.format(currentDate);
        System.out.println("Default pattern: " + DateToStr);

        format = new SimpleDateFormat("yyyy/MM/dd");
        DateToStr = format.format(currentDate);
        System.out.println(DateToStr);

        format = new SimpleDateFormat("dd-M-yyyy hh:mm:ss");
        DateToStr = format.format(currentDate);
        System.out.println(DateToStr);

        format = new SimpleDateFormat("dd MMMM yyyy zzzz", Locale.CHINESE);
        DateToStr = format.format(currentDate);
        System.out.println(DateToStr);

        format = new SimpleDateFormat("MMMM dd HH:mm:ss zzzz yyyy", Locale.CHINA);
        DateToStr = format.format(currentDate);
        System.out.println(DateToStr);

        format = new SimpleDateFormat("E, dd MMM yyyy HH:mm:ss z");
        DateToStr = format.format(currentDate);
        System.out.println(DateToStr);

        try {
            Date strToDate = format.parse(DateToStr);
            System.out.println(strToDate);
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
}

```

Let’s explain the different formats of SimpleDateFormat class in the above code. Firstly, we create a Date object which is initialized with the current date and time. Then, we create different date formatters with different patterns, such as:

让我们来解释上述代码中 SimpleDateFormat 类的不同格式。首先，我们创建了一个初始化为当前日期和时间的 Date 对象。然后，我们用不同的模板字符串创建了不同的日期格式器，例如：

The default pattern, which shows the date in the form of month/day/year and the time using the 12-hour clock.

默认模板字符串展示日期的格式为 `month/day/year`，时间使用 12 小时制表示。

yyyy/MM/dd, which shows the date in the form of year/month/day. As we can observe（观察）, the pattern for the year has 4 letters, which means that the full form of the year will be used (e.g. 2018). Otherwise（否则） a short or abbreviated（缩写） form is used if available.

`yyyy/MM/dd` 将日期展示为 `year/month/day`，正如我们所观察到的，年份在该模板字符串下有 4 个字符，这意味着将使用一年的完整形式（例如 2018 年）。否则，可以使用简写或缩写的格式。

dd-M-yyyy hh:mm:ss, which shows the date in the form of date-month-year (the month will be shown in the abbreviated form, as it has only one letter and not two as in the previous case) and futhermore（此外）, it shows the time (hour, minutes and seconds) while the hour is in am/pm format.

`dd-M-yyyy hh:mm:ss` 将日期展示为 `date-month-year` 的格式（月份将以缩写形式显示，因为它只有一个字符，而不像以前那样只有两个字符）此外，它还显示时间（小时、分钟和秒），而小时的格式是 `am/pm`。

dd MMMM yyyy zzzz, which shows the date and the timezone in full format. We can observe that we also defined the locale of the date/time: Locale.ENGLISH or Locale.ITALIAN below.

`dd MMMM yyyy zzzz` 以完整格式显示日期和时区。我们可以观察到，还定义了 `date/time` 为英国地区，或下文中的意大利地区。

译注：为便于查看效果，已将代码修改为 CHINESE 和 CHINA

E, dd MMM yyyy HH:mm:ss z, which shows the date, the day name of the week and the time (we can see that the hour is in capital, which means that the hour’s values here are between 0 – 23, as we use the 24-hour clock).

`E, dd MMM yyyy HH:mm:ss z` 显示了日期、星期几和时间（我们可以看到小时用大写，这意味着小时的值在 0-23 之间，因为我们使用 24 小时制）。

You may notice that there is a slight but basic difference to the followings:

你可能会注意到下面这些细微但却基本的区别：

mm: representes the minutes.

mm：代表「分钟」

MM: represents the Month.

MM：代表「月份」

dd: represents the day.

dd：代表「日」

DD: represents the day in year (e.g. 189 out of 365).

DD：代表当年中的第几天（例如 365 天中的第 189 天）

hh: represents the hour’s value using the 12-hour clock.

hh：代表使用 12 小时制时的小时数

HH: represents the hour’s value using the 24-hour clock.

HH：代表使用 24 小时制时的小时数

Using all those formatters, we format dates as strings.

使用所有这些格式器，可让我们将 Date 格式化为字符串。

Finally, we show a reverse example, where we parse a string into date, using the parse() method.

最后，我们展示了一个相反的案例，使用 `parse()` 方法将字符串解析为 Date。

For a detailed explanation of the different existing patterns you can visit the java doc SimpleDateFormat.

对于不同现有模板字符串的详细解释，可以访问 SimpleDateFormat 类的文档。

If we run the above code, we will have the following results:

如果我们运行上述代码，我们将得到以下结果:

```
Default pattern: 18-8-4 上午11:17
2018/08/04
04-8-2018 11:17:03
04 八月 2018 China Standard Time
八月 04 11:17:03 中国标准时间 2018
星期六, 04 八月 2018 11:17:03 CST
Sat Aug 04 11:17:03 CST 2018
```
