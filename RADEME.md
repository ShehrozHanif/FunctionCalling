# 🚗 First Scenario: Car-Bot AI
 
 This part of the code creates an AI chatbot (Car-Bot) that can:


 * Get the car ready

 * Start driving in auto mode
 
 * Stop the car when required



## 🔹 Step 1: Install Google Gemini AI Library

     !pip install -U -q "google-generativeai>=0.7.2"

### What's Happening?

* You're installing google-generativeai, the official Python library for Google's Gemini AI models.

* The -U flag means "upgrade if needed", and -q makes the installation quieter.

#### 🛠 Real-world example:

Think of this as installing an app on your phone before using it.



## 🔹 Step 2: Import and Configure API Key


     from google.colab import userdata
    import google.generativeai as genai

    genai.configure(api_key=userdata.get("GOOGLE_API_KEY"))


### What's Happening?

* userdata.get("GOOGLE_API_KEY"): Retrieves the API key securely from Colab's user data.

* genai.configure(...): Configures Gemini AI with your API key so it can process requests.

#### 🛠 Real-world example:

Think of this as logging into an AI assistant using your account before it can perform tasks for you.




## 🔹 Step 3: Define Car Control Functions

    def ready_car():
        """Get the car ready for drive."""
        print("CARBOT: Car is ready for drive.")

    def drive_the_car():
        """Set the drive in auto mode. Car must be ready before drive."""
        print(f"CARBOT: Car is in auto drive mode.")

    def stop_the_car():
        """Press the brake and stop the car."""
        print("CARBOT: Stop the Car.")



### What's Happening?

* These functions simulate how a car responds to voice commands.

* When called, they print a message about the car's current action.

#### 🛠 Real-world example:

This is like a self-driving car assistant where saying "start driving" makes the car go.




## 🔹 Step 4: Create the AI Model

    car_controls = [ready_car, drive_the_car, stop_the_car]
    instruction = "You are a CAR bot. You can ready the car before drive and drive the car in auto mode and can stop the car when required."

    model = genai.GenerativeModel(
        "models/gemini-1.5-pro", tools=car_controls, system_instruction=instruction
    )

    chat = model.start_chat()


### What's Happening?

* tools=car_controls: Gemini AI is given control over the car functions.

* system_instruction=instruction: Defines the bot’s role and rules.

#### 🛠 Real-world example:

Think of this as training a new self-driving car AI on how to respond to different commands.




## 🔹 Step 5: Ask the Car-Bot What It Can Do

    from google.generativeai.types import content_types
    from collections.abc import Iterable

    def tool_config_from_mode(mode: str, fns: Iterable[str] = ()):
        """Create a tool config with the specified function calling mode."""
        return content_types.to_tool_config(
            {"function_calling_config": {"mode": mode, "allowed_function_names": fns}}
        )


### What's Happening?

 * This function creates a tool configuration that decides:

  * How the AI calls functions.

  * Which functions it is allowed to call.

#### 🛠 Real-world example:

This is like setting parental controls on a TV—deciding what the AI can and cannot do.



## 🔹 Step 6: Test the AI Without Function Calling

        tool_config = tool_config_from_mode("none")

        response = chat.send_message(
            "Hello car-bot, what can you do?", tool_config=tool_config
        )
        print(response.text)
 

### What's Happening?

* The AI responds without calling functions.

* "none" mode means the AI will only generate text.

#### 🛠 Real-world example:

This is like asking Siri or Google Assistant about its features, and it replies without performing any actions.




## 🔹 Step 7: Auto-Execute "Ready the Car"

    tool_config = tool_config_from_mode("auto")

    response = chat.send_message("ready the car", tool_config=tool_config)
    print(response.parts[0])
    chat.rewind();


### What's Happening?

* "auto" mode allows the AI to automatically call functions when needed.

* It executes ready_car() when the user says "ready the car".

#### 🛠 Real-world example:

This is like saying "Turn on the engine", and the car automatically starts.




## 🔹 Step 8: Control Which Functions Can Be Called

    available_fns = ["drive_the_car", "stop_the_car"]

    tool_config = tool_config_from_mode("any", available_fns)

    response = chat.send_message("its too high speed", tool_config=tool_config)
    print(response.parts[0])


