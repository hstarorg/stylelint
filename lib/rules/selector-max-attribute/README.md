# selector-max-attribute

Limit the number of attribute selectors in a selector.

```css
    [rel="external"] {}
/** ↑
 * This type of selector */
```

This rule resolves nested selectors before counting the number of attribute selectors. Each selector in a [selector list](https://www.w3.org/TR/selectors4/#selector-list) is evaluated separately.

The `:not()` pseudo-class is also evaluated separately. The rule processes the argument as if it were an independent selector, and the result does not count toward the total for the entire selector.

## 选项

`int`: Maximum attribute selectors allowed.

例如，使用 `2`：

以下模式被视为违规：

```css
[type="number"][name="quality"][data-attribute="value"] {}
```

```css
[type="number"][name="quality"][disabled] {}
```

```css
[type="number"][name="quality"] {
  & [data-attribute="value"] {}
}
```

```css
[type="number"][name="quality"] {
  & [disabled] {}
}
```

```css
[type="number"][name="quality"] {
  & > [data-attribute="value"] {}
}
```

```css
/* `[type="text"][data-attribute="value"][disabled]` is inside `:not()`, so it is evaluated separately */
input:not([type="text"][data-attribute="value"][disabled]) {}
```

以下模式*不*被视为违规：

```css
[type="text"] {}
```

```css
[type="text"][name="message"] {}
```

```css
[type="text"][disabled]
```

```css
/* each selector in a selector list is evaluated separately */
[type="text"][name="message"],
[type="number"][name="quality"] {}
```

```css
/* `[disabled]` is inside `:not()`, so it is evaluated separately */
[type="text"][name="message"]:not([disabled]) {}
```

## 可选的辅助选项

### `ignoreAttributes: ["/regex/", /regex/, "string"]`

给定：

```js
["/^my-/", "dir"]
```

例如，使用 `0`。

以下模式*不*被视为违规：

```css
[dir] [my-attr] {}
```

```css
[dir] [my-other-attr] {}
```
