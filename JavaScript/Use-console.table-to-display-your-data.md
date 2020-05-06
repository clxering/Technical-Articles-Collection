# Use `console.table` to display your data

**使用 `console.table` 展示数据**

> 转译自：https://www.samanthaming.com/tidbits/2-console-table

This is a cool way to display your data in your browser dev tools. Works great with Array and Objects. Instead of console.log, try `console.table` next time ⭐️

这是在浏览器开发工具中显示数据的一种很酷的方式。对数组和对象非常有效。下次尝试 `console.table`，而不是 console.log

```js
const amazingAthletes = [
  {
    firstName: "Ronda",
    lastName: "Rousey",
    sport: "🥊"
  },
  {
    firstName: "Chloe",
    lastName: "Kim",
    sport: "🏂"
  },
  {
    firstName: "Tessa",
    lastName: "Virtue",
    sport: "⛸"
  },
  {
    firstName: "Hayley",
    lastName: "Wickenheiser",
    sport: "🏒"
  }
]

console.table(amazingAthletes);
```