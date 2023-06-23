# Custom Tiles
By popular community request, we’ve started putting together a few Custom Tiles for Homey. These are still very much beta and have not been tested thoroughly or over an extended period of time.

## Source vs Compiled
You are currently in the `/homey/src` folder which includes the raw source code for each of the Custom Tiles. 

When we publish custom tiles (to the `/homey/` parent folder), we typically transpile the JavaScript to support older browsers. 

### Transpiler
We've recently started working on a built-in transpilation feature in the Custom Tile editor which can automatically translate modern JavaScript (eg. ES2022) to support older browsers (eg. ES5). This enables us to support browsers that are over a decade old while still having the convenience of writing modern JavaScript. 

This means that you can write using asnyc/await, arrow functions, `let` declarations and more and the transpiler will automatically take care of converting things.

Source:
```
let getGreeting = (name="there") => {
  console.log(`Hello ${name}, nice to meet you!`)
}
```

Compiled:
```
var getGreeting = function() {
    var name = arguments.length > 0 && arguments[0] !== void 0 ? arguments[0] : "there";
    console.log("Hello ".concat(name, ", nice to meet you!"));
};
```

This feature _currently_ requires you to manually view the transpiled source which you can do by tapping the cog icon in the top-right corner of the Code Editor and then select 'Preview Transpile' from the dropdown. 


### Legacy Mess
Some of these tiles were based on older tiles that we made for other platforms and at that time, we have to _manually_ write JavaScript using only features that were supported by older browsers. As such, the source code for some of these tiles is a bit of a mess of old style `function()` and `var` declarations while also mixed in with `() => {}` and `let`. 

Anyway, it doesn't cause any functional issues, but it's worth mentioned in case you are trying to understand the coding _style_ used. (In other words, it's not an intentional style, just hasn't been cleaned up)

# API Approaches
Also note that this is a _very_ early proof of concept. As of 2023-06-23, each tile uses a slightly different approach for communicating with the Homey APIs and you may find this interesting for your own learning.

* API Key + HTTP Calls (Homey 2023 only)
* API Key + Homey Library
* Homey.ink Token + Homey Library

### Homey Flows: API Key + HTTP Calls
The Homey Flows example _only_ supports Homey 2023 as it uses the new [API Key feature](https://support.homey.app/hc/en-us/articles/8178797067292-Getting-started-with-API-Keys) that Homey included with the 2023 models. 

To prove the point that you can use the Homey API _directly_ without needing to use the Homey API library, this demo tile makes calls directly to the Homey API endpoints. 

> ⚠️ This example will _not_ work with Homey 2019. Check the Homey Google Gauge tile for a **very unofficial** approach to supporting 2019 devices. 

### Homey Google Gauge: API Key / Homey.ink Workaround
The Google Gauge example uses the official Homey API JavaScript library to communicate with your Homey. 

It uses a number of **unofficial** and **unsupported** tricks to simplify the configuration process for end-users as well as to support the older Homey models. 

Some of the tricks used:
* User can paste in a URL from their Homey and it will automatically parse out the Homey Cloud ID and Device ID and use that.
  * Furthermore, it uses the Cloud ID in an unofficial way to interrogate the Homey and determine the IP adddress of the Homey so it can use that to setup the API
  * Homey has a weird limitation where the library only supports local communication with the Homey and it will fail to initialize when you use the cloud address which is why we use a direct call to the cloud endpoint to get the IP address and then use that to initialize the library
* For supporting older Homey devices, the library leverages the Homey Cloud API. Since this is running within a Custom Tile, we can't use the 'Local Development' Client ID and Secret mentioned in the [Homey Web API Docs](https://api.developer.homey.app/), so we use a trick similar to what Homey Dash is using -- we let the user authenticate to [Homey.ink](https://homey.ink) which is a project that Emile (Homey founder) put together. 
   * While I haven't looked closely at the Homey Dash code, I believe they've mostly cloned the base Homey.ink source and are leveraging the same (older) APIs for interacting.
   * Our example uses the new Homey API Library in roughly the same way, but demonstrates how you can initialize a Token object and pass it to the AthomCloudAPI to instantiate a valid client.
   * Furthermore, this approach also mirrors the Homey.ink/Homey Dash approach of including the Homey.ink Client ID + Client Secret which enables the API client to automatically grab fresh authorization tokens.
      * Otherwise, normally authorization tokens are short lived and you have to use a refresh token to grab a new auth token.
      * By including the client ID + secret from Homey.ink, the library is able to automatically use the refresh token. 
    > 🛑 As you might surmise from the above, this is a **very unofficial** approach and something that Homey themselves could decide to close at some point in the future. 