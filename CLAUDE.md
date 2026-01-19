# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a documentation repository for the vCon (Virtual Conversation) ecosystem. It contains:

- **IETF Internet-Drafts**: Specifications for vCon core format, extensions, and related standards
- **Developer Guides**: How to use the vCon Python library, build adapters, and create server links
- **Conserver Documentation**: Setup and configuration guides for the vCon server

## Key Documents

### IETF Drafts (`.txt` files)
- `draft-ietf-vcon-vcon-core-*.txt` - Core vCon specification (JSON format for conversation data)
- `draft-ietf-vcon-overview-*.txt` - vCon overview and use cases
- `draft-ietf-vcon-privacy-primer-*.txt` - Privacy considerations
- `draft-ietf-vcon-cc-extension-*.txt` - Call center extension
- `draft-howe-vcon-*.txt` - Individual drafts for lifecycle, lawful basis, WTF extension

### Developer Guides (`.md` files)
- `LLM_GUIDE.md` - vCon Python library API reference (Vcon, Party, Dialog classes)
- `vcon_adapter_guide.md` - Building adapters to convert data into vCon format
- `HOW_TO_CREATE_LINKS.md` - Creating processing links for vCon server
- `HOW_TO_CREATE_EXTERNAL_LINK.md` - Creating standalone link packages
- `conserver_config_guide.md` - vCon server YAML configuration reference
- `conserver-quick-start.md` / `vcon_quickstart_guide.md` - Server setup guides

## vCon Data Model

A vCon contains:
- **parties**: Conversation participants (tel, name, role, mailto)
- **dialog**: Messages/recordings with type, start time, mimetype, body
- **analysis**: Processing results (transcriptions, sentiment, summaries)
- **attachments**: Additional files/data
- **tags**: Key-value metadata pairs

## vCon Python Library Patterns

```python
from vcon import Vcon
from vcon.party import Party
from vcon.dialog import Dialog

# Create vCon
vcon = Vcon.build_new()

# Add participants
vcon.add_party(Party(tel="+1234567890", name="Alice", role="caller"))

# Add dialog
vcon.add_dialog(Dialog(
    type="text",
    start=datetime.now(timezone.utc).isoformat(),
    parties=[0, 1],
    originator=0,
    mimetype="text/plain",
    body="Hello"
))

# Add metadata and analysis
vcon.add_tag("customer_id", "12345")
vcon.add_analysis(type="summary", dialog=0, vendor="openai", body={...}, encoding="json")

# Validate and save
is_valid, errors = vcon.is_valid()
vcon.save_to_file("conversation.json")
```

## vCon Server Link Interface

Every link must implement:

```python
def run(vcon_uuid: str, link_name: str, opts: dict = default_options) -> Optional[str]:
    """
    Returns vcon_uuid to continue chain, None to stop.
    """
    vcon_redis = VconRedis()
    vcon = vcon_redis.get_vcon(vcon_uuid)
    # Process vCon...
    vcon_redis.store_vcon(vcon)
    return vcon_uuid
```

## Server Configuration Structure

```yaml
links:
  link_name:
    module: links.module_name
    pip_name: package-name  # optional, for external packages
    options: {}

storages:
  storage_name:
    module: storage.backend
    options: {}

chains:
  chain_name:
    links: [link1, link2]
    storages: [storage1]
    ingress_lists: [input_queue]
    egress_lists: [output_queue]
    enabled: 1
```

## Ecosystem Documentation

This repository includes documentation from key vcon-dev repositories organized by category:

### Libraries
- `libraries/vcon-lib-README.md` - Python vCon library (Party, Dialog, Vcon classes)
- `libraries/vcon-js-README.md` - TypeScript/JavaScript vCon library

### Adapters
- `adapters/vcon-audio-adapter-README.md` - Convert audio files to vCon format
- `adapters/vcon-siprec-adapter-README.md` - SIPREC SRS to vCon conversion

### Processing Links
- `links/vcon-sample-link-README.md` - Template for creating external links
- `links/speechmatics-link-README.md` - Speechmatics transcription with WTF format

### Transcription
- `transcription/wtf-transcript-converter-README.md` - World Transcription Format conversion

### AI Integration
- `ai-integration/vcon-mcp-README.md` - MCP server for AI assistant integration

### Examples & Testing
- `examples/fake-vcons-README.md` - Synthetic vCon files for testing

### Resources
- `resources/awesome-vcon.md` - Curated list of vCon resources, tools, and adopters

## External Resources

- vCon Server: https://github.com/vcon-dev/vcon-server
- vCon Admin: https://github.com/vcon-dev/vcon-admin
- IETF vCon Specification: https://datatracker.ietf.org/doc/draft-ietf-vcon-vcon/
- Awesome vCon: https://github.com/vcon-dev/awesome-vcon
- vCon Open Forum: https://vcons.org/
