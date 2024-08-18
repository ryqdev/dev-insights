

## Install in MacOS
```shell
pip install SpeechRecognition

# Install pyaudio
brew install portaudio
pip install pyaudio

# install pocketsphinx
pip install pocketsphinx
```

demo code:
```python
import speech_recognition as sr
# obtain audio from the microphone
r = sr.Recognizer()
with sr.Microphone() as source:
    print("Say something!")
    audio = r.listen(source)
# recognize speech using Sphinx
try:
    print("Speaker: " + r.recognize_sphinx(audio))
except sr.UnknownValueError:
    print("Sphinx could not understand audio")
except sr.RequestError as e:
    print("Sphinx error; {0}".format(e))
```