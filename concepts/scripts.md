# Scripts
Scripts are the main way of interacting with your user, they make developing waterfall style bots much easier.

```javascript
// Profile example without scripts
newScript().dialog(function(session, response, stop) {
    //Send instructions to the user.
    if (session.location) {
        request(forecastOptions)
            .then(function(results) {
                response.sendText
            });
    } else {
        // fallback
        respinse.sendText('Where do you want a forecast for?');
    }
})
```

```javascript
// Profile example with scripts
newScript('profile')
.begin(function(session, response) {
    response.sendText('Can I have your name?');
})
.expect.text(function(session, response) {
    session.user.name = session.message.text;
})
.dialog(function(session, response) {
    response.sendText(`Thanks ${session.user.name}, how old are you?`);
})
.expect.text(function(session, response, stop) {
    // really simple to do validation
    const age = parseInt(session.message.text, 10);
    if (!age) {
        response.sendText(`I'm sorry but "${session.message.text}" isn't a number`);
        response.sendText('Can you try again?');
        stop();
    }
    session.user.age = age;
})
.dialog(function(session, response) {
    response.sendText(`OK, I'll remember that you're ${session.user.age} years old`);
});
```