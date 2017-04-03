[< - Return to Session](/api/session.md)

#Session -- Message
Incoming message from user

### Common Properties
- type (string)

# Incoming messages

## greeting {#greeting}
Sent once to the bot when a user first connects to the bot. Useful for sending an introduction about the bot.

### Properties
- type (string)
 - `'greeting'`
 
#### Example
```javascript
.dialog((session, response, stop) => {
   if (session.message.type === 'greeting') {
       response.sendText('Welcome!');
   }
});
```

## text {#text}
Text message from the user.
### Properties
- type (string)
 - `'text'`
- text (string)

#### Example
```javascript
.dialog((session, response, stop) => {
  if (session.message.type === 'text') {
     response.sendText(`You said: ${session.message.text}`);
  }
});
```

## button {#greeting}
Payload from a button clicked by the user.
### Properties
- type (string)
 - `'postback'`
- payload (string)

#### Example
```javascript
.dialog((session, response, stop) => {
  if (session.message.type === 'postback') {
    switch (session.message.payload) {
     case 'button1':
       break;
     
     case 'button2':
       break;
  }
});
```

## image {#greeting}
Image sent by the user
### Properties
- type (string)
 - `'image'`
- url (string)

#### Example
```javascript
.dialog((session, response, stop) => {
  if (session.message.type === 'image') {
    response.sendText('You sent this to me:').sendImage(session.message.url);
  }
});
```

