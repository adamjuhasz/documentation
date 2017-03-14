
[< - Return to API](/api/introduction.md)

# **newScript**(**name**?: _string_): _Script_
## Properties
- [begin](#begin)
- [dialog](#dialog)
 - always
- [intent](#intent)
 - always
- [button](#button)
 - always
- [expect](#expect)
 - [text](#expect-text)
 - [button](#expect-button)
 - [intent](#expect-intent)
 - [match](#expect-match)
 - [catch](#expect-catch)


All properties take a function in the format of

function(**session**: [Session](/api/session.md), **response**: [Response](/api/response.md), **stop**: [StopFunction](#stopfunction)) => void

_When no script-name is passed the script is set as the default script._

#### Example
```javascript
newScript()
    .begin(function(session, response, stop){
        response.sendText('Welcome to this bot');
        response.sendButtons()
            .text('Choose')
            .addButton('postback', '☘️ Feeling lucky', 'shuffle')
            .addButton('postback', '🕵️ Browse', 'browse')
            .addButton('url', 'Visit site', 'https://alana.cloud')
            .send();
    })
    .intent.always('general', 'help', function(session, response, stop) {
        response.sendText('This is a helpful message');
    })
    .expect
        .button('shuffle', function(session, response, stop) {
            // send shuffled list
            response.startScript('random');
        })
        .button('browse', function(session, response, stop) {
            response.startScript(...);
        })
        .catch(function(session, response, stop) {
            response.sendText('This bot is confused');
        });
        
newScript('random')
    .begin(...)
    .dialog(...)
```

---

## .begin {#begin}
This is called when the script is first entered by the user.

```javascript
// Example
newScript('weather').begin(function(session, response, stop) {
    response.sendText('Where would you like the weather forecast for?');
});
```

## .intent {#intent}
Called when alana you want to respond to a user intent
Optionally called with `.intent.always(domain, action, function(...){...})`

```javascript
// Example
newScript('weather')
    .intent.always('general', 'help', function(session, response) {
        response.sendText('I can help you find out if it is cold outside');
    })
    // ...
    .intent('weather', 'snow', function(session, response) {
        // do only a snow forecast
    })
```

## .dialog {#dialog}
Main way to orchestrate an interaction with the user. Will waterfall through multiple dialogs until **stop()** is called or an **expect.(type)** call is hit.
```javascript
// Example
newScript('weather')
  .dialog(function(session, response, stop) {
    if (!session.input.location) {
      response.sendText("I don't know where that is, can you try again?");
      stop();
    }
    response.sendText(`Checking forecast for ${session.input.location}`);
    return request({
      uri: 'forecast.com', method: 'POST', json: true, 
      body: {},
    });
  });
```

## .expect {#expect}
**Properties**
* text
* image
* button

#### Text {#expect-text}
```javascript
// Example
newScript('weather').expect.text(function(session, response, stop) {
    console.log(session.input.url);
});
```

#### Button {#expect-button}
```javascript
// Example
newScript('weather').expect.button(function(session, response, stop) {
    console.log(session.input.message);
});
```

## StopFunction {#stopfunction}
Defined as `() => void`

Will stop the script from executing till the next user input and rerun the current step.


