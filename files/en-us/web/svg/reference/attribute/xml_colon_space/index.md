---
title: xml:space
slug: Web/SVG/Reference/Attribute/xml:space
page-type: svg-attribute
status:
  - deprecated
browser-compat: svg.global_attributes.xml_space
sidebar: svgref
---

{{Deprecated_Header}}

SVG supports the built-in XML **`xml:space`** attribute to handle whitespace characters inside elements. Child elements inside an element may also have an `xml:space` attribute that overrides the parent's one.

> [!NOTE]
> Instead of using the `xml:space` attribute, use the {{cssxref("white-space")}} CSS property.

This attribute influences how browsers parse text content and therefore changes the way the {{Glossary("DOM")}} is built. Therefore, changing this attribute's value through the DOM API may have no effect.

## Elements

You can use this attribute with any SVG element.

## Usage notes

<table class="properties">
  <tbody>
    <tr>
      <th scope="row">Value</th>
      <td><code>default</code> | <code>preserve</code></td>
    </tr>
    <tr>
      <th scope="row">Default value</th>
      <td><code>default</code></td>
    </tr>
    <tr>
      <th scope="row">Animatable</th>
      <td>No</td>
    </tr>
  </tbody>
</table>

- `default`
  - : With this value set, whitespace characters will be processed in this order:
    1. All newline characters are removed.
    2. All tab characters are converted into space characters.
    3. All leading and trailing space characters are removed.
    4. All contiguous space characters are collapsed into a single space character.

- `preserve`
  - : This value tells the user agent to convert all newline and tab characters into spaces. Then, it draws all space characters (including leading, trailing and multiple consecutive space characters).

    For example, the string "a&nbsp;&nbsp;&nbsp;b" (three spaces between "a" and "b") separates "a" and "b" more than "a b" (one space between "a" and "b").

## Examples

```css hidden
html,
body,
svg {
  height: 100%;
}
```

```html-nolint
<svg viewBox="0 0 160 50" xmlns="http://www.w3.org/2000/svg">
  <text y="20" xml:space="default">    Default    spacing</text>
  <text y="40" xml:space="preserve">    Preserved    spacing</text>
</svg>
```

{{EmbedLiveSample("Examples", "160", "50")}}

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}
