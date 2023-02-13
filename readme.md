# LiveLog Service

A deno deploy logging service

LiveLog is a utility service used to remotely log messages.   

This service recieves POST requests from one or more applications.   

These requests are then live streamed to any registered listeners (EventSource
clients).   

LiveLog is effectively a pub/sub remote console.log service.
One or more applications could stream messages to one or more viewers.

## Interface
The message interface is as follows:

```ts
type Payload {
   CLS: boolean   // clear the screen first (default = false)
   TS: string     // a timestamp string 
   from: string   // typically an app_name + method_name 
   msg: string    // a string or JSON.stringified object 
}
 
type LogEvent {
   topic: string
   data: Payload
}

// example usage
export function log(thisMsg: string, clearFirst = true) {
   
   const jsonBody = JSON.stringify({ 
      topic: 'log', 
      data: { 
         CLS: clearFirst, // clear the screen first
         TS: new Date().toLocaleTimeString('en-US'), // time stamp 
         from: "updateText", // an event name
         msg: thisMsg        // the payload string
      } 
   })

   fetch("https://live-log.deno.dev/", {
      method: "POST",
      body: jsonBody
   })
}
```
## See LiveLog for application usage
https://github.com/nhrones/LiveLog