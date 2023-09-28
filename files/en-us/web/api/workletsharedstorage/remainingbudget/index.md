---
title: "WorkletSharedStorage: remainingBudget() method"
short-title: remainingBudget()
slug: Web/API/WorkletSharedStorage/remainingBudget
page-type: web-api-instance-method
status:
  - experimental
browser-compat: api.WorkletSharedStorage.remainingBudget
---

{{APIRef("Shared Storage API")}}{{SeeCompatTable}}

The **`remainingBudget()`** method of the
{{domxref("WorkletSharedStorage")}} interface returns the remaining navigation budget for the current origin.

The navigation budget is the number of bits of entropy permitted inside a {{htmlelement("fencedframe")}} as a result of {{domxref("WindowSharedStorage.selectURL()")}} calls, per origin, per 24-hour time period. This is not the same as the number of navigations; rather it is based on the number of potential navigations in each call. Each time a `selectURL()` navigation occurs, an amount equal to logarithm base 2 of the number of URL choices is deducted from the corresponding origin's budget.

Navigation budget is a mechanism designed to limit the rate of leakage of cross-site data to the destination pages navigated to in [fenced frames](/en-US/docs/Web/API/Fenced_Frame_API).

## Syntax

```js-nolint
remainingBudget()
```

### Parameters

None.

### Return value

A {{jsxref("Promise")}} that fulfills with a number representing the remaining navigation budget.

### Exceptions

- {{jsxref("TypeError")}}
  - : Thrown if the worklet module has not yet been added with {{domxref("Worklet.addModule", "addModule()")}}.

## Examples

```js
// remainingBudget() available inside a shared storage worklet module

async function retrieveBudget() {
  const budget = await this.sharedStorage.remainingBudget();
  console.log(budget);
}
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [Shared Storage API](/en-US/docs/Web/API/Shared_storage_API)
