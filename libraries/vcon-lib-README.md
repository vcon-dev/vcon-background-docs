# vCon Library (vcon-lib)

> Source: https://github.com/vcon-dev/vcon-lib

A comprehensive Python implementation for managing virtual conversation objects (vCons).

## Overview

The vCon Library provides a complete implementation of the vCon format for representing conversations and related metadata. This library implements the latest vCon specification with validation capabilities.

## Core Capabilities

### Conversation Management
- **Parties**: Manage conversation participants
- **Dialogs**: Handle conversation content (text, audio, video)
- **Attachments**: Add supplementary files and data
- **Analysis**: Store processing results (transcriptions, sentiment, summaries)

### Contact Methods Supported
- Telephone (tel:)
- Email (mailto:)
- SIP addresses
- Decentralized Identifiers (DID)

### Media Formats
- Audio
- Video
- Text
- Images

## Security and Privacy Features

### Digital Security
- Digital signatures for authenticity
- Content hashing for integrity verification

### Privacy Compliance
- **Lawful Basis Extension**: GDPR compliance support
- **WTF Extension**: World Transcription Format for standardized transcription

## Installation

```bash
pip install vcon-lib
```

## Quick Start

```python
from vcon import Vcon
from vcon.party import Party
from vcon.dialog import Dialog
from datetime import datetime, timezone

# Create a new vCon
vcon = Vcon.build_new()

# Add parties
vcon.add_party(Party(
    tel="+1234567890",
    name="Alice",
    role="caller"
))
vcon.add_party(Party(
    tel="+0987654321",
    name="Bob",
    role="callee"
))

# Add a text dialog
vcon.add_dialog(Dialog(
    type="text",
    start=datetime.now(timezone.utc).isoformat(),
    parties=[0, 1],
    originator=0,
    mimetype="text/plain",
    body="Hello, how can I help you today?"
))

# Add metadata
vcon.add_tag("customer_id", "12345")

# Validate the vCon
is_valid, errors = vcon.is_valid()

# Save to file
vcon.save_to_file("conversation.json")

# Load from file or URL
loaded_vcon = Vcon.load_from_file("conversation.json")
```

## Key Features

### Extension Framework
- Validation system
- Processing plugins
- Granular permission management

### Export Capabilities
- SRT subtitle format
- WebVTT format
- JSON export

### Transcription Converters
Convert provider-specific transcription data from:
- OpenAI Whisper
- Deepgram
- Other transcription services

## Validation and Testing

The package includes comprehensive test coverage for:
- Core functionality
- Validation rules
- Media format support
- Security features
- Backward compatibility

## Related Projects

- [vcon-server](https://github.com/vcon-dev/vcon-server) - vCon processing server
- [vcon-js](https://github.com/vcon-dev/vcon-js) - TypeScript/JavaScript library
- [pydantic-vcon](https://github.com/vcon-dev/pydantic-vcon) - Pydantic models for validation

## License

MIT License
