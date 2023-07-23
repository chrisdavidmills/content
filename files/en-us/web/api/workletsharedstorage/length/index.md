---
title: "WorkletSharedStorage: length() method"
short-title: length()
slug: Web/API/WorkletSharedStorage/length
page-type: web-api-instance-method
status:
  - experimental
browser-compat: api.WorkletSharedStorage.length
---

{{APIRef("Shared Storage API")}}{{SeeCompatTable}}

The **`length()`** method of the
{{domxref("WorkletSharedStorage")}} interface returns the number of entries currently stored in shared storage for the current origin.

## Syntax

```js-nolint
length()
```

### Parameters

None.

### Return value

A {{jsxref("Promise")}} that fulfills with a number representing the number of key/value pairs currently in shared storage.

### Exceptions

- {{jsxref("TypeError")}}
  - : Thrown if:
    - The worklet module has not yet been added with {{domxref("Worklet.addModule", "addModule()")}}.
    - The browser fails to retrieve the number of entries for some other reason.

## Examples

```js
// length() available inside a shared storage worklet module

async function retrieveLength() {
  const length = await this.sharedStorage.length();
  console.log(length);
}
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [Shared Storage API](/en-US/docs/Web/API/Shared_storage_API)
