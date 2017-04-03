[< - Return to API](/api/introduction.md)

# Response

### Methods
- startScript
- endScript
- goto
- sendText
- sendImage
- createButtons

## startScript {#startScript}
Begin a new script using the scripts name
```typescript
.startScript(script_name: string) => void
```
```javascript
// Example
newScript('weather')
  // dialogs go here

newScript()
.dialog(function(session, response, stop) {
  if (session.intent.domain = 'weather') {
    response.startScript('weather');
  }
});
```
---
## endScript {#endScript}
Go back to the default script
```typescript
.endScript() => void
```
```javascript
// Example
newScript('weather')
  // dialogs go here
  .dialog(function(session, response, stop) {
    response.endScript();
  });

newScript()
.dialog(function(session, response, stop) {
  if (session.intent.domain = 'weather') {
    response.startScript('weather');
  }
});
```
---
## goto {#goto}
Go to a named dialog
```typescript
.goto(dialog_name: string) => void
```
```javascript
// Example
.dialog('starting', function(session, response, stop) {
  response.sendText(`Checking forecast for ${session.input.location}`);
  return request({
    uri: 'forecast.com', method: 'POST', json: true,
    body: {},
  });
})
.dialog(function(session, response, stop) {
  if (...) {
    response.goto('starting');
  }
});
```
--
## sendText {#sendtext}
Send a text message to the user.
```typescript
.sendText(text: string) => Promise<Response>
```
```javascript
// Example
.dialog(function(session, response, stop) {
  response.sendText('hi');
})
```

## sendImage {#sendimage}
Send an image to the user.
```typescript
.sendImage(url: string) => Promise<Response>
```
```javascript
// Example
.dialog(function(session, response, stop) {
  response.sendImage('https://image.come/hi.jpg');
})
```

## createButtons {#createbuttons}
Send buttons to the user
```typescript
.createButtons() => Button
class Button {
  text(): string
  text(text: string): this;
  addButton('postback', text: string, payload: string): this
  addButton('url', text: string, payload: string): this
  send(): Response
}
```
```javascript
// Example
.dialog(function(session, response, stop) {
  response.createButtons()
    .text('choose one:')
    .addButton('postback', 'choice 1', 'c1')
    .addButton('postback', 'choice 2', 'c2')
    .addButton('url', 'Visit website', 'https://www.alana.tech')
    .send();
})
.expect.button('c1', function(session, response, stop) {
  // clicked choice 1
})
.expect.button('c2', function(session, response, stop) {
  // clicked choice 2
})
```