# Use `console.table` to display your data

This is a cool way to display your data in your browser dev tools. Works great with Array and Objects. Instead of console.log, try console.table next time ⭐️

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