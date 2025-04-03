# Using `:has()` as a Parent Selector in CSS

There is no parent selector in CSS. However, we can use the recently introduced `:has()` pseudo-class to effectively achieve parent selection.

## Use Case

Consider the following HTML structure:

```html
<div class="list-container">
  <div class="list-item">
    <div class="list-item-label">
      <span class="list-item-label-text" data-name="item-A">Item A</span>
    </div>
    <div class="list-item-action">
      <button>Act on Item A</button>
    </div>
  </div>

  <div class="list-item">
    <div class="list-item-label">
      <span class="list-item-label-text" data-name="item-B">Item B</span>
    </div>
    <div class="list-item-action">
      <button>Act on Item B</button>
    </div>
  </div>

  <div class="list-item">
    <div class="list-item-label">
      <span class="list-item-label-text" data-name="item-C">Item C</span>
    </div>
    <div class="list-item-action">
      <button>Act on Item C</button>
    </div>
  </div>
</div>
```

### The Problem

Suppose we are writing a test that needs to click the button inside the second `.list-item`, the one containing `"Item B"` inside a `<span>`. We can use the `data-name` attribute to select this specific `<span>`. However, in order to click the correct button, we need to select the `<button>` inside the same `.list-item`.

The challenge is that CSS does not provide a way to traverse up from `<span data-name="item-B">` to its parent `.list-item` and then target the corresponding button.

If we could select the parent `<div class="list-item-label">` of the `span` element, we could then use a sibling selector to find the corresponding `.list-item-action` and its button. However, CSS lacks a direct parent selector.

### Hypothetical Solution

If CSS had a parent selector (e.g., `^`), we could write:

```css
.list-container span[data-name="item-B"] ^ div ~ div.list-item-action > button {
  /* If a parent selector (^) existed, we could select the button like this */
}
```

### Solution Using `:has()`

> **Note:** According to MDN, the `:has()` CSS pseudo-class represents an element if any of the relative selectors passed as an argument match at least one element when anchored against this element.

Instead of selecting the `span` with `[data-name="item-B"]`, we can select the `.list-item-label` that **has** a `span` with `[data-name="item-B"]`. Then, we can use sibling selector to target div containing the button:

```css
.list-container .list-item-label:has(span[data-name="item-B"]) ~ .list-item-action > button {
  /* Correctly selects the button for "Item B" */
}
```
