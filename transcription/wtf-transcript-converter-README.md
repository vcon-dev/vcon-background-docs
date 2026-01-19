# vCon WTF - Python Transcription Library

> Source: https://github.com/vcon-dev/wtf-transcript-converter

A Python library enabling conversion between various transcription provider formats and the standardized IETF World Transcription Format (WTF), with vCon container integration.

## Overview

The WTF (World Transcription Format) is a standardized format for transcription data that enables interoperability between different transcription providers. This library provides tools to convert to and from WTF format.

## Key Capabilities

- **Multi-Provider Support**: Convert transcripts from multiple providers
- **WTF Specification Compliance**: Full compliance with the IETF WTF specification
- **vCon Container Integration**: Seamlessly integrate with vCon conversation containers
- **CLI Tools**: Command-line interface for batch operations
- **Built-in Validation**: Validate transcripts against the WTF schema
- **Quality Assessment**: Evaluate transcription quality metrics

## Supported Providers

- OpenAI Whisper
- Deepgram
- AssemblyAI
- Rev.ai
- NVIDIA Canary
- NVIDIA Parakeet
- Speechmatics
- AWS Transcribe

## Installation

### From PyPI

```bash
pip install wtf-transcript-converter
```

### From Source

```bash
git clone https://github.com/vcon-dev/wtf-transcript-converter.git
cd wtf-transcript-converter
uv sync
```

## Python Usage

### Basic Conversion

```python
from wtf_converter import WhisperConverter, DeepgramConverter

# Convert Whisper output to WTF
whisper_converter = WhisperConverter()
wtf_transcript = whisper_converter.to_wtf(whisper_output)

# Validate the result
is_valid, errors = wtf_transcript.validate()
if is_valid:
    print("Transcript is valid WTF format")
else:
    print(f"Validation errors: {errors}")
```

### Converting from WTF

```python
from wtf_converter import DeepgramConverter

# Convert WTF back to Deepgram format
deepgram_converter = DeepgramConverter()
deepgram_format = deepgram_converter.from_wtf(wtf_transcript)
```

### Working with vCon

```python
from vcon import Vcon
from wtf_converter import WhisperConverter

# Load a vCon with audio
vcon = Vcon.load_from_file("conversation.json")

# Get transcription and convert to WTF
whisper_output = transcribe_audio(vcon.dialog[0].body)
converter = WhisperConverter()
wtf_transcript = converter.to_wtf(whisper_output)

# Add WTF transcript to vCon analysis
vcon.add_analysis(
    type="transcription",
    dialog=0,
    vendor="whisper",
    body=wtf_transcript.to_dict(),
    encoding="json"
)
```

## CLI Usage

### Convert to WTF

```bash
# Convert a Whisper transcript to WTF
wtf to-wtf --provider whisper input.json output.wtf.json

# Convert with validation
wtf to-wtf --provider deepgram --validate input.json output.wtf.json
```

### Convert from WTF

```bash
# Convert WTF back to provider format
wtf from-wtf --provider assemblyai input.wtf.json output.json
```

### Validate

```bash
# Validate a WTF transcript
wtf validate transcript.wtf.json
```

### Batch Processing

```bash
# Convert all transcripts in a directory
wtf batch --provider whisper --input-dir ./whisper_outputs --output-dir ./wtf_outputs
```

## WTF Format Structure

The WTF format includes:

```json
{
  "version": "1.0",
  "transcript": "Full transcript text here...",
  "language": "en",
  "segments": [
    {
      "start": 0.0,
      "end": 2.5,
      "text": "Hello, how can I help you?",
      "speaker": "speaker_0",
      "confidence": 0.95
    }
  ],
  "words": [
    {
      "word": "Hello",
      "start": 0.0,
      "end": 0.5,
      "confidence": 0.98,
      "speaker": "speaker_0"
    }
  ],
  "speakers": [
    {
      "id": "speaker_0",
      "name": "Agent"
    }
  ],
  "metadata": {
    "duration": 125.5,
    "audio_format": "audio/wav",
    "sample_rate": 16000
  },
  "quality": {
    "overall_confidence": 0.94,
    "word_count": 245,
    "speaker_count": 2
  },
  "provider": {
    "name": "whisper",
    "model": "large-v3",
    "version": "20231114"
  }
}
```

## Cross-Provider Testing

The library includes comprehensive testing capabilities:

```bash
# Run consistency tests across providers
wtf test consistency --providers whisper,deepgram,assemblyai

# Performance benchmarking
wtf test benchmark --input audio.wav

# Quality comparison
wtf test compare --reference reference.wtf.json --test test.wtf.json
```

## Development

### Setup

```bash
# Clone repository
git clone https://github.com/vcon-dev/wtf-transcript-converter.git
cd wtf-transcript-converter

# Install with dev dependencies
uv sync --dev

# Install pre-commit hooks
pre-commit install
```

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=wtf_converter

# Run specific provider tests
pytest tests/test_whisper.py
```

### Code Quality

```bash
# Format code
black wtf_converter/ tests/

# Sort imports
isort wtf_converter/ tests/

# Lint
flake8 wtf_converter/ tests/

# Type checking
mypy wtf_converter/
```

## API Reference

### Converters

Each provider has a dedicated converter class:

- `WhisperConverter`
- `DeepgramConverter`
- `AssemblyAIConverter`
- `RevAIConverter`
- `NVIDIACanaryConverter`
- `NVIDIAParakeetConverter`
- `SpeechmaticsConverter`
- `AWSTranscribeConverter`

### Common Methods

All converters implement:

```python
class BaseConverter:
    def to_wtf(self, provider_output: dict) -> WTFTranscript:
        """Convert provider format to WTF"""

    def from_wtf(self, wtf_transcript: WTFTranscript) -> dict:
        """Convert WTF to provider format"""

    def validate(self, wtf_transcript: WTFTranscript) -> tuple[bool, list]:
        """Validate a WTF transcript"""
```

## Related Projects

- [vcon-lib](https://github.com/vcon-dev/vcon-lib) - Python vCon library
- [speechmatics-link](https://github.com/vcon-dev/speechmatics-link) - Speechmatics integration
- [vcon-server](https://github.com/vcon-dev/vcon-server) - vCon processing server

## License

MIT License
