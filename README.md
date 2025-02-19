# Whisper Streaming Web: Real-time Speech-to-Text with Web UI & FastAPI WebSocket

This fork of [Whisper Streaming](https://github.com/ufal/whisper_streaming) adds a ready-to-use HTML interface, making it super easy to start transcribing audio directly from your browser. Just launch the local server, allow microphone access, and start streaming. Everything runs locally on your machine 🎙️✨

## What's New?  
✅ **Built-in Web UI** – Just open your browser and start transcribing, no need to build a frontend.  
✅ **FastAPI Server with WebSocket Endpoint** – Enables real-time STT in browsers with async FFmpeg processing.  
✅ **Buffering Preview** – Displays unvalidated buffer content for better streaming feedback.  
✅ **Multiple Users Support** – The backend handles multiple users simultaneously without conflicts.  
✅ **HTML - JavaScript Client Implementation** – A plug-and-play MediaRecorder setup for seamless client integration
✅ **MLX Whisper Backend** – Optimized Apple Silicon support for faster local processing.  
✅ **Enhanced sentence segmentation** – Improves buffer trimming and sentence boundaries in certain languages  
✅ **Diarization (Beta)** – Real-time speaker labeling using [Diart](https://github.com/juanmc2005/diart).  

<p align="center">
  <img src="src/web/demo.png" alt="Demo Screenshot" width="600">
</p>

## Installation

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/QuentinFuxa/whisper_streaming_web
   cd whisper_streaming_web
   ```


### How to Launch the Server

1. **Dependencies**:

- Install required dependences :

    ```bash
    # Whisper streaming required dependencies
    pip install librosa soundfile

    # Whisper streaming web required dependencies
    pip install fastapi ffmpeg-python
    ```
- Install at least one whisper backend among:

    ```
   whisper
   whisper-timestamped
   faster-whisper (faster backend on NVIDIA GPU)
   mlx-whisper (faster backend on Apple Silicon)
   ```
- Optionnal dependencies

    ```
    # If you want to use VAC (Voice Activity Controller). Useful for preventing hallucinations
    torch
   
    # If you choose sentences as buffer trimming strategy
    mosestokenizer
    wtpsplit
    tokenize_uk # If you work with Ukrainian text

    # If you want to run the server using uvicorn (recommended)
    uvicorn

    # If you want to use diarization
    diart
    ```


3. **Run the FastAPI Server**:

    ```bash
    python whisper_fastapi_online_server.py --host 0.0.0.0 --port 8000
    ```

    - `--host` and `--port` let you specify the server’s IP/port. 
    - `-min-chunk-size` sets the minimum chunk size for audio processing. Make sure this value aligns with the chunk size selected in the frontend. If not aligned, the system will work but may unnecessarily over-process audio data.
    - For a full list of configurable options, run `python whisper_fastapi_online_server.py -h`
    - `--transcription`, default to True. Change to False if you want to run only diarization
    - `--diarization`, default to False, let you choose whether or not you want to run diarization in parallel
    - For other parameters, look at [whisper streaming](https://github.com/ufal/whisper_streaming) readme.

4. **Open the Provided HTML**:

    - By default, the server root endpoint `/` serves a simple `live_transcription.html` page.  
    - Open your browser at `http://localhost:8000` (or replace `localhost` and `8000` with whatever you specified).  
    - The page uses vanilla JavaScript and the WebSocket API to capture your microphone and stream audio to the server in real time.

### How the Live Interface Works

- Once you **allow microphone access**, the page records small chunks of audio using the **MediaRecorder** API in **webm/opus** format.  
- These chunks are sent over a **WebSocket** to the FastAPI endpoint at `/asr`.  
- The Python server decodes `.webm` chunks on the fly using **FFmpeg** and streams them into the **whisper streaming** implementation for transcription.  
- **Partial transcription** appears as soon as enough audio is processed. The “unvalidated” text is shown in **lighter or grey color** (i.e., an ‘aperçu’) to indicate it’s still buffered partial output. Once Whisper finalizes that segment, it’s displayed in normal text.  
- You can watch the transcription update in near real time, ideal for demos, prototyping, or quick debugging.

### Deploying to a Remote Server

If you want to **deploy** this setup:

1. **Host the FastAPI app** behind a production-grade HTTP(S) server (like **Uvicorn + Nginx** or Docker). If you use HTTPS, use "wss" instead of "ws" in WebSocket URL.
2. The **HTML/JS page** can be served by the same FastAPI app or a separate static host.  
3. Users open the page in **Chrome/Firefox** (any modern browser that supports MediaRecorder + WebSocket).  

No additional front-end libraries or frameworks are required. The WebSocket logic in `live_transcription.html` is minimal enough to adapt for your own custom UI or embed in other pages.

## Acknowledgments

This project builds upon the foundational work of the Whisper Streaming project. We extend our gratitude to the original authors for their contributions.

