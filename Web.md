<!-- # Cdial Agent PRO Widget SDK -->
# Indigenius AI Widget

`CdialWidget` is a powerful SDK for integrating AI Agent call widget into your web application, offering functionalities to easily initiate and manage calls with your AI-powered customer service agents.

## Installation

To use the SDK in your project, you can either:

1. **Install via npm** (recommended for modern JavaScript applications)

   ```bash
   npm install indigenius-ai-widget
   ```

2. **Include the UMD build via CDN** (for legacy support)

   ```html
   <script src="https://cdn.jsdelivr.net/npm/indigenius-ai-widget@1.0.0/dist/sdk.umd.js"></script>
   ```

3. **Include the official url** (for legacy support)

   ```html
   <script
     type="module"
     src="https://widget.indigenius.ai/v1/index.js"
   ></script>
   ```

---

## Quick Start

### HTML Integration

You can integrate the widget into your HTML file with the following steps:

1. Add the `<indigenius-convai>` component to your HTML body to display the widget.
2. Include the SDK script (either from a CDN or local build).

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Webapp</title>
  </head>
  <body>
    <h1>Welcome to my random HTML site...</h1>

    <!-- Add the widget component -->
    <indigenius-convai agent-id="your-agent-id"></indigenius-convai>

    <!-- Add the SDK script (local or CDN) -->
    <script
      type="module"
      src="https://cdn.jsdelivr.net/npm/indigenius-ai-widget@1.0.0/dist/sdk.umd.js"
    ></script>

    <script defer>
      // Initialize event listeners
      window.addEventListener("load", () => {
        widget.on("dialing", () => {
          console.log("Dialing detected");
        });
        widget.on("connected", () => {
          console.log("Connected detected");
        });
        widget.on("ended", () => {
          console.log("Ended detected");
        });
        widget.on("error", (err) => {
          console.error("Error:", err);
        });
      });
    </script>
  </body>
</html>
```

## Framework Usage

### Initialize the Widget

The `init` method is used to initialize the widget with an agent ID. Ensure that you provide a valid `agentId` when initializing the widget.

```js
import { CdialWidget } from "indigenius-ai-widget";

const widget = new CdialWidget({
  agentId: "your-agent-id",
});

widget.init().then(() => {
  console.log("Widget initialized");
});
```

The `init` method loads the widget's theme, sets up elements, and attaches them to the page _if you want to use the dashboard customized flo0ating UI._

### Toggle Call

Use `toggleCall` to start or end a call. This method toggles the widget between the "call in progress" state and the "idle" state.

```js
widget.toggleCall(); // Starts or ends the call based on the current state
```

You can listen to events related to call state changes.

### Event Listeners

The SDK supports various events that you can listen to for user interaction or state changes.

#### Example: Listening to Call Start and End Events

```js
import { on } from "indigenius-ai-widget";

// Listen to call started
on("callStarted", () => {
  console.log("Call has started");
});

// Listen to call ended
on("callEnded", () => {
  console.log("Call has ended");
});
```

### Destroy the Widget

To clean up and remove the widget from the page, use the `destroy` method. This is useful if you need to remove the widget entirely after use.

```js
widget.destroy();
```

This method will remove all elements created by the widget and stop any ongoing call operations.

---

## Full Example

Here’s a complete example of how to use the widget:

---

## React Integration (Simplified Example)

Here's how to use the widget in a basic React application:

### 1. Install the SDK

```bash
npm install indigenius-ai-widget
```

### 2. Use in React Component

```jsx
// App.jsx or App.js
import { useEffect, useRef, useState } from "react";
import { CdialWidget } from "indigenius-ai-widget";

function App() {
  const widgetRef = useRef(null);
  const [callState, setCallState] = useState("idle");

  useEffect(() => {
    const widget = new CdialWidget({ agentId: "your-agent-id" });
    widgetRef.current = widget;

    widget.on("dialing", () => setCallState("dialing"));
    widget.on("connected", () => setCallState("connected"));
    widget.on("ended", () => setCallState("ended"));
    widget.on("error", console.error);

    widget.init();

    return () => {
      widget.destroy?.();
    };
  }, []);

  const toggleCall = () => {
    widgetRef.current?.toggleCall?.();
  };

  const getButtonText = () => {
    switch (callState) {
      case "dialing":
        return "Dialing...";
      case "connected":
        return "End Call";
      default:
        return "Start Call";
    }
  };

  return (
    <div style={{ padding: "2rem", fontFamily: "Arial" }}>
      <h2>Cdial Widget Call Button</h2>
      <button onClick={toggleCall}>{getButtonText()}</button>
    </div>
  );
}

export default App;
```

## Methods

### `init()`

Initializes the widget with the given `agentId` and sets up the UI.

**Returns**: `Promise<void>`

### `toggleCall()`

Starts or ends a call based on the current widget state.

**Returns**: `void`

### `destroy()`

Destroys the widget, removing all associated elements and cleaning up resources.

**Returns**: `void`

---

## Events

The widget emits several events that you can listen to for customizing your application's behavior. Below is a list of available events, their descriptions, and usage examples.

### 📡 `dialing`

Emitted when the widget starts attempting to connect to the AI agent.

**Example:**

```js
myCdialWidget.on("dialing", () => {
  console.log("Dialing detected");
});
```

---

### 🔗 `connected`

Emitted when the connection to the AI agent is successfully established.

**Example:**

```js
myCdialWidget.on("connected", () => {
  console.log("Connected detected");
});
```

---

### ❌ `ended`

Emitted when the call has ended or was terminated.

**Example:**

```js
myCdialWidget.on("ended", () => {
  console.log("Ended detected");
});
```

---

### 🎙️ `transcript`

Emitted whenever a new piece of speech is transcribed during the call.

**Payload:** `text` (string) – the transcribed voice input.

**Example:**

```js
myCdialWidget.on("transcript", (text) => {
  console.log("Transcript Received:", text);
});
```

---

### ⚠️ `error`

Emitted if there is an internal error during the operation of the widget.

**Payload:** `err` (Error or string) – details about the error that occurred.

**Example:**

```js
myCdialWidget.on("error", (err) => {
  console.error("Error:", err);
});
```

---

You can register these event listeners after the widget has loaded:

```html
<script defer>
  window.addEventListener("load", () => {
    myCdialWidget.on("dialing", () => {
      console.log("Dialing detected");
    });

    myCdialWidget.on("connected", () => {
      console.log("Connected detected");
    });

    myCdialWidget.on("ended", () => {
      console.log("Ended detected");
    });

    myCdialWidget.on("transcript", (text) => {
      console.log("Transcript Received:", text);
    });

    myCdialWidget.on("error", (err) => {
      console.error("Error:", err);
    });
  });
</script>
```

---

## License

This SDK is released under the MIT License. See LICENSE for more details.

---
