# **Indigenius Python SDK**
This package lets you start indigenius calls directly in your Python application.

## **Installation**
You can install the package via pip:

````bash
pip install indigenius_python
````
On Mac, you might need to install brew install portaudio to satisfy pyaudio's dependency requirement.

## **Usage**
First, import the indigenius class from the package:

````bash
from indigenius_python import indigenius
````

Then, create a new instance of the indigenius class, passing your Public Key as a parameter to the constructor:

```bash
indigenius = indigenius(api_key='your-public-key')
````
You can start a new call by calling the start method and passing an assistant object or assistantId. You can find the available options here: docs.indigenius.ai

````bash
indigenius.start(assistant_id='your-assistant-id')
````
or
````
bash
assistant = {
  'firstMessage': 'Hey, how are you?',
  'context': 'You are an employee at a drive thru...',
  'model': 'gpt-3.5-turbo',
  'voice': 'jennifer-playht',
  "recordingEnabled": True,
  "interruptionsEnabled": False
}

indigenius.start(assistant=assistant)
````
The start method will initiate a new call.

You can override existing assistant parameters or set variables with the assistant_overrides parameter. Assume the first message is Hey, {{name}} how are you? and you want to set the value of name to John:

```bash
assistant_overrides = {
  "recordingEnabled": False,
  "variableValues": {
    "name": "John"
  }
}
indigenius.start(assistant_id='your-assistant-id', assistant_overrides=assistant_overrides)
````

