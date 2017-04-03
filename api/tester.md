[< - Return to API](/api/introduction.md)

# Tester

## methods
- checkForTrailingDialogs
- expectText
- sendText
- run

```typescript
newTest() => Tester
class Tester {
  checkForTrailingDialogs(state: bool = true): this;
  debugBreak(): this;
  expectText(phrase: string): this;
  expectButtons(text: string, buttons: Array<Button>): this;
  sendText(phrase: string): this;
  sendButtonClick(payload: string): this;
  run(): Promise<void>
}
```

