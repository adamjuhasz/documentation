#Methods

## Globals
```typescript
newScript(scriptname?: string): Script;
```

```typescript
addGreeting((user: User, response: Response) => void): void;
```

```typescript
request({
uri: string;
method: string;
}): Promise<Request>;
```
```typescript
newTest(): Tester
```


### newScript('name')
Main function used to build up scripts for your bot. All of the properties ext

* [begin](./newscript.html)
* dialog
* expect


### request(options)