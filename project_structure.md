# WhisperLiveKit Project Structure

WhisperLiveKit is a Python project that provides real-time, fully local speech-to-text transcription and speaker diarization capabilities using Whisper. The project is structured to handle live audio processing, transcription, and speaker identification.

## Directory Structure

```
WhisperLiveKit/
├── setup.py                           # Package installation configuration
├── whisper_fastapi_online_server.py   # Main FastAPI server implementation
├── whisperlivekit/
│   ├── __init__.py
│   ├── audio_processor.py             # Core audio processing functionality
│   ├── basic_server.py               
│   ├── core.py                        # Core configuration and initialization
│   ├── silero_vad_iterator.py         # Voice Activity Detection implementation
│   ├── timed_objects.py               # Time-related data structures
│   ├── diarization/                   # Speaker diarization functionality
│   │   ├── __init__.py
│   │   └── diarization_online.py      # Real-time speaker identification
│   ├── web/                           # Web interface components
│   │   ├── __init__.py
│   │   └── live_transcription.html    # Browser-based interface
│   └── whisper_streaming_custom/      # Custom Whisper streaming implementation
│       ├── __init__.py
│       ├── backends.py                # Different Whisper backend implementations
│       ├── online_asr.py             # Online ASR processing
│       └── whisper_online.py         # Whisper streaming core functionality
```

## Key Components

### 1. Core Module (core.py)
- Provides the main `WhisperLiveKit` class that initializes and configures the system
- Handles command-line argument parsing for various configuration options
- Manages model initialization for ASR and diarization
- Supports multiple Whisper backends (faster-whisper, whisper_timestamped, mlx-whisper, openai-api)

### 2. Audio Processor (audio_processor.py)
- Central component for audio stream processing
- Features:
  - Real-time audio buffering and processing
  - FFmpeg integration for audio format conversion
  - Concurrent processing of transcription and diarization
  - State management for transcription and speaker identification
  - Results formatting and streaming

### 3. Diarization Module (diarization/)
- Implements speaker identification functionality
- Uses Diart for real-time speaker diarization
- Integrates with pyannote.audio models for speaker segmentation

### 4. Whisper Streaming Custom (whisper_streaming_custom/)
- Custom implementation of Whisper for streaming audio
- Supports multiple backend options:
  - faster-whisper
  - whisper_timestamped
  - mlx-whisper (optimized for Apple Silicon)
  - OpenAI API

### 5. Web Interface (web/)
- Browser-based interface for audio capture and visualization
- Real-time display of transcription and speaker identification
- WebSocket-based communication with the backend

## Key Features

1. **Real-time Processing**
   - Streams audio directly from browser
   - Processes audio in chunks for immediate feedback
   - Supports multiple simultaneous users

2. **Voice Activity Detection**
   - Optional VAD to prevent hallucinations
   - Configurable chunk size and processing parameters

3. **Flexible Configuration**
   - Multiple Whisper model options
   - Configurable language settings
   - Adjustable buffer and processing parameters

4. **Speaker Diarization**
   - Real-time speaker identification
   - Integration with pyannote.audio models
   - Speaker attribution for transcribed segments

5. **Multiple Backend Support**
   - faster-whisper
   - whisper_timestamped
   - mlx-whisper (Apple Silicon optimization)
   - OpenAI API integration

## Technical Implementation

### Audio Processing Pipeline
1. Audio capture via browser's MediaRecorder API
2. WebSocket streaming of audio chunks
3. FFmpeg conversion to PCM format
4. Parallel processing:
   - Voice activity detection
   - Speech transcription
   - Speaker diarization
5. Real-time result formatting and client updates

### State Management
- Maintains buffer state for transcription and diarization
- Handles concurrent processing with asyncio
- Manages speaker attribution and text alignment
- Provides formatted results with timing information

### Error Handling
- Robust error recovery for FFmpeg processing
- Automatic restart of failed components
- Graceful degradation of services
- Comprehensive logging system

## Configuration Options
The system supports extensive configuration through command-line arguments or programmatic initialization:

- Model selection and caching
- Language and task settings
- Processing parameters
- Voice activity detection
- Buffer management
- Server configuration
- Diarization settings

This project provides a complete solution for real-time speech processing with modular architecture and extensive configuration options.
