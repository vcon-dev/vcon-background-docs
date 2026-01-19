# vCon Background Documentation

A comprehensive documentation repository for the vCon (Virtual Conversation) ecosystem. This repository serves as a reference for AI assistants and developers building tools with vCon.

## What is vCon?

vCon is a JSON-based, signed/encrypted container for complete conversations—participants, media, analysis, and attachments. Think of it as "PDF for conversations" - a standardized format that works across communication platforms.

## Repository Structure

```
background-docs/
├── CLAUDE.md              # AI assistant guidance
├── README.md              # This file
├── adapters/              # Data source adapters
├── ai-integration/        # AI/MCP integration
├── examples/              # Sample data & testing
├── libraries/             # SDK documentation
├── links/                 # Processing link guides
├── resources/             # Curated resources
└── transcription/         # Transcription tools
```

## Documentation by Category

### Libraries

Core libraries for working with vCon in different languages:

| Document | Description |
|----------|-------------|
| [vcon-lib-README.md](libraries/vcon-lib-README.md) | Python vCon library - Party, Dialog, Vcon classes, validation |
| [vcon-js-README.md](libraries/vcon-js-README.md) | TypeScript/JavaScript library with full vcon-core-01 compliance |

### Adapters

Convert data from various sources into vCon format:

| Document | Description |
|----------|-------------|
| [vcon-audio-adapter-README.md](adapters/vcon-audio-adapter-README.md) | Monitor directories for audio files, extract metadata, create vCons |
| [vcon-siprec-adapter-README.md](adapters/vcon-siprec-adapter-README.md) | SIPREC SRS server that converts recordings to vCon |

### Processing Links

Extend vCon server with custom processing:

| Document | Description |
|----------|-------------|
| [vcon-sample-link-README.md](links/vcon-sample-link-README.md) | Template for creating external processing links |
| [speechmatics-link-README.md](links/speechmatics-link-README.md) | Speechmatics transcription with WTF format output |

### Transcription

Tools for working with transcription data:

| Document | Description |
|----------|-------------|
| [wtf-transcript-converter-README.md](transcription/wtf-transcript-converter-README.md) | Convert between provider formats and standardized WTF |

### AI Integration

Integrate vCon with AI assistants and tools:

| Document | Description |
|----------|-------------|
| [vcon-mcp-README.md](ai-integration/vcon-mcp-README.md) | MCP server enabling AI assistants to work with vCon data |

### Examples & Testing

Sample data and testing resources:

| Document | Description |
|----------|-------------|
| [fake-vcons-README.md](examples/fake-vcons-README.md) | Synthetic vCon files for testing and development |

### Resources

Curated lists and external references:

| Document | Description |
|----------|-------------|
| [awesome-vcon.md](resources/awesome-vcon.md) | Comprehensive list of vCon specs, tools, adopters, and community |

## Quick Reference

### vCon Data Model

A vCon contains:
- **parties**: Conversation participants (tel, name, role, mailto)
- **dialog**: Messages/recordings with type, start time, mimetype, body
- **analysis**: Processing results (transcriptions, sentiment, summaries)
- **attachments**: Additional files/data
- **tags**: Key-value metadata pairs

### Python Quick Start

```python
from vcon import Vcon
from vcon.party import Party
from vcon.dialog import Dialog
from datetime import datetime, timezone

# Create vCon
vcon = Vcon.build_new()

# Add parties
vcon.add_party(Party(tel="+1234567890", name="Alice", role="caller"))
vcon.add_party(Party(tel="+0987654321", name="Bob", role="callee"))

# Add dialog
vcon.add_dialog(Dialog(
    type="text",
    start=datetime.now(timezone.utc).isoformat(),
    parties=[0, 1],
    originator=0,
    mimetype="text/plain",
    body="Hello, how can I help you?"
))

# Save
vcon.save_to_file("conversation.json")
```

### TypeScript Quick Start

```typescript
import { Vcon, Party, Dialog } from 'vcon-js';

const vcon = Vcon.buildNew();

vcon.addParty(new Party({ tel: '+1234567890', name: 'Alice', role: 'agent' }));
vcon.addParty(new Party({ tel: '+0987654321', name: 'Bob', role: 'customer' }));

vcon.addDialog(new Dialog({
  type: 'text',
  start: new Date(),
  parties: [0, 1],
  body: 'Hello!',
  mediatype: 'text/plain'
}));

const json = vcon.toJson();
```

### Server Link Interface

```python
def run(vcon_uuid: str, link_name: str, opts: dict = default_options) -> str | None:
    """Process a vCon. Return uuid to continue chain, None to stop."""
    vcon_redis = VconRedis()
    vcon = vcon_redis.get_vcon(vcon_uuid)
    # Process vCon...
    vcon_redis.store_vcon(vcon)
    return vcon_uuid
```

## External Links

- **IETF vCon Working Group**: https://datatracker.ietf.org/group/vcon/documents/
- **vCon Server**: https://github.com/vcon-dev/vcon-server
- **vCon Admin**: https://github.com/vcon-dev/vcon-admin
- **Documentation Hub**: https://docs.vcons.org/
- **Community Forum**: https://vcons.org/

## Contributing

This repository aggregates documentation from the vcon-dev ecosystem. For updates to individual projects, please contribute to the source repositories.

## License

MIT License
