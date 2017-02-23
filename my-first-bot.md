#Creating your first _echo_ bot

## Create
Create a new bot using the floating action on the main page. Enter basic details of the bot and select 'none' as the template

_tl:dr_
1. Click '+'
2. Enter bot name
3. Select template **none**
---
## Code
Click on your new bot to open it and then select the 'Code' tab. 

Notice the two directories, "scripts" and "tests". Click on "main.js" under scripts to open it.

Change the `response.sendText(...);` code to read `response.sendText(sessions.message.text);`. This will send back the exact some text that the user is sending.


The file should be similar to
```javascript
newScript()
  .dialog((sessions, response) => {
    response.sendText(sessions.message.text);
  });
``` 

---
## Test
Now let's test

---
## Deploy