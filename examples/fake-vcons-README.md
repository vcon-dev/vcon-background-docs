# Synthetic Virtual Conversation Files (vCon)

> Source: https://github.com/vcon-dev/fake-vcons

This repository contains synthetic virtual conversation files (vCon) created using the [vcon_faker](https://github.com/vcon-dev/vcon_faker) tool. These files simulate interactions between a customer service agent and a customer across various business scenarios.

## About vcon_faker

The vcon_faker tool is a powerful Python-based utility that generates realistic synthetic conversations for testing and development purposes. It leverages AI models to create natural-sounding dialogues between agents and customers, complete with appropriate metadata and context.

## Structure of a vCon (Virtual Conversation)

A vCon (Virtual Conversation) is a standardized JSON-based container designed to store and exchange data about real-time human conversations. It can encapsulate information from various communication modes, such as phone calls, video conferences, SMS, MMS, emails, and more.

### Key Components of a vCon

A vCon contains five main sections:

1. **Metadata**: Details about the conversation's context, including unique identifiers, timestamps, subject, and references to previous versions
2. **Parties**: Details about participants including their roles, identifiers, and contact information
3. **Dialog**: The actual content of the conversation (text, audio, or video)
4. **Analysis**: Derived data such as transcripts, translations, sentiment analysis, or semantic tagging
5. **Attachments**: Additional files related to the conversation (slides, images, documents)

### Structure of a vCon JSON Object

A vCon JSON object can be in one of three forms:
- **Unsigned**: Initial or intermediate state during data collection
- **Signed**: Verified state with a digital signature for immutability
- **Encrypted**: Secure state to protect sensitive data

#### General Structure

```json
{
  "vcon": "0.0.1",
  "uuid": "string",
  "created_at": "date",
  "updated_at": "date",
  "subject": "string",
  "redacted": {},
  "appended": {},
  "group": [],
  "parties": [],
  "dialog": [],
  "analysis": [],
  "attachments": []
}
```

#### Parties

Each participant in the conversation:

```json
{
  "tel": "string",
  "mailto": "string",
  "name": "string",
  "role": "string",
  "uuid": "string"
}
```

#### Dialog

Each segment of the conversation:

```json
{
  "type": "string",
  "start": "date",
  "duration": "number",
  "parties": [],
  "mimetype": "string",
  "filename": "string",
  "body": "string",
  "encoding": "string"
}
```

#### Analysis

Derived insights about the conversation:

```json
{
  "type": "string",
  "dialog": [],
  "mimetype": "string",
  "filename": "string",
  "vendor": "string",
  "body": "string",
  "encoding": "string"
}
```

#### Attachments

Supplemental data:

```json
{
  "type": "string",
  "start": "date",
  "party": "number",
  "mimetype": "string",
  "filename": "string",
  "body": "string",
  "encoding": "string"
}
```

### Security and Integrity

- **Signing**: vCons can be signed using JWS (JSON Web Signature) for integrity and authenticity
- **Encryption**: Sensitive vCons can be encrypted using JWE (JSON Web Encryption)
- **Versioning**: Redacted and appended vCons reference their original versions

## Example Conversation Format

The conversations are formatted as an array of message objects:

```json
{
  "conversation": [
    {"speaker": "Agent", "message": "Hello! My name is John Doe from Car Rentals. How can I assist you today?"},
    {"speaker": "Customer", "message": "Hi, I need to inquire about my recent rental."},
    {"speaker": "Agent", "message": "Certainly! Could I have your booking reference number, please?"}
  ]
}
```

## Usage

These synthetic vCon files can be used for:

- Training customer service agents
- Demonstrating customer interaction model capabilities
- Testing conversational AI systems
- Development and testing of vCon processing applications
- Creating realistic test datasets for machine learning models

## Getting Started with vcon_faker

To generate your own synthetic conversations:

1. Clone the vcon_faker repository:
   ```bash
   git clone https://github.com/vcon-dev/vcon_faker.git
   ```

2. Follow the installation and setup instructions in the [vcon_faker documentation](https://github.com/vcon-dev/vcon_faker#readme)

3. Use the tool to generate synthetic conversations with your desired parameters and scenarios

## Example Use Cases

- **Customer Support**: Storing call recordings, transcripts, and attachments for quality assurance and analytics
- **Legal Compliance**: Maintaining immutable records of conversations with signatures for regulatory purposes
- **Machine Learning**: Using vCon data as input for training AI models while adhering to data privacy laws

## Related Projects

- [vcon_faker](https://github.com/vcon-dev/vcon_faker) - Generate fake vCons
- [vcon-lib](https://github.com/vcon-dev/vcon-lib) - Python vCon library
- [vcon-server](https://github.com/vcon-dev/vcon-server) - vCon processing server

## License

MIT License
