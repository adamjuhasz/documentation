# newScript(name?: string)
## Properties
* [begin](#begin)
* dialog
* expect

All properties take a function in the format of
function(**session**: [Session](/api/session.md), **response**: [Response](/api/response.md), **stop**: [StopFunction](#stopfunction)) => void

_When no script-name is passed the script is set as the default script._

## begin {#begin}
Example:
```typescript
newScript('weather').begin((session, response, stop) => {
    response.sendText('Where would you like the weather forecast for?');
});
```
This is called when the script is first entered by the user.

## dialog {#dialog}
Example:
```javascript
newScript('weather').dialog((session, response, stop) => {
    if (!session.input.location) {
        response.sendText("I don't know where that is, can you try again?");
        stop();
    }
    response.sendText(`Checking forecast for ${session.input.location}`);
    return request({
        uri: 'forecast.com',
        method: 'POST',
        json: true,
        body: {},
    });
});
```

## expect {#expect}
### Properties

## StopFunction {#stopfunction}
Defined as `() => void`

Will stop the script from executing till the some user input.