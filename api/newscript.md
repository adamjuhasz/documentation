[<- Return to API](/api/introduction.md)

# **newScript**(name?: _string_): _Script_
## Properties
* [begin](#begin)
* dialog
* expect

All properties take a function in the format of

function(**session**: [Session](/api/session.md), **response**: [Response](/api/response.md), **stop**: [StopFunction](#stopfunction)) => void

_When no script-name is passed the script is set as the default script._

## begin {#begin}
This is called when the script is first entered by the user.

```javascript
// Example
newScript('weather').begin(function(session, response, stop) {
    response.sendText('Where would you like the weather forecast for?');
});
```


## dialog {#dialog}
Main way to orchestrate an interaction with the user.
```javascript
// Example
newScript('weather').dialog(function(session, response, stop) {
    if (!session.input.location) {
        response.sendText("I don't know where that is, can you try again?");
        stop();
    }
    response.sendText(`Checking forecast for ${session.input.location}`);
    return request({
        uri: 'forecast.com', method: 'POST',
        json: true, body: {},
    });
});
```

## expect {#expect}
**Properties**
* text
* image
* button

#### Text
```javascript
// Example
newScript('weather').expect.text(function(session, response, stop) {
    console.log(session.input.url);
});
```

#### Image
```javascript
// Example
newScript('weather').expect.image(function(session, response, stop) {
    console.log(session.input.message);
});
```

#### Button
```javascript
// Example
newScript('weather').expect.button(function(session, response, stop) {
    console.log(session.input.message);
});
```

## StopFunction {#stopfunction}
Defined as `() => void`

Will stop the script from executing till the next user input.