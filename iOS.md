# **indigenius iOS SDK**
This package lets you start indigenius calls directly in your iOS app.

## **Requirements**
- iOS 13.0 or later
## **Installation**
## **Swift Package Manager**
In Xcode, go to File -> Add Packages... and enter the following URL in 'Search or Enter Package URL' textbox in the top right corner of that window: (https://github.com/indigeniusAI/ios)

Pick the desired dependency rule (under “Dependency Rule”), as well as build target (under “Add to Project”) and click “Add Package”.

## **In Package.swift**
To depend on the indigenius package, you can declare your dependency in your `Package.swift`:

```bash
.package(url: "https://github.com/indigeniusAI/ios", branch: "main"),
````
and add `"indigenius"` to your application/library target, `dependencies`, e.g. like this:

```bash.target(name: "YourApp", dependencies: [
    .product(name: "indigenius", package: "ios")
],
````
## **App Setup**
You will need to update your project's Info.plist to add three new entries with the following keys:

- NSMicrophoneUsageDescription
- UIBackgroundModes
For the first two key's values, provide user-facing strings explaining why your app is asking for microphone access.

UIBackgroundModes is handled slightly differently and will resolve to an array. For its first item, specify the value voip. This ensures that audio will continue uninterrupted when your app is sent to the background.

To add the new entries through Xcode, open the Info.plist and add the following four entries (Camera is optional):

|Key|	Type|	Value|
|--------|-----|-----|
|Privacy - Camera Usage Description|	String |"Your app name needs camera access to work|	
|Privacy - Microphone Usage Description|	String	|"Your app name needs microphone access to work"|
|Required background modes	|Array	|1 item|
|---> Item 0	|String	|"App provides Voice over IP services"|

If you view the raw file contents of Info.plist, it should look like this:

````bash
<dict>
    ...
    <key>NSMicrophoneUsageDescription</key>
    <string>Your app name needs microphone access to work</string>
    <key>NSCameraUsageDescription</key>
    <string>Your app name needs camera access to work</string>
    <key>UIBackgroundModes</key>
    <array>
        <string>voip</string>
        <string>audio</string>
    </array>
    ...
</dict>
````

## **Usage**
### 1. Starting a Call
- #### Methods:
  - `start(assistantId: String, assistantOverrides: [String: Any] = [:])`
  - `start(assistant: [String: Any], assistantOverrides: [String: Any] = [:])`
- #### Description:
  - Use these methods to initiate a new call. You can either start a call by passing an `assistantId` or by providing an `assistant` dictionary with specific parameters.
  - These methods throw an error if there's already an ongoing call to ensure that only one call is active at any time.
  - The `assistantOverrides` parameter is optional and allows you to override an assistant's default settings or set variables. For example, if the first message is "Hello {{name}}", you can set `assistantOverride`s to `["variableValues": ["name": "Alice"]]` to replace `{{name}}` with `Alice`.

## 2. Muting a Call
- #### Methods:
  - `setMuted()`
  - `isMuted()`
- #### Description:
  - Use these methods to mute or unmute an ongoing call.
  - Parameters: ``muted is a boolean value indicating whether the audio should be muted.
  - `setMuted(false)` will mute the audio and `etMuted(true)` will unmute audio.
## 3. Handling Events
- #### Overview: The SDK provides various events that you can listen to for handling different aspects of the call lifecycle and interactions.
- #### Key Events:
  - `callDidStart`: Emitted when a call successfully begins.
  - `callDidEnd:` Emitted when a call is successfully ended.
  - `appMessageReceived([String: Any], from: Daily.ParticipantID)`: Occurs when a message is received during the call. Live transcripts and function calls will be sent through this.
  -`error(Swift.Error)`: Triggered if there's an error during the call setup or execution.

## Implementing in Your Project
To see these methods in action, refer to our example files:

- SwiftUI Example: Check the `SwiftUICallView.swift` file in the Example folder. This file demonstrates the integration of these methods in a SwiftUI view.
- UIKit Example: Look at the `UIKitCallViewController.swift` file for an example of using the indigenius SDK within a UIKit view controller.

These examples will guide you through effectively implementing and managing voice calls in your iOS applications using the indigenius SDK.
