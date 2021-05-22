import pyttsx3
import datetime
import speech_recognition as sr 
import wikipedia
import webbrowser
import os
import random
import pyjokes
import smtplib




eng = pyttsx3.init('sapi5')
voices = eng.getProperty('voices')
eng.setProperty('voice',voices[1].id)
eng.setProperty('rate', 170)

def speak(audio):
    eng.say(audio)
    eng.runAndWait()

def WishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        print("Good Morning!")
        speak("Good Morning!")
       
    elif hour>=12 and hour<18:
        print("Good Afternoon!")
        speak("Good Afternoon!")
        

    else:
        print("Good Evening!") 
        speak("Good Evening!")
        
    print("Hello Sir. I am Alexis. How may I help you")    
    speak("Hello Sir. I am Alexis. How may I help you")
    
def TakeCommand():
    rp = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening. . .")
        rp.pause_threshold = 1
        rp.energy_threshold = 400
        audio = rp.listen(source)
    try:
        print("recognizing...")
        query = rp.recognize_google(audio, language="en-in")
        print("user said:", query)



    except Exception as e:
        print(e)
        print("Say that again please")
        return "None"
    return query
def SendEmail(to, content):
    ser = smtplib.SMTP('smtp.gmail.com', 587)
    ser.ehlo()
    ser.starttls()
    ser.login('Youremail@gmail.com', 'your email passwaord')
    ser.sendmail('youremail@gmail.com', to, content)
    ser.close()


WishMe()
while True:
    query = TakeCommand().lower()

    if "wikipedia" in query:
        print("Searching Wikipedia...")
        speak("Searching Wikipedia")
        query = query.replace("wikipedia", "")
        ans = wikipedia.summary(query, sentences = 2)
        print("According to wikipedia...")
        speak("According to Wikipedia")
        print(ans)
        speak(ans)
    elif "open youtube" in query:
        print("Opening Youtube")
        speak("Opening Youtube")
        webbrowser.open("youtube.com")
    
    elif "stackoverflow" in query:
        print("Opening stackoverflow")
        speak("Opening stackoverflow")
        webbrowser.open("stackoverflow.com")
    elif "play movie" in query:
        print("Which movie:")
        speak("Which movie")
    elif "any movie" in query:
        var = random.randint(0,9)
        print("playing random movie")
        speak("Playing Random Movie")
        ran = "D:\\siri test movies" # here you need to write the path of folder where you have stored movies in your pc
        mov1 = os.listdir(ran)  
        os.startfile(os.path.join(ran, mov1[var]))    
    elif "the time" in query:
        Time = datetime.datetime.now().strftime("%H:%M:%S")
        print(Time)
        speak(Time)
    elif "open google chrome" in query:
        chromePath = "C:\\Program Files (x86)\\Google\\Chrome\\Application" #here you need to write the path of google chrome application
        print("Opening Google Chrome")
        speak("Opening Google chrome")
        os.startfile(chromePath)
    elif "youtube search" in query:
        speak("Searching Youtube")
        query = query.replace("youtube search", "")
        web = "https://www.youtube.com/results?search_query=" + query
        webbrowser.open(web)
    elif "joke" in query:
        get = pyjokes.get_joke()
        speak("As you wish sir")
        speak(get)
    elif "how are you" in query:
        speak("I am fine Sir. Hope you are also good") 
    elif "send email" in query:
        try:
            speak("what should I write")
            content = TakeCommand()
            to = "Here enter the email of the person whom you want to send email"
            SendEmail(to, content)
            print("Sending Email")
            speak("Sending Email")
            print("Email has been send")
            speak("Email has been send")
        except Exception as e:
            speak("I am not able to send this Email") 
    elif "open visual studio code" in query:
        print("Opening visual studio code")
        speak("Opening visual studio code")
        os.startfile("C:\\Users\\acer\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe") # here you need to write path of vscode application
    elif "wasn't funny" in query:
        speak("Sorry Sir")
        speak("Anyway, thanks for your time")