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