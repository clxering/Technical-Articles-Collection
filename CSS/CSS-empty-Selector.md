# CSS :empty Selector

Often, we want to style elements that contain content. How about when an element has no children or text at all? Easy, you can use the `:empty` selector 🤩

```html
<p></p>
<!-- NOT empty: note the blank space -->
<p></p>
<!-- YES empty: nothing inbetween -->
```

```css
p::before {
  font-family: "FontAwesome";
  content: "\f240";
}

p:empty::before {
  content: "\f244";
}

p {
  color: silver;
}

p:empty {
  color: red;
}
```

## What's considered empty?

When I first encounter this, there was a few confusion about what this property considers as **empty**. Let's stick with `MDN's` definition here:

> The :empty CSS pseudo-class represents any element that has no children. Children can be either element nodes or text (including whitespace). Comments, processing instructions, and CSS content do not affect whether an element is considered empty.

### Is empty

As long as there is no whitespace, it's an empty element.

```html
<p></p>
```

Comment in between is also considered an empty element. As long as there is no whitespace.

```html
<p><!-- comment --></p>
```

### Not empty

Whitespace is considered not empty. Even in a new line, there is whitespace, so not empty! Emphasizing on this, cause I made the same mistake 😅

```html
<p></p>

<p>
  <!-- comment -->
</p>
```

Having children element also counts as not empty

```html
<p><span></span></p>
```

### Whitespace in Future Spec

The good news is -- in [Selectors Level 4](https://drafts.csswg.org/selectors-4/#the-empty-pseudo), whitespace would be considered empty! This will make it similar to act like `:-moz-only-whitespace`. In other words, this would considered empty:

```html
<!-- Considered Empty in CSS Selectors Level 4 -->
<p></p>
```

⚠️ BUT, don't do this yet. Currently no browser supports this.

## Examples using `:empty`

Okay, let's take a look at some real-life examples of using `:empty`.

### Using `:empty` in Form Error Message

This is the example that made me first discovered `:empty`. So I wanted to prepend a ❌ icon on my error message. But the problem is the icon appeared even when I had no error message. But then no problem, I can just use the `:empty` to only append the icon when there IS an error message 👍

### CSS

```css
.error:before {
  color: red;
  content: "\0274c "; /* ❌ icon */
}
```

### HTML

```html
<!-- No error message -->
<div class="error"></div>

<!-- Yes error message -->
<div class="error">Missing Email</div>
```

### Output

Without Empty

<blockquote style="height: 88px">
<div class="error" style="transform: translateY(100%);">❌</div><div class="error" style="transform: translateY(100%);">❌ Missing Email</div>
</blockquote>

With `:empty`

<blockquote style="height: 64px;">
<div class="error" style=""></div><div class="error" style="transform: translateY(100%);">❌ Missing Email</div>
</blockquote>

### Using `:empty` in Alerts

Here's another example using `:empty` to hide empty state.

```css
.alert {
  background: pink;
  padding: 10px;
}
.alert:empty {
  display: none;
}
```

### TML

```html
<div class="alert"></div>
<div class="alert">Alert Message</div>
```

### Output

Without empty

<blockquote style="height: 124px">
<div class="alert" style="background: pink; padding: 10px; margin-bottom:20px;transform: translateY(100%);"></div><div class="alert" style="background: pink; padding: 10px;transform: translateY(50%);">Alert Message</div>
</blockquote>

With :empty

<blockquote style="height: 84px">
<div class="alert"></div><div class="alert" style="background: pink; padding: 10px;transform: translateY(50%);">Alert Message</div>
</blockquote>

## Browser Support

Support on this is actually really good. It supports all the way to Internet Explorer 9 🙌

- [Browser Support: empty](https://developer.mozilla.org/en-US/docs/Web/CSS/:empty#Browser_compatibility)

## Community Examples

I discovered `:empty` when I was trying to style empty error messages in a form. `<div class="error"></div>`. No JS required anymore, I can use pure CSS 👍

**💬 What other use cases do you know?**

- @delusioninabox: A fix to remove janky spacing of empty paragraph tags, generally from user-created content. 😅

- @hkfoster: I’ve used it to squash any number of randomly generated selectors that I can’t weed out on the templating side. 👍

- @sumurai8: Removing padding from empty paragraphs

- @\_ottenga: Nice for notification dot (if empty is not visible, if has some number - for example - is red with the number inside)

- @stephenjbell: `li:empty { visibility:hidden; }` Let an empty list item act as kind of a paragraph break (no bullet) in the middle of a list.

- @jlabs: I've used this recently within ul that show's a message when the ul has no children - the list was populated via JS.

## Community Input

- @bourhaouta: One more thing about :empty it doesn't select elements that contain whitespace, in the other side we have :blank but it's not well supported yet 😔

- @link2twenty: I love seeing little known features like this getting some spotlight! It's probably worth noting when the level 4 selectors roll out white space will be included as empty 🙂

- @okumurakengo: also :not and :empty can be used to hide empty state 😀

```css
<style>
.alert:not(:empty) {
  background: pink;
  padding: 10px;
}
</style>
<div class="alert"></div>
<div class="alert">Alert Message</div>
```

## Resources

- [MDN Web Docs:](https://developer.mozilla.org/en-US/docs/Web/CSS/:empty)
- [w3schools:](https://www.w3schools.com/cssref/sel_empty.asp)
- [CSS Tricks: empty](https://css-tricks.com/almanac/selectors/e/empty/)
- [5 Lesser Used CSS Selectors](https://bitsofco.de/5-lesser-used-css-selectors/)
- [empty and blank](https://zellwk.com/blog/empty-and-blank/)
- [freeCodeCamp: When to use the :empty and :blank CSS pseudo selectors](https://www.freecodecamp.org/news/empty-and-blank-53b9e96151cd/)
- [Why empty states deserve more design time](https://www.invisionapp.com/inside-design/why-empty-states-deserve-more-design-time/)
- [codrops: empty](https://tympanus.net/codrops/css_reference/empty/)
