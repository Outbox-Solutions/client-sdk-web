# Outbox Web SDK

This package lets you start Outbox calls directly in your webapp.

## Installation

You can install the package via npm:

```bash
npm install @outbox-ai/web
```

## Usage

First, import the Outbox class from the package:

```javascript
import Outbox from "@outbox-ai/web";
```

Then, create a new instance of the Outbox class, passing your Public Key as a parameter to the constructor:

```javascript
const outbox = new Outbox("your-public-key");
```

You can start a new call by calling the `start` method and passing an `assistant` object or `assistantId`:

```javascript
outbox.start({
  model: {
    provider: "openai",
    model: "gpt-3.5-turbo",
    messages: [
      {
        role: "system",
        content: "You are an assistant.",
      },
     ],
   },
   voice: {
    provider: "11labs",
    voiceId: "burt",
  },
  ...
});
```

```javascript
outbox.start("your-assistant-id");
```

The `start` method will initiate a new call.

You can override existing assistant parameters or set variables with the `assistant_overrides` parameter.
Assume the first message is `Hey, {{name}} how are you?` and you want to set the value of `name` to `John`:

```javascript
const assistantOverrides = {
  recordingEnabled: false,
  variableValues: {
    name: "John",
  },
};

outbox.start("your-assistant-id", assistantOverrides);
```

You can send text messages to the assistant aside from the audio input using the `send` method and passing appropriate `role` and `content`.

```javascript
outbox.send({
  type: "add-message",
  message: {
    role: "system",
    content: "The user has pressed the button, say peanuts",
  },
});
```

Possible values for the role are `system`, `user`, `assistant`, `tool` or `function`.

You can stop the session by calling the `stop` method:

```javascript
outbox.stop();
```

This will stop the recording and close the connection.

The `setMuted(muted: boolean)` can be used to mute and un-mute the user's microphone.

```javascript
outbox.isMuted(); // false
outbox.setMuted(true);
outbox.isMuted(); // true
```

The `say(message: string, endCallAfterSpoken?: boolean)` can be used to invoke speech and gracefully terminate the call if needed

```javascript
outbox.say("Our time's up, goodbye!", true);
```

## Events

You can listen to the following events:

```javascript
outbox.on("speech-start", () => {
  console.log("Speech has started");
});

outbox.on("speech-end", () => {
  console.log("Speech has ended");
});

outbox.on("call-start", () => {
  console.log("Call has started");
});

outbox.on("call-end", () => {
  console.log("Call has stopped");
});

outbox.on("volume-level", (volume) => {
  console.log(`Assistant volume level: ${volume}`);
});

// Function calls and transcripts will be sent via messages
outbox.on("message", (message) => {
  console.log(message);
});

outbox.on("error", (e) => {
  console.error(e);
});
```

These events allow you to react to changes in the state of the call or speech.

## License

```
MIT License

This package (@outbox-ai/web) is a fork of @vapi-ai/web, with modifications to support Outbox AI.
Original copyright (c) 2024 Vapi Labs Inc. Licensed under the MIT License.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
