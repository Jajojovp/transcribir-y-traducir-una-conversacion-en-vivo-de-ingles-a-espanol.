import speech_recognition as sr
from google.cloud import speech_v1p1beta1
from google.cloud.speech_v1p1beta1 import enums
from google.cloud import translate_v2 as translate
from gtts import gTTS
import pygame
import os

# Configura la autenticación para acceder a la API de Google Cloud
os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = "google_credentials.json"

# Inicializa el cliente de la API de traducción de Google Cloud
translate_client = translate.Client()

def transcribe_and_translate():
    # Inicializa el reconocimiento de voz
    r = sr.Recognizer()

    # Obtén una muestra de audio de la conversación en vivo
    with sr.Microphone() as source:
        print("Habla ahora...")
        audio = r.listen(source)

    # Inicializa el cliente de la API de Speech-to-Text de Google Cloud
    client = speech_v1p1beta1.SpeechClient()

    # Configura la solicitud de transcripción
    config = speech_v1p1beta1.RecognitionConfig(
        encoding=enums.RecognitionConfig.AudioEncoding.LINEAR16,
        sample_rate_hertz=44100,
        language_code='en-US',
        audio_channel_count=2,
        enable_word_time_offsets=True
    )

    # Envía la muestra de audio a la API para obtener la transcripción
    response = client.recognize(config, audio)

    # Obtén la transcripción del resultado
    transcription = response.results[0].alternatives[0].transcript

    # Traduce la transcripción a español usando la API de traducción de Google
    translation = translate_client.translate(transcription, target_language='es')['translatedText']

    print("Transcripción en inglés: ", transcription)
    print("Traducción al español: ", translation)

    # Convierte el texto traducido a un archivo de audio
    audio = gTTS(translation, lang='es')
    audio.save('transcription.mp3')

    # Reproduce el archivo de audio
    pygame.init()
    pygame.mixer.music.load("transcription.mp3")
    pygame.mixer.music.play()
    while pygame.mixer.music.get_busy():
        pygame.time.Clock().tick(10)

if __name__ == "__main__":
    while True:
        transcribe_and_translate()
