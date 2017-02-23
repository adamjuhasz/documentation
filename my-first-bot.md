#Creating your first _echo_ bot

## Create
Create a new bot using the floating action on the main page. Enter basic details of the bot and select 'none' as the template

_tl:dr_
1. Click '+'
2. Enter bot name
3. Select template **none**
![](/assets/Screen Shot 2017-02-23 at 6.39.31 PM.png)
---
## Code
Click on your new bot to open it and then select the **Code** tab. 

Notice the two directories, **scripts** and **tests**. Click on **main.js** in the scripts directory to open it for editing.

Change the `response.sendText(...);` code to read `response.sendText(sessions.message.text);`. This will send back the exact some text that the user is sending.

Click **save** in the toolbar to save the changes.

The file should be similar to:
```javascript
newScript()
  .dialog((sessions, response) => {
    response.sendText(sessions.message.text);
  });
``` 
![](/assets/Screen Shot 2017-02-23 at 6.38.56 PM.png)
---
## Interactive Test
Now let's test the new function interactively. Click **Open Chat** on the bottom right of the screen. Type something into the message window and watch the bot type it right back to you.

---

## Automated Test
As good software engineers lets add some automated testing. Ckick on **main.js** in the tests directory to open it. First let's try adding an 

---
## Deploy
What's a bot good for if no one can talk to it. Let's deploy our first bot.

---
# Add some jazz
Next lets do some test driven development and test some new functionality. We'll add a greeting so that a new user knows what to do.