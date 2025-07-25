---
title: "Notification: Notification() constructor"
short-title: Notification()
slug: Web/API/Notification/Notification
page-type: web-api-constructor
browser-compat: api.Notification.Notification
---

{{APIRef("Web Notifications")}}{{securecontext_header}} {{AvailableInWorkers}}

The **`Notification()`** constructor creates a new
{{domxref("Notification")}} object instance, which represents a user notification.

> [!NOTE]
> Trying to create a notification inside the {{domxref("ServiceWorkerGlobalScope")}} using the `Notification()` constructor will throw a `TypeError`. Use {{domxref("ServiceWorkerRegistration.showNotification()")}} instead.

## Syntax

```js-nolint
new Notification(title)
new Notification(title, options)
```

### Parameters

- `title`
  - : Defines a title for the notification, which is shown at the top of the notification window.
- `options` {{optional_inline}}
  - : An options object containing any custom settings that you want to apply to the notification. The possible options are:
    - `actions` {{optional_inline}}
      - : Must be unspecified or an empty array. `actions` is only supported for persistent notifications fired from a service worker using {{domxref("ServiceWorkerRegistration.showNotification()")}}.
    - `badge` {{optional_inline}}
      - : A string containing the URL of the image used to represent the notification when there isn't enough space to display the notification itself; for example, the Android Notification Bar. On Android devices, the badge should accommodate devices up to 4x resolution, about 96x96px, and the image will be automatically masked.
    - `body` {{optional_inline}}
      - : A string representing the body text of the notification, which is displayed below the title. The default is the empty string.
    - `data` {{optional_inline}}
      - : Arbitrary data that you want associated with the notification. This can be of any [structured-clonable](/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm#supported_types) data type. The default is `null`.
    - `dir` {{optional_inline}}
      - : The direction in which to display the notification. It defaults to `auto`, which just adopts the browser's language setting behavior, but you can override that behavior by setting values of `ltr` and `rtl` (although most browsers seem to ignore these settings.)
    - `icon` {{optional_inline}}
      - : A string containing the URL of an icon to be displayed in the notification.
    - `image` {{optional_inline}}
      - : A string containing the URL of an image to be displayed in the notification.
    - `lang` {{optional_inline}}
      - : The notification's language, as specified using a string representing a language tag according to {{RFC(5646, "Tags for Identifying Languages (also known as BCP 47)")}}. See the Sitepoint [ISO 2 letter language codes](https://www.sitepoint.com/iso-2-letter-language-codes/) page for a simple reference. The default is the empty string.
    - `renotify` {{optional_inline}}
      - : A boolean value specifying whether the user should be notified after a new notification replaces an old one. The default is `false`, which means they won't be notified. If `true`, then `tag` also must be set.
    - `requireInteraction` {{optional_inline}}
      - : Indicates that a notification should remain active until the user clicks or dismisses it, rather than closing automatically. The default value is `false`.
    - `silent` {{optional_inline}}
      - : A boolean value specifying whether the notification should be silent, i.e., no sounds or vibrations should be issued regardless of the device settings. If set to `true`, the notification is silent; if set to `null` (the default value), the device's default settings are respected.
    - `tag` {{optional_inline}}
      - : A string representing an identifying tag for the notification. The default is the empty string.
    - `timestamp` {{optional_inline}}
      - : A timestamp, given as {{glossary("Unix time")}} in milliseconds, representing the time associated with the notification. This could be in the past when a notification is used for a message that couldn't immediately be delivered because the device was offline, or in the future for a meeting that is about to start.
    - `vibrate` {{optional_inline}}
      - : A [vibration pattern](/en-US/docs/Web/API/Vibration_API#vibration_patterns) for the device's vibration hardware to emit with the notification. If specified, `silent` must not be `true`.

### Return value

An instance of the {{domxref("Notification")}} object.

### Exceptions

- {{jsxref("TypeError")}}
  - : Thrown if:
    - The constructor is called within the {{domxref("ServiceWorkerGlobalScope")}}.
    - The `actions` option is specified and is not empty.
    - The `silent` option is `true` and the `vibrate` option is specified.
    - The `renotify` option is `true` but the `tag` option is empty.
- `DataCloneError` {{domxref("DOMException")}}
  - : Thrown if serializing the `data` option failed for some reason.

## Examples

Here is a most basic example to only show a notification if permission is already granted. For more complete examples, see the {{domxref("Notification")}} page.

```js
if (Notification.permission === "granted") {
  const notification = new Notification("Hi there!");
}
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

### Chrome notes

Starting in Chrome 49, notifications don't work in incognito mode.

Chrome for Android will throw a {{jsxref("TypeError")}} when calling the
`Notification` constructor. It only supports creating
notifications from a service worker. See the
[Chromium issue tracker](https://crbug.com/481856) for more details.

## See also

- [Using the Notifications API](/en-US/docs/Web/API/Notifications_API/Using_the_Notifications_API)
