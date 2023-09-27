---
title: Using the Popover API
slug: Web/API/Popover_API/Using
page-type: guide
status:
  - experimental
---

{{DefaultAPISidebar("Popover API")}}

The **Popover API** provides developers with a standard, consistent, flexible mechanism for displaying popover content on top of other page content. Popover content can be controlled either declaratively using HTML attributes, or via JavaScript. This article provides a detailed guide to using all of its features.

## Creating declarative popovers

In its simplest form, a popover is created by adding the [`popover`](/en-US/docs/Web/HTML/Global_attributes/popover) attribute to the element that you want to contain your popover content. An `id` is also needed to associate the popover with its controls.

```html
<div id="mypopover" popover>Popover content</div>
```

> **Note:** Setting the `popover` attribute with no value is equivalent to setting `popover="auto"`.

Adding this attribute causes the element to be hidden on page load by having {{cssxref("display", "display: none")}} set on it. To show/hide the popover, you need to add some control buttons. You can set a {{htmlelement("button")}} (or {{htmlelement("input")}} of `type="button"`) as a popover control button by giving it a [`popovertarget`](/en-US/docs/Web/HTML/Element/button#popovertarget) attribute, the value of which should be the ID of the popover to control:

```html
<button popovertarget="mypopover">Toggle the popover</button>
<div id="mypopover" popover>Popover content</div>
```

The default behavior is for the button to be a toggle button — pressing it repeatedly will toggle the popover between showing and hidden.

If you want to change that behavior, you can use the [`popovertargetaction`](/en-US/docs/Web/HTML/Element/button#popovertargetaction) attribute — this takes a value of `"hide"`, `"show"`, or `"toggle"`. For example, to create separate show and hide buttons, you could do this:

```html
<button popovertarget="mypopover" popovertargetaction="show">
  Show popover
</button>
<button popovertarget="mypopover" popovertargetaction="hide">
  Hide popover
</button>
<div id="mypopover" popover>Popover content</div>
```

You can see how the previous code snippet renders in our [Basic declarative popover example](https://mdn.github.io/dom-examples/popover-api/basic-declarative/) ([source](https://github.com/mdn/dom-examples/tree/main/popover-api/basic-declarative)).

> **Note:** If the `popovertargetaction` attribute is omitted, `"toggle"` is the default action that will be performed by a control button.

When a popover is shown, it has `display: none` removed from it and it is put into the {{glossary("top layer")}} so it will sit on top of all other page content.

## auto state, and "light dismiss"

When a popover element is set with `popover` or `popover="auto"` as shown above, it is said to have **auto state**. The two important behaviors to note about auto state are:

- The popover can be "light dismissed" — this means that you can hide the popover by clicking outside it or pressing the <kbd>Esc</kbd> key.
- Usually, only one popover can be shown at a time — showing a second popover when one is already shown will hide the first one. The exception to this rule is when you have nested auto popovers. See the [Nested popovers](#nested_popovers) section for more details.

> **Note:** `popover="auto"` popovers are also dismissed by successful {{domxref("HTMLDialogElement.showModal()")}} and {{domxref("Element.requestFullscreen()")}} calls on other elements in the document. Bear in mind however that calling these methods on a shown popover will result in failure because those behaviors don't make sense on an already-shown popover. You can however call them on an element with the `popover` attribute that isn't currently being shown.

Auto state is useful when you only want to show a single popover at once. Perhaps you have multiple teaching UI messages that you want to show, but don't want the display to start getting cluttered and confusing, or perhaps you are showing status messages where the new status overrides any previous status.

You can see the behavior described above in action in our [Multiple auto popovers example](https://mdn.github.io/dom-examples/popover-api/multiple-auto/) ([source](https://github.com/mdn/dom-examples/tree/main/popover-api/multiple-auto)). Try light dismissing the popovers after they are shown, and see what happens when you try to show both at the same time.

## Using manual popover state

The alternative to auto state is **manual state**, achieved by setting `popover="manual"` on your popover element:

```html
<div id="mypopover" popover="manual">Popover content</div>
```

In this state:

- The popover cannot be "light dismissed", although declarative show/hide/toggle buttons (as seen earlier) will still work.
- Multiple independent popovers can be shown at a time.

You can see this behavior in action in our [Multiple manual popovers example](https://mdn.github.io/dom-examples/popover-api/multiple-manual/) ([source](https://github.com/mdn/dom-examples/tree/main/popover-api/multiple-manual)).

## Showing popovers via JavaScript

You can also control popovers using a JavaScript API.

The {{domxref("HTMLElement.popover")}} property can be used to get or set the [`popover`](/en-US/docs/Web/HTML/Global_attributes/popover) attribute. This can be used to create a popover via JavaScript, and is also useful for feature detection. For example:

```js
function supportsPopover() {
  return HTMLElement.prototype.hasOwnProperty("popover");
}
```

Similarly:

- {{domxref("HTMLButtonElement.popoverTargetElement")}} and {{domxref("HTMLInputElement.popoverTargetElement")}} provide an equivalent to the [`popovertarget`](/en-US/docs/Web/HTML/Element/button#popovertarget) attribute, allowing you to set up the control button(s) for a popover, although the property value taken is a reference to the popover DOM element to control.
- {{domxref("HTMLButtonElement.popoverTargetAction")}} and {{domxref("HTMLInputElement.popoverTargetAction")}} provide an equivalent to the [`popovertargetaction`](/en-US/docs/Web/HTML/Element/button#popovertargetaction) global HTML attribute, allowing you to specify the action taken by a control button.

Putting these three together, you can programmatically set up a popover and its control button, like so:

```js
const popover = document.getElementById("mypopover");
const toggleBtn = document.getElementById("toggleBtn");

const keyboardHelpPara = document.getElementById("keyboard-help-para");

const popoverSupported = supportsPopover();

if (popoverSupported) {
  popover.popover = "auto";
  toggleBtn.popoverTargetElement = popover;
  toggleBtn.popoverTargetAction = "toggle";
} else {
  toggleBtn.style.display = "none";
}
```

You also have several methods to control showing and hiding:

- {{domxref("HTMLElement.showPopover()")}} to show a popover.
- {{domxref("HTMLElement.hidePopover()")}} to hide a popover.
- {{domxref("HTMLElement.togglePopover()")}} to toggle a popover.

For example, you might want to provide the ability to toggle a help popover on and off by clicking a button or pressing a particular key on the keyboard. The first one could be achieved declaratively, or you could do it using JavaScript as shown above.

For the second one, you could create an event handler that programs two separate keys — one to open the popover and one to close it again:

```js
document.addEventListener("keydown", (event) => {
  if (event.key === "h") {
    if (popover.matches(":popover-open")) {
      popover.hidePopover();
    }
  }

  if (event.key === "s") {
    if (!popover.matches(":popover-open")) {
      popover.showPopover();
    }
  }
});
```

This example uses {{domxref("Element.matches()")}} to programmatically check whether a popover is currently showing. The {{cssxref(":popover-open")}} pseudo-class matches only popovers that are currently being shown. This is important to avoid the errors that are thrown if you try to show an already-shown popover, or hide an already-hidden popover.

Alternatively, you could program a single key to show _and_ hide the popover like this:

```js
document.addEventListener("keydown", (event) => {
  if (event.key === "h") {
    popover.togglePopover();
  }
});
```

See our [Toggle help UI example](https://mdn.github.io/dom-examples/popover-api/toggle-help-ui/) ([source](https://github.com/mdn/dom-examples/tree/main/popover-api/toggle-help-ui)) to see the popover JavaScript properties, feature detection, and `togglePopover()` method in action.

## Dismissing popovers automatically via a timer

Another common pattern in JavaScript is dismissing popovers automatically after a certain amount of time. You might for example want to create a system of "toast" notifications, where you have multiple actions underway at a time (for example multiple files uploading), and want to show a notification when each one succeeds or fails. For this you'll want to use manual popovers so you can show several at the same time, and use {{domxref("setTimeout")}} to remove them. A function for handling such popovers might look like so:

```js
function makeToast(result) {
  const popover = document.createElement("article");
  popover.popover = "manual";
  popover.classList.add("toast");
  popover.classList.add("newest");

  let msg;

  if (result === "success") {
    msg = "Action was successful!";
    popover.classList.add("success");
    successCount++;
  } else if (result === "failure") {
    msg = "Action failed!";
    popover.classList.add("failure");
    failCount++;
  } else {
    return;
  }

  popover.textContent = msg;
  document.body.appendChild(popover);
  popover.showPopover();

  updateCounter();

  setTimeout(() => {
    popover.hidePopover();
    popover.remove();
  }, 4000);
}
```

You can also use the {{domxref("HTMLElement.beforetoggle_event", "beforetoggle")}} event to perform a follow-up action when a popover shows or hides. In our example we execute a `moveToastsUp()` function to make the toasts all move up the screen to make way each time a new toast appears:

```js
popover.addEventListener("toggle", (event) => {
  if (event.newState === "open") {
    moveToastsUp();
  }
});

function moveToastsUp() {
  const toasts = document.querySelectorAll(".toast");

  toasts.forEach((toast) => {
    if (toast.classList.contains("newest")) {
      toast.style.bottom = `5px`;
      toast.classList.remove("newest");
    } else {
      const prevValue = toast.style.bottom.replace("px", "");
      const newValue = parseInt(prevValue) + 50;
      toast.style.bottom = `${newValue}px`;
    }
  });
}
```

See our [Toast popover example](https://mdn.github.io/dom-examples/popover-api/toast-popovers/) ([source](https://github.com/mdn/dom-examples/tree/main/popover-api/toast-popovers)) to see the above example snippets in action, and full comments for explanation.

## Nested popovers

There is an exception to the rule about not displaying multiple auto popovers at once — when they are nested inside one another. In such cases, multiple popovers are allowed to both be open at the same time, due to their relationship with each other. This pattern is supported to enable use cases such as nested popover menus.

There are three different ways to create nested popovers:

1. Direct DOM descendants:

   ```html
   <div popover>
     Parent
     <div popover>Child</div>
   </div>
   ```

2. Via invoking/control elements:

   ```html
   <div popover>
     Parent
     <button popovertarget="foo">Click me</button>
   </div>

   <div popover id="foo">Child</div>
   ```

3. Via the `anchor` attribute:

   ```html
   <div popover id="foo">Parent</div>

   <div popover anchor="foo">Child</div>
   ```

See our [Nested popover menu example](https://mdn.github.io/dom-examples/popover-api/nested-popovers/) ([source](https://github.com/mdn/dom-examples/tree/main/popover-api/nested-popovers)) for an example. You'll notice that quite a few event handlers have been used to display and hide the subpopover appropriately during mouse and keyboard access, and also to hide both menus when an option is selected from either. Depending on how you handle loading of new content , either in an SPA or multi-page website, some of all of these may not be necessary, but they have been included in this demo for illustrative purposes.

## Styling popovers

The popover API has some related CSS features available that are worth looking at.

In terms of styling the actual popover, you can select all popovers with a simple attribute selector (`[popover]`), or you select popovers that are showing using a new pseudo-class — {{cssxref(":popover-open")}}.

When looking at the first couple of examples linked at the start of the article, you may have noticed that the popovers appear in the middle of the viewport. This is the default styling, achieved like this in the UA stylesheet:

```css
[popover] {
  position: fixed;
  inset: 0;
  width: fit-content;
  height: fit-content;
  margin: auto;
  border: solid;
  padding: 0.25em;
  overflow: auto;
  color: CanvasText;
  background-color: Canvas;
}
```

To override the default styles and get the popover to appear somewhere else on your viewport, you would need to override the above styles with something like this:

```css
:popover-open {
  width: 200px;
  height: 100px;
  position: absolute;
  inset: unset;
  bottom: 5px;
  right: 5px;
  margin: 0;
}
```

You can see an isolated example of this in our [Popover positioning example](https://mdn.github.io/dom-examples/popover-api/popover-positioning/) ([source](https://github.com/mdn/dom-examples/tree/main/popover-api/popover-positioning)).

The {{cssxref("::backdrop")}} pseudo-element is a full-screen element placed directly behind showing popover elements in the {{glossary("top layer")}}, allowing effects to be added to the page content behind the popover(s) if desired. You might for example want to blur out the content behind the popover to help focus the user's attention on it:

```css
::backdrop {
  backdrop-filter: blur(3px);
}
```

See our [Popover blur background example](https://mdn.github.io/dom-examples/popover-api/blur-background/) ([source](https://github.com/mdn/dom-examples/tree/main/popover-api/blur-background)) for an idea of how this renders.

## Animating popovers

For popovers to be animated, several different features are required.

First of all, popovers are set to `display: none;` when hidden and `display: block;` when shown, as well as being removed from / added to the {{glossary("top layer")}}. Therefore, `display` needs to be animatable. This is now the case; [supporting browsers](/en-US/docs/Web/CSS/display#browser_compatibility) animate `display` with a variation on the [discrete animation type](/en-US/docs/Web/CSS/CSS_animated_properties#discrete). Specifically, the browser will flip between `none` and another value of `display` so that the animated content is shown for `100%` of the animation duration. So for example:

- When animating between `display` `none` and `block`, the value will flip to `block` at `0%` of the animation duration so it is visible throughout.
- When animating between `display` `block` and `none`, the value will flip to `none` at `100%` of the animation duration so it is visible throughout.

### Transitioning a popover

When animating popovers with transitions, these additional features are needed:

- [`@starting-style`](/en-US/docs/Web/CSS/@starting-style) is used to provide a set of starting values for properties set on the popover that you want to transition from when it is shown. This is needed because, by default, [CSS transitions](/en-US/docs/Web/CSS/CSS_transitions) are not triggered on elements' first style updates, or when the `display` type changes from `none` to another type, to avoid unexpected behavior.
- [`display`](/en-US/docs/Web/CSS/display) needs to be added to the transitions list so that the popover will remain as `display: block` for the duration of the animation, allowing the other animations to be seen.
- [`overlay`](/en-US/docs/Web/CSS/overlay) needs to be added to the transitions list so that the removal of the popover from the top layer will be deferred until the animation is finished, again allowing the other animations to be seen.
- [`transition-behavior: allow-discrete`](/en-US/docs/Web/CSS/transition-behavior) needs to be set on the `display` and `overlay` transitions. This effectively enables discrete transitions.

Let's have a look at an example so you can see what this looks like:

The HTML contains a {{htmlelement("div")}} element declared as a popover, and a {{htmlelement("button")}} element designated as the popover's toggle control:

```html
<button popovertarget="mypopover">Toggle the popover</button>
<div popover="auto" id="mypopover">I'm a Popover! I should animate.</div>
```

The CSS for the example looks like this:

```css
html {
  font-family: Arial, Helvetica, sans-serif;
}

/* Transition for the popover itself */

[popover]:popover-open {
  opacity: 1;
  transform: scaleX(1);
}

[popover] {
  font-size: 1.2rem;
  padding: 10px;
  opacity: 0;
  transform: scaleX(0);
  transition:
    opacity 0.7s,
    transform 0.7s,
    overlay 0.7s allow-discrete,
    display 0.7s allow-discrete;
}

/* Needs to be after the previous [popover]:popover-open rule to take effect,
    as the specificity is the same */
@starting-style {
  [popover]:popover-open {
    opacity: 0;
    transform: scaleX(0);
  }
}

/* Transition for the popover's backdrop */

[popover]::backdrop {
  background-color: rgba(0, 0, 0, 0);
  transition:
    display 0.7s allow-discrete,
    overlay 0.7s allow-discrete,
    background-color 0.7s;
}

[popover]:popover-open::backdrop {
  background-color: rgba(0, 0, 0, 0.25);
}

/* This starting-style rule cannot be nested inside the above selector
because the nesting selector cannot represent pseudo-elements. */

@starting-style {
  [popover]:popover-open::backdrop {
    background-color: rgba(0, 0, 0, 0);
  }
}
```

The two popover properties we want to show animations for are [`opacity`](/en-US/docs/Web/CSS/opacity) and [`transform`](/en-US/docs/Web/CSS/transform) — specifically, a horizontally scaling transform. We want the popover to fade in and out, as well as growing/shrinking horizontally. To achieve this, we have set a starting state for these properties on the default hidden state of the popover element (selected via `[popover]`), and an end state on the open state of the popover (selected via the [`:popover-open`](/en-US/docs/Web/CSS/:popover-open) pseudo-class). We then set a [`transition`](/en-US/docs/Web/CSS/transition) property to animate between the two.

And as discussed earlier, we have:

- Set a starting state for the transition inside the `@starting-style` block.
- Added `display` to the list of transitioned elements so that the animated element is visible (set to `display: block`) throughout both the entry and exit animation. Without this, the exit animation would not be visible; in effect, the popover would just disappear.
- Added `overlay` to the list of transitioned elements to make sure that the removal of the element from the top layer is deferred until the animation has been completed. This doesn't make a huge difference for simple animations such as this one, but in more complex cases not doing this can result in the element being removed from the overlay too quickly, meaning the animation is not smooth or effective.
- Set `allow-discrete` on both the above transitions to enable discrete transitions.

You'll note that we've also included a transition on the [`::backdrop`](/en-US/docs/Web/CSS/::backdrop) that appears behind the popover when it opens, to provide a nice darkening animation. `[popover]:popover-open::backdrop` is needed to select the backdrop when the popover is open.

The code renders as follows:

{{ EmbedLiveSample("Transitioning a popover", "100%", "200") }}

### A popover keyframe animation

When animating a popover with [CSS animations](/en-US/docs/Web/CSS/CSS_animations), there are some differences to note:

- You don't provide a starting state inside a `@starting-style` block; instead, you need to provide the starting `display` value in an explicit starting keyframe (for example using `0%` or `from`).
- You don't need to enable discrete transitions; there is no equivalent to `allow-discrete` inside keyframes.
- You can't set `overlay` inside keyframes either. The best solution we found at this time to defer removal from the top layer was to use JavaScript — animating the popover and then using {{domxref("setTimeout()")}} to defer removal of the popover until after the animation is finished.

Let's look at an example. The HTML is contains a {{htmlelement("div")}} element declared as a popover, and a {{htmlelement("button")}} element designated as the popover's toggle control:

```html
<button id="toggle-button">Toggle the popover</button>
<div popover="manual" id="mypopover">I'm a Popover! I should animate.</div>
```

The CSS is as follows:

```css
html {
  font-family: Arial, Helvetica, sans-serif;
}

[popover] {
  font-size: 1.2rem;
  padding: 10px;
}

/* Animation classes */

.fade-in {
  animation: fade-in 0.7s ease-out forwards;
}

.fade-out {
  animation: fade-out 0.7s ease-out forwards;
}

/* Animation keyframes */

@keyframes fade-in {
  0% {
    opacity: 0;
    transform: scaleX(0);
    display: none;
  }

  100% {
    opacity: 1;
    transform: scaleX(1);
    display: block;
  }
}

@keyframes fade-out {
  0% {
    opacity: 1;
    transform: scaleX(1);
    display: block;
  }

  100% {
    opacity: 0;
    transform: scaleX(0);
    display: none;
  }
}
```

We have defined keyframes that specify the desired entry and exit animations, and assigned those to CSS classes. This example doesn't animate the backdrop like the transitions example above did, as that didn't seem possible to reproduce with a keyframe animation in our experiments.

We then use some rudimentary JavaScript to apply those classes to the popover as it is shown and hidden, triggering the animations at the right times. As mentioned earlier, we have used `setTimeout()` to defer the hiding of the popover until the animations have finished.

```js
const popover = document.getElementById("mypopover");
const toggleBtn = document.getElementById("toggle-button");

toggleBtn.addEventListener("click", () => {
  if (!popover.matches(":popover-open")) {
    popover.classList.remove("fade-out");
    popover.classList.add("fade-in");
    popover.showPopover();
  } else if (popover.matches(":popover-open")) {
    popover.classList.remove("fade-in");
    popover.classList.add("fade-out");
    setTimeout(() => {
      popover.hidePopover();
    }, 700);
  }
});
```

The code renders as follows:

{{ EmbedLiveSample("A popover keyframe animation", "100%", "200") }}