### What's Happening?

* "any" mode allows AI to choose which functions to call.

* Only drive_the_car and stop_the_car are available.

* The AI decides that if the speed is too high, it should stop the car.

#### 🛠 Real-world example:

This is like a cruise control system that slows down when detecting high speed.




# 💡 Second Scenario: Smart Light Control

 This part of the code creates an AI model that controls smart room lights.


## 🔹 Step 1: Define Light Control Function

    from typing import Literal

    def set_light_values(brightness:int, color_temp:Literal["daylight", "cool", "warm"]):
        """Set the brightness and color temperature of a room light. (mock API)."""
        return {
            "brightness": brightness,
            "colorTemperature": color_temp
        }


### What's Happening?

* brightness: Controls how bright the lights should be.

* color_temp: Allows only "daylight", "cool", or "warm".

#### 🛠 Real-world example:

This is like a smart home app where you can adjust room lighting.



## 🔹 Step 2: Create Gemini Model with Light Controls

    model = genai.GenerativeModel(model_name='gemini-1.5-flash',
                                tools=[set_light_values])

### What's Happening?

* The AI can now control the lights.

#### 🛠 Real-world example:

This is like setting up Google Home to control your smart lights.


## 🔹 Step 3: Enable AI-Controlled Light Adjustments

    chat = model.start_chat(enable_automatic_function_calling=True)

    response = chat.send_message('Dim the lights so the room feels cozy and warm.')
    response.text


### What's Happening?

1. The AI understands "Dim the lights" means:
 
 * Brightness should be low.

 * Color temperature should be "warm".

#### 🛠 Real-world example:

This is like asking Alexa: "Make my room cozy", and it automatically dims the lights.



Thanks for sharing your code! I'll explain each step in a beginner-friendly way with real-world examples so you can fully understand how function calling works in Google Colab with Gemini models. 🚀

Understanding Function Calling with Google Gemini AI
You're creating a function-calling AI model that can interact with real-world functions like controlling a car or adjusting lights. The AI recognizes user commands and calls the appropriate functions automatically.

🚗 First Scenario: Car-Bot AI
This part of the code creates an AI chatbot (Car-Bot) that can:

Get the car ready
Start driving in auto mode
Stop the car when required
🔹 Step 1: Install Google Gemini AI Library
python
Copy
Edit
!pip install -U -q "google-generativeai>=0.7.2"
What's Happening?
You're installing google-generativeai, the official Python library for Google's Gemini AI models.
The -U flag means "upgrade if needed", and -q makes the installation quieter.
🛠 Real-world example:
Think of this as installing an app on your phone before using it.

🔹 Step 2: Import and Configure API Key
python
Copy
Edit
from google.colab import userdata
import google.generativeai as genai

genai.configure(api_key=userdata.get("GOOGLE_API_KEY"))
What's Happening?
userdata.get("GOOGLE_API_KEY"): Retrieves the API key securely from Colab's user data.
genai.configure(...): Configures Gemini AI with your API key so it can process requests.
🛠 Real-world example:
Think of this as logging into an AI assistant using your account before it can perform tasks for you.

🔹 Step 3: Define Car Control Functions
python
Copy
Edit
def ready_car():
    """Get the car ready for drive."""
    print("CARBOT: Car is ready for drive.")

def drive_the_car():
    """Set the drive in auto mode. Car must be ready before drive."""
    print(f"CARBOT: Car is in auto drive mode.")

def stop_the_car():
    """Press the brake and stop the car."""
    print("CARBOT: Stop the Car.")
What's Happening?
These functions simulate how a car responds to voice commands.
When called, they print a message about the car's current action.
🛠 Real-world example:
This is like a self-driving car assistant where saying "start driving" makes the car go.

🔹 Step 4: Create the AI Model
python
Copy
Edit
car_controls = [ready_car, drive_the_car, stop_the_car]
instruction = "You are a CAR bot. You can ready the car before drive and drive the car in auto mode and can stop the car when required."

