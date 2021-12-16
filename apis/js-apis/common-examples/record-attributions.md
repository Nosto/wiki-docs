# Recording attribution for any event type
## Summary
Nosto already offers session API, for recording attribution. But it's limited to `vp` events so far. In an attempt to expand this functonality to any event types, a new session API method `recordAttribution` has been introduced. Using JS API, this method can be utilised to update current visit with details of events for attribution.

## Usage
The session API mthod `recordAttribution` accepts one event at a time, but use case chain multiple calls to `recordAttribution` and add as many events as they want. 

### Method documentation
class: session.js
name: recordAttribution
parameters: 
|  name  | field type | is required | description                                                      |
|:------:|:----------:|:-------------:|:------------------------------------------------------------------:|
|  type  |   string   |     yes     | type of event to which a placement (ref) should be attributed    |
| target |   string   |     yes     | id of the element that's been loaded as a result of the event    |
|   ref  |   string   |      no     | id of the element that hosted the link which triggered the event |
| refSrc |   string   |      no     | id of parent element of the link that triggered the event        |

### Examples

(1) Attributing a placement click to a `vp` event

```javascript
nostojs(function(api) {
  api
  .defaultSession()
  .recordAttribution("vp", "12345678", "frontpage-nosto-1")
  .done()
});
```
(2) Attributing a placement click to a `cc` event

```javascript
nostojs(function(api) {
  api
  .defaultSession()
  .recordAttribution("cc", "12345678", "frontpage-nosto-1")
  .done()
});
```
In similar way, we will be able to send in all the Nosto supported event types that are relevant