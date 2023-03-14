# sprachsteuerung-termux

import speech_recognition as sr
import os
import geocoder
import pyttsx3

# Sprachmodelle für Deutsch und Russisch
languages = {'de': 'de-DE', 'ru': 'ru-RU'}

# Funktion zum Sprechen des Textes
def speak(text):
    engine = pyttsx3.init()
    engine.setProperty('rate', 150)
    engine.say(text)
    engine.runAndWait()

# Benutzerdefinierte Sprachsteuerung
def voice_control():
    # Erstellen Sie ein Recognizer-Objekt
    r = sr.Recognizer()
    
    # Verwenden Sie das Mikrofon als Quelle
    with sr.Microphone() as source:
        print("Sprechen Sie jetzt")
        speak("Sprechen Sie jetzt")
        audio = r.listen(source)

        # Versuchen, die Sprache mit jedem Sprachmodell zu erkennen
        for lang_code, lang_model in languages.items():
            try:
                text = r.recognize_google(audio, language=lang_model)
                print(f"Erkannter Text ({lang_code}): {text}")
                
                # Wenn der erkannte Text "Text schreiben" ist, öffnen Sie einen Texteditor
                if "text schreiben" in text.lower():
                    os.system("termux-open-editor")
                    speak("Texteditor geöffnet.")
                
                # Wenn der erkannte Text "Wo bin ich" ist, sagen Sie dem Benutzer den Standort des Geräts
                elif "wo bin ich" in text.lower():
                    g = geocoder.ip('me')
                    speak(f"Sie befinden sich in {g.city}, {g.country}.")
                
                # Fügen Sie hier weitere Anweisungen hinzu, um andere Befehle auszuführen
                
                return

            except sr.UnknownValueError:
                pass
        
    print("Fehler bei der Spracherkennung")
    speak("Fehler bei der Spracherkennung.")

# Rufen Sie die Sprachsteuerungsfunktion auf und führen Sie sie im Hintergrund aus
while True:
    try:
        voice_control()
    except KeyboardInterrupt:
        print("Ich gehe jetzt aus.")
        speak("Ich gehe jetzt aus.")



