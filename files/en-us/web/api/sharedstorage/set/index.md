---
title: "SharedStorage: set() method"
short-title: set()
slug: Web/API/SharedStorage/set
page-type: web-api-instance-method
status:
  - experimental
browser-compat: api.SharedStorage.set
---

{{APIRef("Shared Storage API")}}{{SeeCompatTable}}

The **`set()`** method of the
{{domxref("SharedStorage")}} interface stores a new key/value pair in the current origin's shared storage or updates an existing one.

## Syntax

```js-nolint
set(key, value)
set(key, value, options)
```

### Parameters

- `key`
  - : A string representing the key of the key/value pair you want to add or update.
- `value`
  - : A string representing the value you want to add or update.
- `options` {{optional_inline}}
  - : An options object containing the following properties:
    - `ignoreIfPresent`
      - : A boolean value that, if set to `true`, will cause the set operation to abort if a key/value pair with the specified `key` already exists. The default value, `false`, will cause the set operation to go ahead and overwrite the previous value, in such cases.

### Return value

A {{jsxref("Promise")}} that fulfills with `undefined`.

### Exceptions

- In the case of {{domxref("WindowSharedStorage")}}, if the `set()` operation doesn't successfully write to the database for a reason other than shared storage not being available, then it fails silently without rejecting.
- In the case of {{domxref("WorkletSharedStorage")}}, the `Promise` rejects with a {{jsxref("TypeError")}} if the worklet module has not been added with {{domxref("Worklet.addModule", "SharedStorageWorklet.addModule()")}}.
- In both cases, the `Promise` rejects with a {{jsxref("TypeError")}} if:
  - The created entry was not successfully stored in the database due to shared storage not being available (for example it is disabled using a browser setting).
  - `key` and/or `value` exceed the browser-defined maximum length.

## Examples

```js
window.sharedStorage
  .set("ab-testing-group", "0", {
    ignoreIfPresent: true,
  })
  .then(console.log("Set operation completed"));
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [Shared Storage API](/en-US/docs/Web/API/Shared_storage_API)
