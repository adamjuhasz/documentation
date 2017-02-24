# Scripts
Scripts are the main way of interacting with your user, they make developing waterfall style bots much easier as you don't have a bunch of `if` and `switch` statements everywhere and need to manually keep track of state.

```javascript
// Profile example without scripts
newScript().dialog(function(session, response, stop) {
    if (session.user.hasProfile) {
        response.sendText('This is the main menu');
    }
    else {
        //we need to manually keep track of what the next question is
        switch (session.user.step) {
            default:
                response.sendText('Can I have your name?');
                sessions.user.step = 'name';
                break;
                
            case 'name':
                if (!session.message.text) {
                    response.sendText('Can I have your name?')
                } else {
                    session.user.name = session.message.text;
                    session.user.step = 'age';
                    response.sendText(`Thanks ${session.user.name}, how old are you?`);
                }
                break;
                
            case 'age': {
                if (!session.message.text) {
                    response.sendText('How old are you?')
                } else {
                    const age = parseInt(session.message.text, 10);
                    if (!age) {
                        response.sendText(`I'm sorry but "${session.message.text}" isn't a number`);
                        response.sendText('Can you try again?');
                        stop();
                    }
                    session.user.age = age;
                    session.user.step = 'email';
                    response.sendText(`OK, I'll remember that you're ${session.user.age} years old`);
                    response.sendText('What is your email?');
                }
            }
            
            case 'email':
                if (!session.message.text) {
                    response.sendText('What is your email?');
                } else {
                    session.user.email = session.message.text;
                    session.user.hasProfile = true;
                    response.sendText('Saved!');
                }
                break;
        }
    }
})
```

```javascript
// Profile example with scripts
newScript().begin(function(session, response) {
    if (!session.user.hasProfile) {
        response.startScript('profile');
    }
    response.sendText('This is the main menu');
});

newScript('profile')
.begin(function(session, response) {
    response.sendText('Can I have your name?');
})
.expect.text(function(session, response) {
    session.user.name = session.message.text;
})
.catch(function(session, response) {
    //if the user sends a photo, ask again
    response.sendText('Can I have your name?');
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
.catch(function(session, response) {
    //if the user sends a photo, ask again (with different prompt)
    response.sendText('How old are you?');
})
.dialog(function(session, response) {
    response.sendText(`OK, I'll remember that you're ${session.user.age} years old`);
    response.sendText('What is your email?');
})
.expect.text(function(session, response) {
    session.user.email = session.message.text;
    session.user.hasProfile = true;
    response.sendText('Saved!');
})
.catch(function(session, response) {
    response.sendText('What is your email?');
})
```