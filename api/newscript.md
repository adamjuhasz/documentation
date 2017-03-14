
[< - Return to API](/api/introduction.md)
# Script
The script is a key component to organizing your bot's behavior. It provides a  rich language to describe your bot flow.

### Globals
 **newScript**(**name**?: _string_): _Script_
 **getScript**(**name**?: _string_): _Script_

### Methods
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
function(**session**: [Session](/api/session.md), **response**: [Response](/api/response.md), **stop**: [StopFunction](#stopfunction)) => `Promise<void>`

Some methods have the **always** modifier available. When adding always to the method, the script will always run through these dialogs. This is useful for responding to general intents or buttons such as 'help', 'game score', 'restart', etc.

_When no script-name is passed the script is set as the default script._

#### Example
```javascript
newScript()
    .begin(function(session, response, stop){
        response.sendText('Welcome to this bot');
    })
    .dialog('menu', function(session, response, stop) {
        response.sendButtons()
            .text('Choose')
            .addButton('postback', '☘️ Feeling lucky', 'shuffle')
            .addButton('postback', '🕵️ Browse', 'browse')
            .addButton('url', 'Visit site', 'https://alana.cloud')
            .send();
    })
    .intent.always('general', 'help', function(session, response, stop) {
        response.sendText('This is a helpful message');
        response.goto('menu');
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
---
## .dialog {#dialog}
Main way to orchestrate an interaction with the user. Will waterfall through multiple dialogs until **stop()** is called or an **expect** call is hit.
```typescript
.dialog(fn: DialogFunction) => Expect
.dialog.always(fn: DialogFunction) => Expect
```
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
---
## .intent {#intent}
Matches on a detected user intent.
Either on just the general domain or on a specific domain and action.
```typescript
.intent(domain: string, fn: DialogFunction) => Expect
.intent(domain: string, action: string, fn: DialogFunction) => Expect
.intent.always(domain: string, fn: DialogFunction) => Expect
.intent.always(domain: string, action: string, fn: DialogFunction) => Expect
```
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
---
## .button {#button}
Responds to a post back button click from the user. If a payload is given it will only match buttons clicks with that payload.
```typescript
.button(payload: string, fn: DialogFunction) => Expect
.button(fn: DialogFunction) => Expect
.button.always(payload: string, fn: DialogFunction) => Expect
.button.always(fn: DialogFunction) => Expect
```

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
---
## .expect {#expect}
#### Properties
- [text](#expect-text)
- [button](#expect-button)
- [intent](#expect-intent)
- [match](#expect-match)
- [catch](#expect-catch)

When an expect method is called, the script will automatically pause at this point and wait for some type of user input. It will then go down the expect chain, multiple expect properties can be chained together.

#### Example
```javascript
newScript()
.dialog('menu', function(session, response, stop) {
    ...
})
.expect
    .button('shuffle', function(session, response, stop) {
        ...
    })
    .button(function(session, response, stop) {
        ...
    })
    .intent('general', 'help', function(session, response, stop) {
        ...
    })
    .match(/(foo|bar)/, function(session, response, stop) {
        ...
    })
    .catch(function(session, response, stop) {
        ...
    });
.dialog(function(session, response, stop) {
    ...
})
```

#### Text {#expect-text}
Matches any text message sent from the user.
```typescript
expect.text(fn: DialogFunction) => Expect
```
```javascript
// Example
newScript('echo')
.expect.text(function(session, response, stop) {
    response.sendText(session.message.text);
});
```

#### Button {#expect-button}
Matches either any button payload from the user or if button payload is provided it only matches a button click with that payload
```typescript
expect.button(fn: DialogFunction) => Expect
expect.button(payload: string, fn: DialogFunction) =>  Expect
```
```javascript
// Example
newScript('echo')
.expect.button('option3', function(session, response, stop) {
    ...
})
.expect.button(function(session, response, stop) {
    switch (session.message.payload) {
        case 'option1'
            break;
        case 'option2'
            break;    
    }
});
```

#### Intent {#expect-intent}
Matches on a detected user intent. 
Either on just the general domain or on a specific domain and action.
```typescript
expect.intent(domain: string, fn: DialogFunction) => Expect
expect.button(domain: string, action: string, fn: DialogFunction) => Expect
```
```javascript
// Example
newScript('weather')
.expect
  .intent('weather', 'snow', function(session, response, stop) {
    response.sendText('It will not snow tomorrow');
  })
  .intent('weather', 'rain', function(session, response, stop) {
    response.sendText('It will rain tomorrow, don\'t get wet!');
  })
  .intent('weather', function(session, response, stop) {
    response.sendText('This is your generic weather forecast');
  });
```

#### Match {#expect-match}
Matches against a regex or a string.
```typescript
expect.match(regex: Regex | string, fn: DialogFunction) => Expect
```
```javascript
// Example
newScript('weather')
.expect
  .match(/blizzard/, function(session, response, stop) {
    ...
  })
  .match('hurricane', function(session, response, stop) {
    ...
  });
```
#### Catch {#expect-catch}
Is a final catch-all for the expect chain. If all other expects fail, this will be called. It must be the end of the expect chain as it returns the script.
```typescript
expect.catch(fn: DialogFunction) => Script
```
```javascript
// Example
newScript('weather')
.expect
  .button(function(session, response, stop) {
    ...
  });
  .text(function(session, response, stop) {
    ...
  })
  .catch(function(session, response, stop) {
    ...
  })
```



