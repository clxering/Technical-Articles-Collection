# HTML `<kbd>` Tag

Wrap your keyboard command text with `<kbd>`. Great to use especially in documentations! It's more semantically correct & allows you to target it to apply some nice styling. You can also use this tag for other user input such as voice input or text entry device 🙌

#### HTML

```html
<kbd>Ctrl</kbd>
```

![HTML-kbd-Tag-1](/pic/HTML-kbd-Tag-1.png)

### Styling

#### CSS

```css
kbd {
  border: 1px solid lime;
  border-radius: 5px;
  padding: 5px;
}
```

Output

![HTML-kbd-Tag-2](/pic/HTML-kbd-Tag-2.png)

## Default Styling

Here's how it looks like without any styling:

```html
<kbd>Ctrl</kbd>

<p>Ctrl</p>
```

Output

![HTML-kbd-Tag-3](/pic/HTML-kbd-Tag-3.png)

As you can tell, it looks quite plain 😅. It simply just has the **monspace** font. If you open it up in your dev tools, this is what you will get:

```css
/_ Default style _/ kbd {
  font-family: monospace;
}
```

**Auto Styling in GitHub Markdown**

But something interesting I discovered. If you use the standard `<kbd>` in GitHub's markdown. It looks very similar to the style I have in my code tidbit. This is great to add some pizzaz to your README files 💃 Another reason to use the correct HTML tags 😆

## Use Case

This is super useful when writing documentations. I remember I use to always just used the `<code>` tag and then apply some sort of class to target it for styling. But after doing some googling, I found this a more semantic solution, `<kbd>`. That's why Google is a programmer's best friend 😂

❌ Bad

```html
To copy text, you can use the keyboard shortcut:

<code>Ctrl</code> + <code>c</code>
```

✅ Good

```html
To copy text, you can use the keyboard shortcut:

<kbd>Ctrl</kbd> + <kbd>c</kbd>
```

## `code` vs `kbd`

So I mentioned that I use to use the `<code>` tag. Let's look at what that does.

|   `<code>`    |    `<kbd>`    |
| :-----------: | :-----------: |
|    `Ctrl`     |     Ctrl      |
| **monospace** | **monospace** |

From your browser, it might not make any difference with the default styling. They both are using the `monospace` font-family. But semantically they are identified differently.

`<kbd>`: Text that indicates user input from a keyboard, voice input, or any other text entry device.

`<code>`: Text that indicates a short fragment of a computer code.

## Community Input

💬 What other HTML5 tags should I cover next? 😀

- @JelteHomminga: `<details>`
- @JimSaiya: `<samp>`
- @funksweat: `<canvas>`
- @tiagombp: `<meter>`

**fieldset**

@percy_burton: Learnt more about fieldset recently. Thought it was just for radio buttons or checkboxes, but can be to group any related form items, I believe. Great for Accessibility too 😀

@JelteHomminga: And useful to set the whole set of fields to disabled instead of doing that for each input separately

## Resources

- [MDN Web Docs: kbd](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/kbd)
- [MDN Web Docs: code](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/code)
- [w3schools: kbd](https://www.w3schools.com/Tags/tag_kbd.asp)
- [w3schools: code](https://www.w3schools.com/Tags/tag_code.asp)
- [Stack Overflow: What are the differences between kbd, samp and code in HTML](https://stackoverflow.com/questions/32284477/what-are-the-differences-between-kbd-samp-and-code-in-html)
- [HTML Spec: kbd](https://html.spec.whatwg.org/#the-kbd-element)
- [HTML kbd Tag](https://html.com/tags/kbd/)
