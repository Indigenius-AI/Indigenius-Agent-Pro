# **Indigenius Flutter SDK**

## **Minimum requirements**
The Daily Client SDK for Flutter requires the following versions:

- Flutter ≥ 3.0.0
- iOS ≥ 13.0 (objective-C and Swift applications are supported)
- Android compileSdkVersion ≥ 33
- Android minSdkVersion ≥ 24
- Android NDK ≥ 25.1.8937393

## **Setup**
Add `indigenius` as a dependency:

````bash
flutter pub add indigenius
````
Then, follow the platform-specific setup instructions for `permission_handler`:

## **iOS**

According to the permission_handler instructions above, add the permission flags for microphone.

Also add this to your Info.plist:

````bash
<key>NSMicrophoneUsageDescription</key>
<string>This app requires access to the microphone for live audio calls.</string>
````

You'll also need to ensure this is set in your Podfile:

````bash
post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
    target.build_configurations.each do |config|
      config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
        '$(inherited)',

        'PERMISSION_MICROPHONE=1',
      ]
    end
  end
end
````
We also recommend adding the audio background mode to your app's capabilities.

## **Android**

Add the necessary permissions to your AndroidManifest.xml:

````bash
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
````
Add the permission flags for microphone according to the permission_handler instructions above.

## **Usage**
First, import the Vapi class from the package:

````bash
import 'package:indigenius/indigenius.dart';
````

Then, create a new instance of the Vapi class, passing your Public Key as a parameter to the constructor:

````bash
var indigenius = indigenius('your-public-key');
````
You can start a new call by calling the `start` method and passing an `assistant` object or `assistantId`:

````bash
await indigenius.start(assistant: {
  "model": {
    "provider": "openai",
    "model": "gpt-3.5-turbo",
    "systemPrompt": "You're an assistant..."
  },
   "voice": {
    "provider": "11labs",
    "voiceId": "roger",
  },
  ...
});
````
````bash
await indigenius.start(assistantId: "your-assistant-id");
````
The `star`t method will initiate a new call.

You can override existing assistant parameters or set variables with the `assistant_overrides` parameter. Assume the first message is `Hey, {{name}} how are you? and you want to set the value of `name` to `John`:

````bash
final assistantOverrides = {
  'recordingEnabled': false,
  'variableValues': {
    'name': 'Ana',
  },
};

await indigenius.start(
  assistantId: 'your-assistant-id',
  assistantOverrides: assistantOverrides,
);

````bash
You can send text messages to the assistant aside from the audio input using the send method and passing appropriate role and content.

indigenius.send({
  "type": "add-message",
  "message": {
    "role": "system",
    "content": "The user has pressed the button, say peanuts",
  },
});
````
Possible values for the role are system, user, assistant, tool or function.


The `setMuted(muted: boolean)` can be used to mute and un-mute the user's microphone.

````bash
indigenius.isMuted(); // false
indigenius.setMuted(true);
indigenius.isMuted(); // true
````

## **Events**
You can listen to the following events:

````bash
indigenius.onEvent.listen((event) {
    if (event.label == "call-start") {
        print('call started');
    }
    if (event.label == "call-end") {
        print('call ended');
    }

    // Speech statuses, function calls and transcripts will be sent via messages
    if (event.label == "message") {
        print(event.value);
    }
});
````

These events allow you to react to changes in the state of the call or speech.

## **Example**
An example can be found in the repo [here](https://github.com/daraPM/Indigenius-Agent-Pro/blob/main/main.dart.md)
