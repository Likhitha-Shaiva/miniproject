import smtplib
import speech_recognition as sr
import pyttsx3
from email.message import EmailMessage
import noisereduce as nr
import numpy as np


# Function to reduce noise in the recorded audio
def reduce_noise(audio_data, rate):
    # Convert byte data to numpy array for processing
    audio_np = np.frombuffer(audio_data, dtype=np.int16)
    # Apply noise reduction
    reduced_noise_audio = nr.reduce_noise(y=audio_np, sr=rate)
    # Convert the cleaned audio back to byte format
    return reduced_noise_audio.tobytes()


recognizer = sr.Recognizer()
engine = pyttsx3.init()

def talk(text):
    engine.say(text)
    engine.runAndWait()



def tts():

    talk("Please start speaking. I am listening.")

    with sr.Microphone() as source:
        recognizer.adjust_for_ambient_noise(source)
        talk("You can speak now.")
        print("Listening...")
        audio = recognizer.listen(source)
        print("Recognizing...")

        try:

            text = recognizer.recognize_google(audio)
            print("You said:", text)
            talk("You said: " + text)
            return text

        except sr.UnknownValueError:
            print("Sorry, I could not understand the audio.")
            talk("Sorry, I could not understand what you said.")
            return None

        except sr.RequestError:
            print("Sorry, there was an error with the recognition service.")
            talk("Sorry, there was an error with the recognition service.")
            return None


email_dict = {
    "apple": "likhithajn2004@gmail.com",
    "orange": "mkruthika7007@gmail.com"
}

def send_email(sender, subject, message):
    try:
        server = smtplib.SMTP("smtp.gmail.com", 587)
        server.starttls()
        server.login("likhithajn2004@gmail.com", "zufc rkfk ghzy rgva")  # Replace with App Password
        email = EmailMessage()
        email["From"] = "likhithajn2004@gmail.com"
        email["To"] = sender
        email["Subject"] = subject
        email.set_content(message)
        server.send_message(email)
        server.quit()
        talk("The email has been sent successfully.")
    except Exception as e:
        print("Error:", e)
        talk("There was an error sending the email.")

def main():
    talk("From whom would you like to send the email?")
    name = tts()
    if name is None:
        talk("I could not understand the name.")
        return

    sender = email_dict.get(name.lower())
    if sender is None:
        talk("I could not find the contact.")
        return

    talk("What is the subject of the email?")
    subject = tts()
    if subject is None:
        talk("I could not understand the subject.")
        return

    talk("What would you like to say in the body of the email?")
    message = tts()
    if message is None:
        talk("I could not understand the message body.")
        return

    send_email(sender, subject, message)


main()
