---
title: container-type
slug: Web/CSS/container-type
page-type: css-property
browser-compat: css.properties.container-type
---

{{CSSRef}}

An element can be established as a query container using the **`container-type`** [CSS](/en-US/docs/Web/CSS) property. `container-type` is used to define the type of container context used in a container query. The available container contexts are:

- Size: Container size queries allow you to selectively apply CSS rules to a container's children based on a general size or inline size condition such as a maximum or minimum dimension, aspect ratio, or orientation.
- Scroll-state: Container scroll-state queries allow you to selectively apply CSS rules to a container's children based on a scroll-state condition such as whether it is partially scrolled or whether it is snapped to a scroll snap container. Scroll-state queries either apply directly to a scroll container or to an element that is affected by the scroll position of an ancestor scroll container.

> [!NOTE]
> When using the `container-type` and {{cssxref("container-name")}} properties, the `style` and `layout` values of the {{cssxref("contain")}} property are automatically applied.

## Syntax

```css
/* Keyword values */
container-type: normal;
container-type: size;
container-type: inline-size;

/* Global Values */
container-type: inherit;
container-type: initial;
container-type: revert;
container-type: revert-layer;
container-type: unset;
```

### Values

- `inline-size`

  - : Establishes a query container for dimensional queries on the [inline axis](/en-US/docs/Web/CSS/CSS_logical_properties_and_values/Basic_concepts_of_logical_properties_and_values#block_and_inline_dimensions) of the container.
    Applies layout, style, and inline-size containment to the element.

    Inline size containment is applied to the element. The inline size of the element can be computed in isolation, ignoring the child elements (see [Using CSS containment](/en-US/docs/Web/CSS/CSS_containment/Using_CSS_containment)).

- `normal`

  - : Default value. The element is not a query container for any container size queries, but remains a query container for [container style queries](/en-US/docs/Web/CSS/@container#container_style_queries).

- `scroll-state`

  - : Establishes a query container for scroll-state queries on the container.

- `size`

  - : Establishes a query container for container size queries in both the [inline and block](/en-US/docs/Web/CSS/CSS_logical_properties_and_values/Basic_concepts_of_logical_properties_and_values#block_and_inline_dimensions) dimensions.
    Applies layout containment, style containment, and size containment to the container.

    Size containment is applied to the element in both the inline and block directions. The size of the element can be computed in isolation, ignoring the child elements.

## Formal definition

{{CSSInfo}}

## Formal syntax

{{CSSSyntax}}

## Description

If you want to selectively apply styles inside a container based on conditional tests performed on the container, you can do so with container queries. The `container-type` property allows you to specify the type of container context to apply to the container — that is, the type of conditional tests you want to perform on it.

A {{cssxref("@container")}} at-rule is then used to specify the test that will be performed on the container, and the rules that will apply if the query returns `true`.

### Container size queries

[Container size queries](/en-US/docs/Web/CSS/CSS_containment/Container_size_and_style_queries#container_size_queries) allow you to selectively apply CSS rules to a container's children based on a size condition such as a maximum or minimum dimension, aspect ratio, or orientation.

Size containment turns off the ability of an element to get size information from its contents, which is important for container queries to avoid infinite loops. If this were not the case, a CSS rule inside a container query could change the content size, which in turn could make the query evaluate to false and change the parent element's size, which in turn could change the content size and flip the query back to true, and so on. This sequence would then repeat itself in an endless loop.

The container size has to be explicitly defined or set by context, such as block-level elements that stretch to the full width of their parent. If an explicit or contextual size is not available, elements with size containment will collapse.

### Container scroll-state queries

[Container scroll-state queries](/en-US/docs/Web/CSS/CSS_conditional_rules/Container_scroll-state_queries) allow you to selectively apply CSS rules to a container's children based on a scroll-state condition such as whether it is partially scrolled, whether it is snapped to a scroll snap container, or whether it is positioned via [`position: sticky`](/en-US/docs/Web/CSS/display) and stuck to a boundary of a {{glossary("scroll container", "scolling container")}}.

## Examples

### Establishing inline size containment

Given the following HTML example which is a card component with an image, a title, and some text:

```html
<div class="container">
  <div class="card">
    <h3>Normal card</h3>
    <div class="content">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua.
    </div>
  </div>
</div>

<div class="container wide">
  <div class="card">
    <h3>Wider card</h3>
    <div class="content">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua.
    </div>
  </div>
</div>
```

To create a container context, add the `container-type` property to an element.
The following uses the `inline-size` value to create a containment context for the [inline axis](/en-US/docs/Web/CSS/CSS_logical_properties_and_values/Basic_concepts_of_logical_properties_and_values#block_and_inline_dimensions) of the container:

```css
.container {
  container-type: inline-size;
  width: 300px;
  height: 120px;
}

.wide {
  width: 500px;
}
```

```css hidden
h3 {
  height: 2rem;
  margin: 0.5rem;
}

.card {
  height: 100%;
}

.content {
  background-color: wheat;
  height: 100%;
}

.container {
  margin: 1rem;
  border: 2px dashed red;
  overflow: hidden;
}
```

Writing a container query via the {{Cssxref("@container")}} at-rule will apply styles to the elements of the container when it is wider than 400px:

```css
@container (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 1fr 2fr;
  }
}
```

{{EmbedLiveSample('Establishing_inline_size_containment', '100%', 300)}}

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [CSS container queries](/en-US/docs/Web/CSS/CSS_containment/Container_queries)
- [Using container size and style queries](/en-US/docs/Web/CSS/CSS_containment/Container_size_and_style_queries)
- [Using container scroll-state queries](/en-US/docs/Web/CSS/CSS_conditional_rules/Container_scroll-state_queries)
- {{Cssxref("@container")}} at-rule
- CSS {{Cssxref("container")}} shorthand property
- CSS {{Cssxref("container-name")}} property
- CSS {{cssxref("content-visibility")}} property
