---
title: "RTCPeerConnection: getReceivers() method"
short-title: getReceivers()
slug: Web/API/RTCPeerConnection/getReceivers
page-type: web-api-instance-method
browser-compat: api.RTCPeerConnection.getReceivers
---

{{APIRef("WebRTC")}}

The **`getReceivers()`** method of the {{domxref("RTCPeerConnection")}} interface returns an array of {{domxref("RTCRtpReceiver")}} objects, each of which represents one RTP receiver.
Each RTP receiver manages the reception and decoding of data for a {{domxref("MediaStreamTrack")}} on an {{domxref("RTCPeerConnection")}}.

## Syntax

```js-nolint
getReceivers()
```

### Parameters

None.

### Return value

An array of {{domxref("RTCRtpReceiver")}} objects, one for each track on the connection.
The array is empty if there are no RTP receivers on the connection.

The order of the returned `RTCRtpReceiver` instances is not defined by the specification, and may change from one call to `getReceivers()` to the next.

The array does not include receivers associated with transceivers that have been [stopped](/en-US/docs/Web/API/RTCRtpTransceiver/currentDirection) (following offer/answer).

## Example

tbd

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [WebRTC](/en-US/docs/Web/API/WebRTC_API)
- {{domxref("RTCRtpSender")}}