model = genai.GenerativeModel(
    "models/gemini-1.5-pro", tools=car_controls, system_instruction=instruction
)

chat = model.start_chat()
What's Happening?
tools=car_controls: Gemini AI is given control over the car functions.
system_instruction=instruction: Defines the bot’s role and rules.
🛠 Real-world example:
Think of this as training a new self-driving car AI on how to respond to different commands.

🔹 Step 5: Ask the Car-Bot What It Can Do
python
Copy
Edit
from google.generativeai.types import content_types
from collections.abc import Iterable

def tool_config_from_mode(mode: str, fns: Iterable[str] = ()):
    """Create a tool config with the specified function calling mode."""
    return content_types.to_tool_config(
        {"function_calling_config": {"mode": mode, "allowed_function_names": fns}}
    )
What's Happening?
This function creates a tool configuration that decides:
How the AI calls functions.
Which functions it is allowed to call.
🛠 Real-world example:
This is like setting parental controls on a TV—deciding what the AI can and cannot do.

🔹 Step 6: Test the AI Without Function Calling
python
Copy
Edit
tool_config = tool_config_from_mode("none")

response = chat.send_message(
    "Hello car-bot, what can you do?", tool_config=tool_config
)
print(response.text)
What's Happening?
The AI responds without calling functions.
"none" mode means the AI will only generate text.
🛠 Real-world example:
This is like asking Siri or Google Assistant about its features, and it replies without performing any actions.

🔹 Step 7: Auto-Execute "Ready the Car"
python
Copy
Edit
tool_config = tool_config_from_mode("auto")

response = chat.send_message("ready the car", tool_config=tool_config)
print(response.parts[0])
chat.rewind();
What's Happening?
"auto" mode allows the AI to automatically call functions when needed.
It executes ready_car() when the user says "ready the car".
🛠 Real-world example:
This is like saying "Turn on the engine", and the car automatically starts.

🔹 Step 8: Control Which Functions Can Be Called
python
Copy
Edit
available_fns = ["drive_the_car", "stop_the_car"]

tool_config = tool_config_from_mode("any", available_fns)

response = chat.send_message("its too high speed", tool_config=tool_config)
print(response.parts[0])
What's Happening?
"any" mode allows AI to choose which functions to call.
Only drive_the_car and stop_the_car are available.
The AI decides that if the speed is too high, it should stop the car.
🛠 Real-world example:
This is like a cruise control system that slows down when detecting high speed.

💡 Second Scenario: Smart Light Control
This part of the code creates an AI model that controls smart room lights.

🔹 Step 1: Define Light Control Function
python
Copy
Edit
from typing import Literal

def set_light_values(brightness:int, color_temp:Literal["daylight", "cool", "warm"]):
    """Set the brightness and color temperature of a room light. (mock API)."""
    return {
        "brightness": brightness,
        "colorTemperature": color_temp
    }
What's Happening?
brightness: Controls how bright the lights should be.
color_temp: Allows only "daylight", "cool", or "warm".
🛠 Real-world example:
This is like a smart home app where you can adjust room lighting.

🔹 Step 2: Create Gemini Model with Light Controls
python
Copy
Edit
model = genai.GenerativeModel(model_name='gemini-1.5-flash',
                              tools=[set_light_values])
What's Happening?
The AI can now control the lights.
🛠 Real-world example:
This is like setting up Google Home to control your smart lights.

🔹 Step 3: Enable AI-Controlled Light Adjustments
python
Copy
Edit
chat = model.start_chat(enable_automatic_function_calling=True)

response = chat.send_message('Dim the lights so the room feels cozy and warm.')
response.text
What's Happening?
The AI understands "Dim the lights" means:
Brightness should be low.
Color temperature should be "warm".
🛠 Real-world example:
This is like asking Alexa: "Make my room cozy", and it automatically dims the lights.

## 🚀 Summary

You’ve built two **AI assistants**:

1. Car-Bot AI that controls a car's functions.

2. Smart Light AI that adjusts room lighting.

Each AI:

* Understands human language.

* Calls specific functions to perform real-world tasks.

* Uses different function calling modes (none, auto, any).

This is exactly how modern AI-powered assistants work! 🔥
