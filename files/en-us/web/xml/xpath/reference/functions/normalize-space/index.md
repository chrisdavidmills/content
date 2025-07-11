---
title: normalize-space
slug: Web/XML/XPath/Reference/Functions/normalize-space
page-type: xpath-function
sidebar: xmlsidebar
---

The `normalize-space` function strips leading and trailing white-space from a string, replaces sequences of whitespace characters by a single space, and returns the resulting string.

## Syntax

```plain
normalize-space( [string] )
```

### Parameters

- `string` (optional)
  - : The string to be normalized. If omitted, string used will be the same as the context node converted to a string.

### Return value

The normalized string.

## Specifications

[XPath 1.0 4.2](https://www.w3.org/TR/xpath-10/#function-normalize-space)

## Gecko support

Supported.
