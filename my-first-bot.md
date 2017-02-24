#Creating your first _echo_ bot

### Create
Create a new bot using the floating action on the main page. Enter basic details of the bot and select 'none' as the template

_tl:dr_
1. Click '+'
2. Enter bot name
3. Select template **none**
![](/assets/Screen Shot 2017-02-23 at 6.39.31 PM.png)
---
### Code
Click on your new bot to open it and then select the **Code** tab. 

Notice the two directories, **scripts** and **tests**. Click on **main.js** in the scripts directory to open it for editing.

Change the `response.sendText(...);` code to read `response.sendText(sessions.message.text);`. This will send back the exact some text that the user is sending.

Click **save** in the toolbar to save the changes.

The file should be similar to:
```ts
newScript()
  .dialog((sessions, response) => {
    response.sendText(sessions.message.text);
  });
``` 
![](/assets/Screen Shot 2017-02-23 at 6.38.56 PM.png)
---
### Interactive test
Now let's test the new function interactively. Click **Open Chat** on the bottom right of the screen. Type something into the message window and watch the bot type it right back to you!

Interactive testing is great for testing new scripts as your write them.

---

### Automated test
As good software engineers lets add some automated testing. Click on **main.js** in the tests directory to open it. First let's try adding a test
```javascript
test('echo', function(){
  return newTest()
    .sendText('hello')
    .expectText('hello')
});
```

Click **Save** on the toolbar and wait a few moments as the tests run, we'll receive a confirmation that all tests passed.

---
### Deploy
What's a bot good for if no one can talk to it. Let's deploy our first bot. Click on the **platforms** tab. Click the floating action button to add a new platform. Select Facebook as the platform. Open a new tab and go to [developers.facebook.com](https://developers.facebook.com) and create a new bot. 

---
# Add a new user greeting
Next lets do some test driven development (TDD) and add a greeting for . We'll add a greeting so that a new user knows what to do when they interact with the bot.

### Add a test
Click on **main.js** in the tests directory to open it. Test for a greeting by adding the following test without changing the other test
```javascript
test('greeting', function(){
  return newTest()
    .expectText("Welome, I'll echo back what you say.")
});
```

Click **Save** on the toolbar and wait a few moments as the tests run, we'll receive a warning that the new **greeting** test failed.

### Add a greeting
Click on **main.js** in the scripts directory to open it. Lets add a greeting with the following code:
```javascript
addGreeting(function(user, response) => {
  response.sendText("Welome, I'll echo back what you say.");
});
```
Click **Save** on the toolbar and wait a few moments as the tests run, we'll receive a warning that the old **echo** test failed but at least the greeting test passed.

### Fix tests
Click on **main.js** in the tests directory to open it. Change the **echo** test to the following:
```ts
test('echo', function(){
  return newTest()
    .expectText("Welome, I'll echo back what you say.")
    .sendText('hello')
    .expectText('hello')
});
```

#Files
_scripts/main.js_
```ts
addGreeting(function(user, response) => {
  response.sendText("Welome, I'll echo back what you say.");
});

newScript()
  .dialog((sessions, response) => {
    response.sendText(sessions.message.text);
  });
```
_tests/main.js_
```ts
test('echo', function(){
  return newTest()
    .expectText("Welome, I'll echo back what you say.")
    .sendText('hello')
    .expectText('hello')
});

test('greeting', function(){
  return newTest()
    .expectText("Welome, I'll echo back what you say.")
  });
```
