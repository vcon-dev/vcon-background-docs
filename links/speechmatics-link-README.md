# Speechmatics vCon Link

> Source: https://github.com/vcon-dev/speechmatics-link

A vCon server link for transcribing audio dialogs using the Speechmatics API. This tool processes vCon objects and stores transcription results in both Speechmatics native format and the standardized World Transcription Format (WTF).

## Key Features

- **Batch Transcription**: Process audio dialogs via Speechmatics API
- **Dual Output Formats**: Store results in both Speechmatics native and WTF formats
- **Speaker Diarization**: Identify and label different speakers
- **Automatic Language Detection**: Detect the language of audio content
- **Idempotent Processing**: Safe to run multiple times without duplicating work
- **Retry Logic**: Exponential backoff for API reliability
- **Comprehensive Logging**: Detailed logging for debugging and monitoring

## Installation

### From GitHub

```bash
pip install git+https://github.com/vcon-dev/speechmatics-link.git
```

### Development Setup

```bash
git clone https://github.com/vcon-dev/speechmatics-link.git
cd speechmatics-link
pip install -e ".[dev]"
```

## Configuration

Configure the link in your vcon-server chain configuration:

```yaml
links:
  speechmatics_transcribe:
    module: speechmatics_link
    pip_name: git+https://github.com/vcon-dev/speechmatics-link.git
    options:
      api_key: ${SPEECHMATICS_API_KEY}
      output_format: "both"  # "speechmatics", "wtf", or "both"
      model: "enhanced"
      diarization: true
      language: "auto"  # or specific language code like "en"

chains:
  transcription_chain:
    links:
      - speechmatics_transcribe
    ingress_lists:
      - audio_ingress
    storages:
      - redis_storage
    enabled: 1
```

### Configuration Options

| Option | Description | Default |
|--------|-------------|---------|
| `api_key` | Speechmatics API key | Required |
| `output_format` | Output format: "speechmatics", "wtf", or "both" | "both" |
| `model` | Transcription model to use | "enhanced" |
| `diarization` | Enable speaker diarization | true |
| `language` | Language code or "auto" for detection | "auto" |

## Output Formats

### Speechmatics Native Format

Contains complete API responses including:
- Word-level timing
- Confidence scores
- Speaker labels
- Job metadata

### WTF (World Transcription Format)

Implements the standardized schema with:
- Transcript text
- Language information
- Segments with timing
- Words with confidence
- Speakers
- Metadata
- Quality metrics
- Provider-specific extensions

## Usage Example

The link processes vCons automatically when configured in a chain. For testing:

```python
from speechmatics_link import run

result = run(
    vcon_uuid="123e4567-e89b-12d3-a456-426614174000",
    link_name="speechmatics_transcribe",
    opts={
        "api_key": "your-api-key",
        "output_format": "both",
        "diarization": True
    }
)
```

## Development & Testing

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=speechmatics_link

# Run specific test
pytest tests/test_transcription.py
```

### Code Formatting

```bash
black speechmatics_link/ tests/
```

### CI/CD

The project uses GitHub Actions for:
- Unit tests across Python 3.12 and 3.13
- Code formatting validation
- Optional integration tests (requires API credentials)

## Related Projects

- [vcon-server](https://github.com/vcon-dev/vcon-server) - vCon processing server
- [wtf-transcript-converter](https://github.com/vcon-dev/wtf-transcript-converter) - WTF format conversion
- [vcon-sample-link](https://github.com/vcon-dev/vcon-sample-link) - Template for external links

## License

MIT License
