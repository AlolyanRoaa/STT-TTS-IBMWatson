# STT-TTS-IBMWatson

Python application that uses a IBM Watson services speech to text and text to speech, this project has used the code in [this repository](https://github.com/IBM/watson-streaming-stt)


PyCharm IDE was used while implementing this project.

## Outline

- Set up Watson services and clone the code in [repository](https://github.com/IBM/watson-streaming-stt)
- Requirements and Dependencies
- Updates needed 
- Demo
- The Problems Faced in This Project


## Set up Watson services and Clone the code


### Set up Watson services


first you must login into your IBM Cloud through https://cloud.ibm.com/ , go to catalog > services > speech-to-text. then select your location and click create in the right bottom side of the page, do the same to create Text to speech service.


<img src="https://github.com/AlolyanRoaa/STT-TTS-IBMWatson/blob/main/images/01-services.PNG" width="500">


in each service go to Manage and keep the API key and the region on URL of the service


### Clone the code


make sure in your preferences that you are signed into github, and after creating new project go to VCS menu and click on Get from Version Control


<img src="https://github.com/AlolyanRoaa/STT-TTS-IBMWatson/blob/main/images/02-VCSmenu.png" width="600">


in the pop up window paste the url of the repository you want to clone, in our case https://github.com/IBM/watson-streaming-stt and click on Clone button.


<img src="https://github.com/AlolyanRoaa/STT-TTS-IBMWatson/blob/main/images/03-getFromVerControl%20window.PNG" width="600">


now all the codes and files in the repository will be available in your project.


## Requirements and Dependencies


Open a terminal under the project directory, and run these commands to install all dependencies needed

```bash
pip install ibm_watson
```

and requirements in the requirements file


```bash
pip install -r requirements.txt
```

to play mb3 files


```bash
pip install playsound
```

## Updates needed


### Speech to Text


go to *speech.cfg.example* file and re-name it by removing *.example* to be *speech.cfg*, and paste the API key and region of Speech to Text service.


in the main function call *on_close(ws)* function in the end, so the last sentence the machine catches will print back on the terminal. 


![04-on_close(ws)](https://github.com/AlolyanRoaa/STT-TTS-IBMWatson/blob/main/images/04-on_close(ws).PNG)


*Note: after this step your application will run smoothly the Speech to Text service, run the command `python transcribe.py -t 5` on terminal. note that the number at the end represents the duration of the recording change it based on your preferences.*


now we will get the output of the Speech to Text into *output.text* file, to do that go to *on_close(ws)* function and add this code


```bash
# now write the final into output.txt file
with codecs.open('output.txt', 'w', encoding='utf-8') as f:
json.dump(transcript, f, ensure_ascii=False)
```


### Text to Speech


create a new function *convert_to_voice()* and write this code on it


```bash
 # the model of TTS
    key = '<API key of TTS>'
    url = '<URL of TTS>'

    # Authenticate
    authenticator = IAMAuthenticator(key)
    tts = TextToSpeechV1(authenticator=authenticator)
    tts.set_service_url(url)

    # Reading from a File
    with open('output.txt', 'r') as f:
        text = f.readlines()

    # replace of  " to space
    text = [line.replace('"', '') for line in text]
    text = ''.join(str(line) for line in text)

    # output as mp3 file
    with open('./voiceReply.mp3', 'wb') as audio_file:
        res = tts.synthesize(text, accept='audio/mp3', voice='en-GB_JamesV3Voice').get_result()
        audio_file.write(res.content)
```

and call the *convert_to_voice()* function at the end on main function.


now to play the mb3 file the TTS made create a *play_reply()* function and call it at the end of main function


```bash
def play_reply():
    playsound('voiceReply.mp3')
    # this print just to clear the channel
    print(" ")
```

![05-callFuncions](https://github.com/AlolyanRoaa/STT-TTS-IBMWatson/blob/main/images/05-callFuncions.PNG)

## Demo


https://user-images.githubusercontent.com/85321139/125677087-88863a9a-60c4-4f41-85bd-436e7e9b096e.mp4


<img src="https://github.com/AlolyanRoaa/STT-TTS-IBMWatson/blob/main/images/06-picDemo.PNG" width="600">


## The Problems Faced in This Project


1- had an errors installing pyaudio using pip. so, i use pipwin insteade. to install pipwin 


```bash
pip install pipwin
```


then install pyaudio


```bash
pipwin install pyaudio
```

2- had Handshake status 403 Forbidden error


![07-error](https://github.com/AlolyanRoaa/STT-TTS-IBMWatson/blob/main/images/07-error.PNG)


i needed to change the region to us-south and run it again.


