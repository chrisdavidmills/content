---
title: Accessibility tree
slug: Glossary/Accessibility_tree
page-type: glossary-definition
sidebar: glossarysidebar
---

The **accessibility tree** contains {{Glossary("accessibility")}}-related information for most HTML elements.

Browsers convert markup into an internal representation called the _[DOM tree](/en-US/docs/Web/API/Document_Object_Model/Using_the_Document_Object_Model)_. The DOM tree contains objects representing all the markup's elements, attributes, and text nodes. Browsers then create an accessibility tree based on the DOM tree, which is used by platform-specific Accessibility APIs to provide a representation that can be understood by assistive technologies, such as screen readers.

There are four properties in an accessibility tree object:

- **name**
  - : How can we refer to this thing? For instance, a link with the text "Read more" will have "Read more" as its name (find more on how names are computed in the [Accessible Name and Description Computation spec](https://w3c.github.io/accname/)).
- **description**
  - : How do we describe this thing, if we want to provide more description beyond the name? The description of a table could explain what kind of information the table contains.
- [**role**](/en-US/docs/Web/Accessibility/ARIA/Reference/Roles)
  - : What kind of thing is it? For example, is it a button, a nav bar, or a list of items?
- [**state**](/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes)
  - : Does it have a state? Examples include checked or unchecked checkbox states and collapsed or expanded states for the [`<summary>`](/en-US/docs/Web/HTML/Reference/Elements/summary) element.

Additionally, the accessibility tree often contains information on what can be done with an element: a link can be _followed_, a text input can be _typed into_, etc.

While still in draft form within the Web Incubator Community Group as of April 2022, the **[Accessibility Object Model](https://wicg.github.io/aom/explainer.html) (AOM)** intends to incubate APIs that make it easier to express accessibility semantics and potentially allow read access to the computed accessibility tree.

## See also

- [Accessibility](/en-US/docs/Web/Accessibility)
- [Learn accessibility](/en-US/docs/Learn_web_development/Core/Accessibility)
- [Web accessibility](https://en.wikipedia.org/wiki/Web_accessibility) on Wikipedia
- [Web Accessibility In Mind](https://webaim.org/)
- [ARIA](/en-US/docs/Web/Accessibility/ARIA)
- [The W3C Web Accessibility Initiative (WAI)](https://www.w3.org/WAI/)
- [Accessible Rich Internet Applications (WAI-ARIA)](https://w3c.github.io/aria/)
- Related glossary terms:
  - {{Glossary("Accessibility")}}
  - {{Glossary("Accessible description")}}
  - {{Glossary("Accessible name")}}
  - {{Glossary("ARIA")}}
