[< - Return to Session](/api/session.md)

#Session -- User
Holds information about the user

### Properties
- [state](#state) (object)
- [id](#id) (string)
- [platform](#platform) (string)
- [script](#script) (string)
- [conversation](#conversation) (array)

## state (object) {#state}
Correct place to store information about the user, will be stored and recalled between calls to the bot.
#### Example
```javascript
newScript()
    .dialog((session, response, stop) => {
        session.user.state = {
            score: 0,
            name: 'user',
        };
    })
    // ...
    .expect.text((session, response, stop) => {
        session.user.state.name = session.message.text;
    });
```

## id (string) {#id}
Unique identifier for the user.
#### Example
```javascript
    .dialog((session, response, stop) => {
        request({
            uri: 'https://api.bot/justdoit',
            method: 'POST',
            json: true,
            body: {
                user: session.user.id,
            },
        });  
    });
```

## platform (string) {#platform}
Text identifier of the platform user is using to interact with the bot.
#### Example
```javascript
.dialog((session, response, stop) => {
    if (session.user.platform === 'Facebook') {
        response.sendText('Welcome from FB');
    }
});
```

## script (string) {#script}
Current script the user is in.
** Do not modify this directly, use .startScript() and .endScript() **
#### Example
```javascript
.dialog((session, response, stop) => {
    switch (session.user.script) {
        case 'a':
            response.startScript('c');
            break;
        case 'b':
            response.endScript();
    }
});
```


## conversation (array) {#conversation}
Array of messages that has been the conversation so far, conversation is sorted reverse order.
#### Example
```javascript
.dialog((session, response, stop) => {
    const currentMessage = session.user.conversation[0];
    // same as session.message;
})
```

