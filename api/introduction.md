#Methods
## Globals
#### newScript
Creates a script for user interaction.
```typescript
newScript(scriptname?: string): Script;
```
#### addGreeting
Adds a greeting for new users. Only called once per user lifetime (on first connection).
```javascript
addGreeting((user: User, response: Response)): void;
```
#### request
Send a http request.
```typescript
request(opts: RequestOptions): Promise<Request>;
```
#### newTest
Create a new test.
```typescript
newTest(): Tester
```


### newScript('name')
Main function used to build up scripts for your bot. All of the properties ext

* [**begin**](/api/newscript.md#begin)
* [**dialog**](/api/newscript.md#dialog)
* [**expect**](/api/newscript.md#expect)


### request(options)
Send an http request to another server