import speech_recognition as sr
import pyttsx3
import time

# Initialize the recognizer and TTS engine
recognizer = sr.Recognizer()
engine = pyttsx3.init()

# Define TTS function
def speak(text):
    engine.say(text)
    engine.runAndWait()

# Define robot action functions
def move_forward():
    speak("Moving forward")
    print("Robot is moving forward")

def move_backward():
    speak("Moving backward")
    print("Robot is moving backward")

def turn_left():
    speak("Turning left")
    print("Robot is turning left")

def turn_right():
    speak("Turning right")
    print("Robot is turning right")

def stop():
    speak("Stopping")
    print("Robot has stopped")

# Main function to listen and control robot
def listen_and_control():
    with sr.Microphone() as source:
        print("Say a command: forward, backward, left, right, stop")
        recognizer.adjust_for_ambient_noise(source)
        
        try:
            audio = recognizer.listen(source)
            command = recognizer.recognize_google(audio).lower()
            print(f"Recognized command: {command}")
            
            # Execute command
            if "forward" in command:
                move_forward()
            elif "backward" in command:
                move_backward()
            elif "left" in command:
                turn_left()
            elif "right" in command:
                turn_right()
            elif "stop" in command:
                stop()
            else:
                speak("Unknown command, please try again.")
                print("Unknown command")

        except sr.UnknownValueError:
            speak("Sorry, I did not understand that.")
            print("Could not understand the command")
        except sr.RequestError as e:
            speak("Speech recognition service is unavailable.")
            print(f"Could not request results; {e}")

# Continuous listening loop
try:
    while True:
        listen_and_control()
        time.sleep(1)  # Small delay between commands for responsiveness
except KeyboardInterrupt:
    speak("Shutting down")
    print("Robot control ended.")
