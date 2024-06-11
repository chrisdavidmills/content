---
title: anchor-name
slug: Web/CSS/anchor-name
page-type: css-property
status:
  - experimental
browser-compat: css.properties.anchor-name
---

{{CSSRef}}{{seecompattable}}

The **`anchor-name`** [CSS](/en-US/docs/Web/CSS) property enables defining an element as an **anchor element** by giving it one or more identifying **anchor names**. Each included name can then be set as the value of a positioned element's {{cssxref("position-anchor")}} property to associate it with its anchor.

## Syntax

```css
/* Single values */
anchor-name: none;
anchor-name: --name;

/* Multiple values */
anchor-name: --name, --another-name;

/* Global values */
anchor-name: inherit;
anchor-name: initial;
anchor-name: revert;
anchor-name: revert-layer;
anchor-name: unset;
```

### Values

- `none`

  - : The default value. Setting `anchor-name: none` on an element means that it is not defined as an anchor element. If the element was previously defined as an anchor and associated with a positioned element, setting `anchor-name: none` disassociates the two.

- {{cssxref("dashed-ident")}}
  - : One or more comma-separated arbitrary custom identifiers defining the name or names of the anchor, which can then be referenced in a {{cssxref("position-anchor")}} property.

## Description

To position an element relative to an anchor element, the positioned element requires three features: an association, a position, and a location. The `anchor-name` and {{cssxref("position-anchor")}} properties provide the association. The anchor element accepts one or more `<dashed-ident>` anchor names set on it via the `anchor-name` property. When one of those names is then set as the value of the positioned element's `position-anchor` property, the two elements are associated. The two element become tethered by setting a {{cssxref("position")}} of `absolute` or `fixed` on the associated element, making it a "positioned" element.

If multiple anchor elements have the same anchor name set on them, and that name is referenced by the `position-anchor` property value of a positioned element, that positioned element will be associated with the last anchor element with that anchor name in the source order.

Anchor positioning changes the [containing block](/en-US/docs/Web/CSS/Containing_block) of anchor-positioned elements, making its `position` relative to its anchor rather than to the nearest positioned ancestor element.

To tether and place a positioned element in a specific location relative to an anchor element, an anchor positioning feature such as the {{cssxref("anchor()")}} function on an {{glossary("inset properties", "inset property")}} or the {{cssxref("inset-area")}} property is needed.

