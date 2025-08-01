# F5-TTS Server

## STEP 1
please install f5 tts base repo first
F5-TTS Server is a FastAPI application that provides endpoints for text-to-speech conversion and voice cloning. It offers a simple and efficient way to generate natural-sounding speech from text using various voices and accents.

This project is built on top of [F5-TTS](https://github.com/SWivid/F5-TTS), a text-to-speech system that enables high-quality voice synthesis and cloning.

## RunPod

There is a RunPod template for this project: [https://runpod.io/console/deploy?template=x9wtzn9izl&ref=2vdt3dn9](https://runpod.io/console/deploy?template=x9wtzn9izl&ref=2vdt3dn9)
The cheapest GPU is enough to run the server.

## Features

- Text-to-speech conversion with customizable voices
- Voice cloning capabilities
- Adjustable speech speed
- Audio file upload and processing

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/ValyrianTech/F5-TTS_server.git
   ```
2. Navigate to the project directory:
   ```bash
   cd F5-TTS_server
   ```
3. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

To start the server, run:

```bash
uvicorn f5-tts_server.server:app --host 0.0.0.0 --port 7860
```

The server provides the following endpoints:

### 1. Base Text-to-Speech

Performs text-to-speech conversion using the base speaker.

**Endpoint:** `/base_tts/`

**Method:** `GET`

**Parameters:**
- `text` (str): The text to convert to speech
- `speed` (float, optional): Speech speed. Default: 1.0

### 2. Voice Change

Changes the voice of an existing audio file.

**Endpoint:** `/change_voice/`

**Method:** `POST`

**Parameters:**
- `reference_speaker` (str): The reference speaker name
- `file` (file): Audio file to process

### 3. Upload Audio

Upload a reference audio file for voice cloning.

**Endpoint:** `/upload_audio/`

**Method:** `POST`

**Parameters:**
- `audio_file_label` (str): Label for the uploaded audio
- `file` (file): Audio file to upload

### 4. Synthesize Speech

Synthesize speech using a specific voice and style.

**Endpoint:** `/synthesize_speech/`

**Method:** `GET`

**Parameters:**
- `text` (str): Text to synthesize
- `voice` (str): Voice to use
- `speed` (float, optional): Speech speed. Default: 1.0

## Response Headers

All synthesis endpoints include these response headers:
- `x-elapsed-time`: Time taken for synthesis (seconds)
- `x-device-used`: Device used for synthesis (CPU/GPU)

## Example Usage

```python
import requests

# Basic text-to-speech
url = "http://localhost:7860/base_tts/"
params = {
    "text": "Hello, this is a test.",
    "speed": 1.0
}

response = requests.get(url, params=params)
with open("output.wav", "wb") as f:
    f.write(response.content)

# Upload reference audio
url = "http://localhost:7860/upload_audio/"
files = {
    'file': ('reference.wav', open('reference.wav', 'rb'), 'audio/wav')
}
data = {
    'audio_file_label': 'custom_voice'
}

response = requests.post(url, files=files, data=data)
print(response.json())  # Should print: {"message": "File reference.wav uploaded successfully with label custom_voice."}

# Voice cloning
url = "http://localhost:7860/synthesize_speech/"
params = {
    "text": "Hello, this is a test.",
    "voice": "custom_voice",
    "speed": 1.0
}

response = requests.get(url, params=params)
with open("cloned_voice.wav", "wb") as f:
    f.write(response.content)
```
