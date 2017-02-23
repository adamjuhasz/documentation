# newScript('script-name')
## Properties
* [begin](#begin)
* dialog
* expect

All properties take a function function(**session**: [Session](./session.html), **response**: [Response](./response.html), **stop**: StopFunction) => void

## begin {#begin}
This is called when the script is first entered by the user.

## dialog {#dialog}

## expect {#expect}

## StopFunction {#stopfunction}
Defined as `() => void`

Will stop the script from executing till the some user input.