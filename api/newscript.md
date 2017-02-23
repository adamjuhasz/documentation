# newScript(name?: string)
## Properties
* [begin](#begin)
* dialog
* expect

All properties take a function function(**session**: [Session](./session.html), **response**: Response, **stop**: [StopFunction](#stopfunction)) => void

_When no name is passed the script is set as the default script._

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
```typescript
newScript('weather').dialog((session, response, stop) => {
response.sendText('Where would you like the weather forecast for?');
});
```

## expect {#expect}
### Properties

## StopFunction {#stopfunction}
Defined as `() => void`

Will stop the script from executing till the some user input.