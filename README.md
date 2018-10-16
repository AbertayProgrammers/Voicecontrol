# Voicecontrol

#!/usr/bin/env python3
# Requires PyAudio and PySpeech.

import pyautogui
import speech_recognition as sr
import time
import os
from time import ctime
from gtts import gTTS

# this method plays the audio file that gets created every time that the program responds
def speak(audioString):
    print(audioString)
    tts = gTTS(text=audioString, lang='en')
    tts.save("audio.mp3")
    os.system("mpg321 audio.mp3")

def recordAudio(f):
    # Record Audio
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Say something!")
        audio = r.listen(source)

    # Speech recognition using Speech Recognition
    data = ""
    try:
        # Uses the default API key
        # To use another API key: `r.recognize_google(audio, key="GOOGLE_SPEECH_RECOGNITION_API_KEY")`
        data = r.recognize_google(audio)
        print("You said: " + data)
        f.write(data+'\n')
    except sr.UnknownValueError:
        print("Speech Recognition could not understand audio")
    except sr.RequestError as e:
        print("Could not request results from Google Speech Recognition service; {0}".format(e))

    return data

def jarvis(data):

    if "how are you" in data:
        speak("I am fine")

    if "what time is it" in data:
        speak(ctime())

    if "next" in data:
        # this is to control the slide deck to go forwards
        #mouse.click(Button.left, 1)
        pyautogui.press('return')

    if "back" in data:
        # this is to control the slide deck to go backwards
        #mouse.clock(Button.left, 1)
        pyautogui.press('backspace')

    if "#Skynet" in data:
        speak("I promise I won't kill you. # tag winky face.")

# initialization
time.sleep(.5)
# calls the speak function to play the audio file created with the string to be played.
speak("Hi Abertay Programming society, what can I do for you?")
# open a file that all that has been spoken in will be recorded to this file.
with open('/home/uroot/Documents/trail.txt', 'w+') as f:
    # just keep going forever, never stopping, because quitting is for losers. :P
    while 1:
        data = recordAudio(f)
        jarvis(data)
    f.close()
