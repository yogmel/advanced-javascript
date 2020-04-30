## Events

[**Go back to Summary**](README.md#summary);

- [Events Phases](#events-phases);
- [stopPropagation() and preventDefault()](#stoppropagation-and-preventdefault);

---------

## Events Phases

- [EventListener() - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

In the DOM, elements are created starting to the `window` object, in a tree structure. When an event is fired in an element, two phases occur: one triggering from the bottom to the top, and the second from top to bottom.

- [Event.eventPhase - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Event/eventPhase#Event_phase_constants)

For example, if the structure is `window > document > body > button-one > button-two`, then the first phase, the "Event Capturing Phase", triggers the window, then document, body and so on.

The second phase, the "Event Bubbling Phase", triggers from the button-one, to button-two, body, document and window.

That means that and events trigger **twice**. However, when using `.addEventListener(event, callback, useCapture)`, by default, only the event in "Event Bubbling Phase" is trigger. If `useCapture` is set to true, then the event in "Event Capturing Phase" is triggered.

### stopPropagation() and preventDefault()
`stopPropagation()` stops the propagation of events in the element of the tree, through the phases seen before.

`preventDefault()` prevents the default action of an element of being executed. That means, for example, that a checkbox will not be checked when clicked, if this method is used. This does not stop the propagation of events.