You cannot associate a positioned element with an anchor element if the anchor is hidden, such as with {{cssxref("display", "display: none")}} or {{cssxref("visibility", "visibility: hidden")}}, or if the anchor is part of the [skipped contents](/en-US/docs/Web/CSS/CSS_containment/Using_CSS_containment#skips_its_contents) of another element due to it having {{cssxref("content-visibility", "content-visibility: hidden")}} set on it.

The `anchor-name` property is supported on all elements that generate a principal box. This means that [pseudo-elements](/en-US/docs/Web/CSS/Pseudo-elements), including generated content created using {{cssxref("::before")}} and {{cssxref("::after")}}, and UI features like the [`range` input] thumb () {{cssxref("::-webkit-slider-thumb")}} can be anchor elements. Pseudo elements are implicitly anchored to the same element as the pseudo-element's originating element, unless otherwise specified.

For more information on anchor features and usage, see the [CSS anchor positioning](/en-US/docs/Web/CSS/CSS_anchor_positioning) module landing page and the [Using CSS anchor positioning](/en-US/docs/Web/CSS/CSS_anchor_positioning/Using) guide.

## Formal definition

{{cssinfo}}

## Formal syntax

{{csssyntax}}

## Examples

### Basic usage

This example tethers a positioned element to an anchor, positioning the element to the right of the anchor.

#### HTML

We specify two {{htmlelement("div")}} elements; an anchor element with a class of `anchor` and a positioned element with a class of `infobox`.

We also include some filler text around the two `<div>`s to make the {{htmlelement("body")}} taller so that it will scroll.

```html
<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
  incididunt ut labore et dolore magna aliqua. Dui nunc mattis enim ut tellus
  elementum sagittis vitae et.
</p>

<div class="anchor">⚓︎</div>

<div class="infobox">
  <p>This is an information box.</p>
</div>

<p>
  Nisi quis eleifend quam adipiscing vitae proin sagittis nisl rhoncus. In arcu
  cursus euismod quis viverra nibh cras pulvinar. Vulputate ut pharetra sit amet
  aliquam.
</p>

<p>
  Malesuada nunc vel risus commodo viverra maecenas accumsan lacus. Vel elit
  scelerisque mauris pellentesque pulvinar pellentesque habitant morbi
  tristique. Porta lorem mollis aliquam ut porttitor. Turpis cursus in hac
  habitasse platea dictumst quisque. Dolor sit amet consectetur adipiscing elit.
  Ornare lectus sit amet est placerat. Nulla aliquet porttitor lacus luctus
  accumsan.
</p>
```

#### CSS

We first declare the `anchor` `<div>` as an anchor element by setting an anchor name on it via the `anchor-name` property:

```css hidden
body {
  width: 50%;
  margin: 0 auto;
}

.anchor {
  font-size: 1.8rem;
  color: white;
  text-shadow: 1px 1px 1px black;
  background-color: hsl(240 100% 75%);
  width: fit-content;
  border-radius: 10px;
  border: 1px solid black;
  padding: 3px;
}
```

```css
.anchor {
  anchor-name: --myAnchor;
}
```

We associate the second `<div>` with the anchor element by setting its anchor name as the value of the positioned element's {{cssxref("position-anchor")}} property.

We include CSS to convert the `infobox` element into an **anchor-positioned element** and position it relative to its anchor by setting the following on the `infobox`:

- {{cssxref("position")}} property to `fixed`, converting it to a **positioned element** so it can be positioned relative to the anchor's position on the page.
- {{cssxref("left")}} property to an {{cssxref("anchor()")}} function with a value of `right`, tethering the positioned element to it's anchor . This positions the infobox's left edge flush to the right edge of its anchor.
- {{cssxref("align-self")}} property to `anchor-center`, centrally aligning the center of the anchor in the inline direction.
- {{cssxref("margin-left")}} to `10px`, creating space between the anchor positioned element and its anchor.

```css hidden
.infobox {
  color: darkblue;
  background-color: azure;
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 10px;
  font-size: 1rem;
}
```

```css
.infobox {
  position-anchor: --myAnchor;
  position: fixed;
  left: anchor(right);
  align-self: anchor-center;
  margin-left: 10px;
}
```

#### Result

Scroll the page to see how the infobox is positioned relative to the anchor; as the anchor scrolls upwards, the positioned element moves along with it.

{{ EmbedLiveSample("Basic usage", "100%", "225") }}

### Multiple positioned elements

This example demonstrates how you can associate multiple positioned elements with one anchor.

#### HTML

The HTML is the same as for the previous example, except this time we have multiple positioned element `<div>`s with different [`id`](/en-US/docs/Web/HTML/Global_attributes/id)s to identify them.

```html
<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
  incididunt ut labore et dolore magna aliqua. Dui nunc mattis enim ut tellus
  elementum sagittis vitae et.
</p>

<div class="anchor">⚓︎</div>

<div class="infobox" id="infobox1">
  <p>This is an information box.</p>
</div>

<div class="infobox" id="infobox2">
  <p>This is another information box.</p>
</div>

<p>
  Nisi quis eleifend quam adipiscing vitae proin sagittis nisl rhoncus. In arcu
  cursus euismod quis viverra nibh cras pulvinar. Vulputate ut pharetra sit amet
  aliquam.
</p>

<p>
  Malesuada nunc vel risus commodo viverra maecenas accumsan lacus. Vel elit
  scelerisque mauris pellentesque pulvinar pellentesque habitant morbi
  tristique. Porta lorem mollis aliquam ut porttitor. Turpis cursus in hac
  habitasse platea dictumst quisque. Dolor sit amet consectetur adipiscing elit.
  Ornare lectus sit amet est placerat. Nulla aliquet porttitor lacus luctus
  accumsan.
</p>
```

#### CSS

The CSS is mostly the same as the previous example too. We declare the `anchor` `<div>` as an anchor element using the `anchor-name` property, giving it an anchor name as before.

```css hidden
body {
  width: 50%;
  margin: 0 auto;
}

.anchor {
  font-size: 1.8rem;
  color: white;
  text-shadow: 1px 1px 1px black;
  background-color: hsl(240 100% 75%);
  width: fit-content;
  border-radius: 10px;
  border: 1px solid black;
  padding: 3px;
}
```

```css
.anchor {
  anchor-name: --myAnchor;
}
```

Each of the two positioned elements are associated with the anchor element by setting its anchor name the positioned element's {{cssxref("position-anchor")}} property value. Both are also positioned `fixed`, making them **anchor positioned elements**. The positioned elements are then positioned in different places relative to the anchor using a combination of inset, alignment, and margin properties.

```css hidden
.infobox {
  color: darkblue;
  background-color: azure;
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 10px;
  font-size: 1rem;
}
```

```css
.infobox {
  position-anchor: --myAnchor;
  position: fixed;
}

#infobox1 {
  left: anchor(right);
  align-self: anchor-center;
  margin-left: 10px;
}

#infobox2 {
  bottom: anchor(top);
  justify-self: anchor-center;
  margin-bottom: 15px;
}
```

#### Result

Scroll the page to see how both of the infoboxes are tethered to the anchor.

{{ EmbedLiveSample("Multiple positioned elements", "100%", "225") }}

### Multiple anchor names

This example demonstrates how an anchor element can have more than one anchor name.

#### HTML

The HTML is the same as for the previous example.

```html
<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
  incididunt ut labore et dolore magna aliqua. Dui nunc mattis enim ut tellus
  elementum sagittis vitae et.
</p>

<div class="anchor">⚓︎</div>

<div class="infobox" id="infobox1">
  <p>This is an information box.</p>
</div>

<div class="infobox" id="infobox2">
  <p>This is another information box.</p>
</div>

<p>
  Nisi quis eleifend quam adipiscing vitae proin sagittis nisl rhoncus. In arcu
  cursus euismod quis viverra nibh cras pulvinar. Vulputate ut pharetra sit amet
  aliquam.
</p>

<p>
  Malesuada nunc vel risus commodo viverra maecenas accumsan lacus. Vel elit
  scelerisque mauris pellentesque pulvinar pellentesque habitant morbi
  tristique. Porta lorem mollis aliquam ut porttitor. Turpis cursus in hac
  habitasse platea dictumst quisque. Dolor sit amet consectetur adipiscing elit.
  Ornare lectus sit amet est placerat. Nulla aliquet porttitor lacus luctus
  accumsan.
</p>
```

#### CSS

The CSS is the same as the previous example too, except we include two comma-separated names in the target's `anchor-name` property value and each positioned element has a different value for `position-anchor`.

```css hidden
body {
  width: 50%;
  margin: 0 auto;
}

.anchor {
  font-size: 1.8rem;
  color: white;
  text-shadow: 1px 1px 1px black;
  background-color: hsl(240 100% 75%);
  width: fit-content;
  border-radius: 10px;
  border: 1px solid black;
  padding: 3px;
}

.infobox {
  color: darkblue;
  background-color: azure;
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 10px;
  font-size: 1rem;
}
```

```css
.anchor {
  anchor-name: --anchor1, --anchor2;
}

.infobox {
  position: fixed;
}

#infobox1 {
  position-anchor: --anchor1;
  left: anchor(right);
  align-self: anchor-center;
  margin-left: 10px;
}

#infobox2 {
  position-anchor: --anchor2;
  bottom: anchor(top);
  justify-self: anchor-center;
  margin-bottom: 15px;
}
```

#### Result

Scroll the page to see how both of the infoboxes are tethered to the anchor.

{{ EmbedLiveSample("Multiple anchor names", "100%", "225") }}

### Multiple positioned elements and anchors

This example demonstrates how you can move multiple positioned elements around, associating them with different anchors.

#### HTML

We have four anchors and two positioned elements, distinguished with different `id` values. The positioned elements contain {{htmlelement("select")}} boxes that allow you to choose which anchor you want to associate them with.

```html
<div id="anchor-container">
  <div class="anchor" id="anchor1">⚓︎</div>
  <div class="anchor" id="anchor2">⚓︎</div>
  <div class="anchor" id="anchor3">⚓︎</div>
  <div class="anchor" id="anchor4">⚓︎</div>
</div>

<div class="infobox" id="infobox1">
  <form>
    <label for="myanchor1-anchor-select">Place infobox on:</label>
    <select id="myanchor1-anchor-select">
      <option value="1">Anchor 1</option>
      <option value="2">Anchor 2</option>
      <option value="3">Anchor 3</option>
      <option value="4">Anchor 4</option>
    </select>
  </form>
</div>

<div class="infobox" id="infobox2">
  <form>
    <label for="myanchor2-anchor-select">Place infobox on:</label>
    <select id="myanchor2-anchor-select">
      <option value="1">Anchor 1</option>
      <option value="2">Anchor 2</option>
      <option value="3">Anchor 3</option>
      <option value="4">Anchor 4</option>
    </select>
  </form>
</div>
```

#### CSS

We declare the first `anchor` `<div>` as an anchor using the `anchor-name` property, which is given two comma-separated anchor names, one for each positioned element. This is the initial state of the demo — both positioned elements will be tethered to the first anchor.

```css hidden
body {
  height: 100vh;
}

#anchor-container {
  display: grid;
  height: 100%;
  grid-template-columns: 1fr 1fr;
  grid-auto-rows: 1fr 1fr;
  justify-items: center;
  align-items: center;
}

.anchor {
  font-size: 1.8rem;
  color: white;
  text-shadow: 1px 1px 1px black;
  background-color: hsl(240 100% 75%);
  width: fit-content;
  border-radius: 10px;
  border: 1px solid black;
  padding: 3px;
}
```

```css
#anchor1 {
  anchor-name: --myAnchor1, --myAnchor2;
}
```

Each of the two positioned elements are given a {{cssxref("position-anchor")}} property with a value matching one of the anchor names. The positioned elements are then given anchor-relative positioning information using a combination of inset, alignment, and margin properties.

```css hidden
.infobox {
  color: darkblue;
  background-color: azure;
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 10px;
  font-size: 1rem;
  width: 120px;
}
```

```css
#infobox1 {
  position-anchor: --myAnchor1;

  position: fixed;
  left: anchor(right);
  align-self: anchor-center;
  margin-left: 10px;
}

#infobox2 {
  position-anchor: --myAnchor2;

  position: fixed;
  bottom: anchor(top);
  justify-self: anchor-center;
  margin-bottom: 15px;
}
```

#### JavaScript

We dynamically change the `anchor-name` values set on the anchor elements in response to different anchors being selected in the positioned elements' `<select>` menus. The key functionality here is the [`change`](/en-US/docs/Web/API/HTMLElement/change_event) event handler, `updateAnchorNames()`. It sets both anchor names on one anchor, if the anchors chosen in the two `<select>` menus are the same. Otherwise, it sets a single anchor name on two separate anchors as appropriate.

```js
// Get references to the two select menus
const select1 = document.querySelector("#myanchor1-anchor-select");
const select2 = document.querySelector("#myanchor2-anchor-select");
// Store references to all the anchors in a NodeList (array-like)
const anchors = document.querySelectorAll("#anchor-container > div");

// Set the same change event handler on both select menus
select1.addEventListener("change", updateAnchorNames);
select2.addEventListener("change", updateAnchorNames);

function updateAnchorNames() {
  // Remove all anchor names fomr all anchors
  for (const anchor of anchors) {
    anchor.style.anchorName = "none";
  }

  // convert the select menu values to numbers, and remove one to
  // make them match the selected anchors' index positions in the NodeList
  const value1 = Number(select1.value) - 1;
  const value2 = Number(select2.value) - 1;

  if (value1 === value2) {
    // If the chosen anchors are both the same, set both anchor
    // names on the same anchor
    anchors[value1].style.anchorName = "--myAnchor1, --myAnchor2";
  } else {
    // If they are not the same, set the anchor names separately
    // on each selected anchor
    anchors[value1].style.anchorName = "--myAnchor1";
    anchors[value2].style.anchorName = "--myAnchor2";
  }
}
```

#### Result

Select different values from the drop-down menus to change the anchors that the elements are positioned relative to.

{{ EmbedLiveSample("Multiple positioned elements and anchors", "100%", "400") }}

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{cssxref("position-anchor")}}
- HTML [`anchor`](/en-US/docs/Web/HTML/Global_attributes/anchor) attribute
- [CSS anchor positioning](/en-US/docs/Web/CSS/CSS_anchor_positioning) module
- [Using CSS anchor positioning](/en-US/docs/Web/CSS/CSS_anchor_positioning/Using)
