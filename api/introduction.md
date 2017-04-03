# Globals
Alana exposes the following global variables
- [newScript](#newScript)
- [getScript](#getScript)
- [addGreeting](#addGreeting)
- [request](#request)
- [newTest](#newTest)

## newScript {#newScript}
Creates a new script for user interaction. Main function used to build and organize your bot.
- If a string is passed in then the script is accessed using that name. 
- If no string is passed then it becomes the default script
```typescript
newScript(scriptname?: string): Script;
```

#### [Script documentation](/api/newscript)
* [begin](/api/newscript.md#begin)
* [dialog](/api/newscript.md#dialog)
* [expect](/api/newscript.md#expect)
* [intent](/api/newscript.md#intent)
* [button](/api/newscript.md#button)

## getScript {#getScript}
Get a previously created script by its name.
```typescript
getScript(scriptname?: string): Script;
```
#### [Script documentation](/api/newscript)

## addGreeting {#addGreeting}
Adds a greeting for new users. Only called once per user lifetime (on first connection).
```typescript
addGreeting((user: User, response: Response)): void;
```
#### [Greeting documentation](/api/newscript)

## request {#request}
Send an http request to another server
```typescript
request(opts: RequestOptions): Promise<Request>;
```
#### [Request documentation](/api/newscript)

## newTest {#newTest}
Create a new test, *Can only be called in the tests directory.*
```typescript
newTest(userId: string): Tester;
```
#### [Testing documentation](/api/newscript)