# ElevenLabs CLI

> **Unofficial CLI**: This is an independent, community-built CLI client. It is not officially released by ElevenLabs.

A comprehensive command-line interface for the ElevenLabs AI audio platform with 100% SDK coverage. Generate speech, transcribe audio, clone voices, and more.

## Installation

### Homebrew (macOS/Linux)

```bash
brew tap hongkongkiwi/tap
brew install elevenlabs-cli
```

### Scoop (Windows)

```powershell
scoop bucket add elevenlabs-cli https://github.com/hongkongkiwi/scoop-elevenlabs-cli
scoop install elevenlabs-cli
```

### Snap (Linux)

```bash
sudo snap install elevenlabs-cli
```

### Cargo (All Platforms)

```bash
cargo install elevenlabs-cli
```

### Docker

```bash
docker pull ghcr.io/hongkongkiwi/elevenlabs-cli:latest
docker run --rm -e ELEVENLABS_API_KEY=your-key ghcr.io/hongkongkiwi/elevenlabs-cli tts "Hello!"
```

## Prerequisites

- **ElevenLabs API key** - Get one free at [ElevenLabs API Keys](https://elevenlabs.io/app/settings/api-keys)

## Configuration

```bash
# Set API key via environment variable
export ELEVENLABS_API_KEY="your-api-key"

# Or save to config file (~/.config/elevenlabs-cli/config.toml)
elevenlabs config set api_key your-api-key

# Set default voice and model
elevenlabs config set default_voice Brian
elevenlabs config set default_model eleven_multilingual_v2
```

## Commands Reference

### Text-to-Speech (`tts`)

Convert text to natural speech with 100+ voices.

```bash
# Basic usage - speaks text and saves to file
elevenlabs tts "Hello, world!"

# Specify voice and model
elevenlabs tts "Hello!" --voice Rachel --model eleven_v3

# Save to specific file
elevenlabs tts "Hello!" --output speech.mp3

# Read text from file
elevenlabs tts --file script.txt --output audiobook.mp3

# Adjust voice settings
elevenlabs tts "Hello!" --stability 0.5 --similarity-boost 0.75 --style 0.3

# Play audio after generation (requires audio feature)
elevenlabs tts "Hello!" --play

# Specify output device for playback
elevenlabs tts "Hello!" --play --output-device "Speakers"

# Specify language for multilingual models
elevenlabs tts "Bonjour!" --language fr

# Use seed for reproducible output
elevenlabs tts "Hello!" --seed 12345
```

**Available voice settings:**
- `--stability` (0.0-1.0): Higher = more consistent, lower = more expressive
- `--similarity-boost` (0.0-1.0): Higher = more similar to original voice
- `--style` (0.0-1.0): Higher = more stylized delivery
- `--speaker-boost`: Enhance speaker clarity

### Speech-to-Text (`stt`)

Transcribe audio with speaker diarization and timestamps.

```bash
# Transcribe audio file
elevenlabs stt audio.mp3

# Specify model
elevenlabs stt audio.mp3 --model scribe_v1

# Enable speaker diarization (identify speakers)
elevenlabs stt meeting.mp3 --diarize --num-speakers 3

# Output as subtitles
elevenlabs stt video.mp3 --format srt --output subtitles.srt
elevenlabs stt video.mp3 --format vtt --output subtitles.vtt

# With word-level timestamps
elevenlabs stt audio.mp3 --timestamps word

# With character-level timestamps
elevenlabs stt audio.mp3 --timestamps character

# Specify language (auto-detected by default)
elevenlabs stt audio.mp3 --language en

# Record from microphone and transcribe
elevenlabs stt --record --duration 10

# List available input devices
elevenlabs stt --list-input-devices
```

### Voice Management (`voice`)

Manage voices including cloning and settings.

```bash
# List all voices
elevenlabs voice list

# List with detailed info
elevenlabs voice list --detailed

# Get voice details
elevenlabs voice get <voice-id>

# Clone a voice from audio samples
elevenlabs voice clone --name "My Voice" --samples sample1.mp3,sample2.mp3

# Clone from directory of samples
elevenlabs voice clone --name "My Voice" --samples-dir ./samples/

# Clone with description and labels
elevenlabs voice clone --name "My Voice" --samples audio.mp3 --description "Professional voice" --labels accent=american

# Get voice settings
elevenlabs voice settings <voice-id>

# Edit voice settings
elevenlabs voice edit-settings <voice-id> --stability 0.6 --similarity-boost 0.8

# Edit voice name or description
elevenlabs voice edit <voice-id> --name "New Name" --description "New description"

# Delete a voice
elevenlabs voice delete <voice-id>

# Share a voice publicly
elevenlabs voice share <voice-id>

# Find similar voices
elevenlabs voice similar --voice-id <voice-id>
elevenlabs voice similar --text "deep male voice with british accent"

# Fine-tune a voice
elevenlabs voice fine-tune start <voice-id> --name "Fine-tuned"
elevenlabs voice fine-tune status <voice-id>
elevenlabs voice fine-tune cancel <voice-id>
```

### Sound Effects (`sfx`)

Generate sound effects from text descriptions.

```bash
# Generate sound effect
elevenlabs sfx "door creaking slowly in a haunted house"

# Specify duration (0.5-22 seconds)
elevenlabs sfx "thunder rumbling" --duration 10

# Adjust prompt influence (0-1, default 0.3)
elevenlabs sfx "rain falling" --influence 0.5

# Save to file
elevenlabs sfx "car engine revving" --output engine.mp3
```

### Audio Isolation (`isolate`)

Remove background noise from audio.

```bash
# Remove background noise
elevenlabs isolate noisy_audio.mp3 --output clean_audio.mp3

# Process and overwrite
elevenlabs isolate recording.mp3
```

### Voice Changer (`voice-changer`)

Transform voice in audio files to a different voice.

```bash
# Transform voice in audio file
elevenlabs voice-changer input.mp3 --voice Rachel --output transformed.mp3

# Specify model
elevenlabs voice-changer input.mp3 --voice Brian --model eleven_multilingual_sts_v2

# Record from microphone and transform
elevenlabs voice-changer --record --duration 10 --voice Rachel --output output.mp3

# Play after processing
elevenlabs voice-changer input.mp3 --voice Rachel --play

# With voice settings
elevenlabs voice-changer input.mp3 --voice Rachel --stability 0.5 --similarity-boost 0.8
```

### Dubbing (`dub`)

Translate and dub video/audio to other languages.

```bash
# Create dubbing project
elevenlabs dub create --file video.mp4 --source-lang en --target-lang es

# With multiple speakers
elevenlabs dub create --file interview.mp4 --source-lang en --target-lang fr --num-speakers 2

# Check dubbing status
elevenlabs dub status <dubbing-id>

# Download dubbed file
elevenlabs dub download <dubbing-id> --output dubbed.mp4

# Delete dubbing project
elevenlabs dub delete <dubbing-id>
```

### Dialogue (`dialogue`)

Generate multi-voice dialogues.

```bash
# Create dialogue with multiple voices
elevenlabs dialogue --inputs "Hello there!:Brian,Hi! How are you?:Rachel,I'm great thanks!:Brian"

# Specify model
elevenlabs dialogue --inputs "text1:voice1,text2:voice2" --model eleven_v3

# Save to file
elevenlabs dialogue --inputs "..." --output dialogue.mp3

# Specify output format
elevenlabs dialogue --inputs "..." --output-format wav_44100
```

### Agents (`agent`)

Manage conversational AI agents.

```bash
# List agents
elevenlabs agent list

# Get agent summaries (lightweight)
elevenlabs agent summaries

# Get agent details
elevenlabs agent get <agent-id>

# Create agent
elevenlabs agent create --name "Support Bot" --voice-id <voice-id> --first-message "Hello!"

# Create with system prompt
elevenlabs agent create --name "Assistant" --system-prompt "You are a helpful assistant"

# Update agent
elevenlabs agent update <agent-id> --name "New Name"

# Delete agent
elevenlabs agent delete <agent-id>

# Get agent public link
elevenlabs agent link <agent-id>

# Duplicate agent
elevenlabs agent duplicate <agent-id> --name "Copy of Agent"

# List agent branches
elevenlabs agent branches <agent-id>

# Rename branch
elevenlabs agent rename-branch <agent-id> <branch-id> --name "production"

# Batch operations
elevenlabs agent batch-list
elevenlabs agent batch-status <batch-id>
elevenlabs agent batch-delete <batch-id>

# Simulate conversation (for testing)
elevenlabs agent simulate <agent-id> --message "Hello" --max-turns 5

# Update turn configuration
elevenlabs agent update-turn <agent-id> --spelling-patience high --silence-threshold-ms 500

# Widget configuration
elevenlabs agent widget-get <agent-id>
elevenlabs agent widget-avatar <agent-id> --avatar-file avatar.png

# WhatsApp integration
elevenlabs agent whatsapp-list
```

### Conversations (`converse`)

Have real-time conversations with agents.

```bash
# Start conversation with agent
elevenlabs converse --agent-id <agent-id>

# List conversations
elevenlabs converse list

# Get conversation details
elevenlabs converse get <conversation-id>

# Get conversation audio
elevenlabs converse audio <conversation-id> --output conversation.mp3

# Delete conversation
elevenlabs converse delete <conversation-id>

# Get signed WebSocket URL
elevenlabs converse signed-url <agent-id>
```

### Knowledge Base (`knowledge`)

Manage documents for RAG (Retrieval-Augmented Generation).

```bash
# List documents
elevenlabs knowledge list

# Add document from URL
elevenlabs knowledge add-from-url --url https://example.com/doc --name "Documentation"

# Add document from text
elevenlabs knowledge add-from-text --text "Content here" --name "My Doc"

# Add document from file
elevenlabs knowledge add-from-file --file document.pdf --name "PDF Doc"

# Get document details
elevenlabs knowledge get <document-id>

# Delete document
elevenlabs knowledge delete <document-id>
```

### RAG Index (`rag`)

Manage RAG indexes for knowledge retrieval.

```bash
# Create RAG index from documents
elevenlabs rag create --document-ids <doc1>,<doc2>

# Get RAG status
elevenlabs rag status <rag-id>

# Rebuild RAG index
elevenlabs rag rebuild <rag-id>

# Get RAG index status
elevenlabs rag index-status <rag-id>

# Delete RAG index
elevenlabs rag delete <rag-id>
```

### History (`history`)

View and manage generation history.

```bash
# List generation history
elevenlabs history list

# List with limit
elevenlabs history list --limit 50

# Show detailed info
elevenlabs history list --detailed

# Get history item details
elevenlabs history get <history-item-id>

# Download audio from history
elevenlabs history download <history-item-id> --output audio.mp3

# Delete history item
elevenlabs history delete <history-item-id>

# Submit feedback
elevenlabs history feedback <history-item-id> --thumbs-up
elevenlabs history feedback <history-item-id> --feedback "Great quality!"
```

### Voice Library (`library`)

Browse and use shared voices from the ElevenLabs library.

```bash
# List library voices
elevenlabs library list

# List collections
elevenlabs library collections

# Get voice details
elevenlabs library get <voice-id>
```

### Voice Design (`voice-design`)

Create new voices from text descriptions.

```bash
# Create voice from text description
elevenlabs voice-design create --text "A warm, professional female voice with a slight British accent"

# Get voice design status
elevenlabs voice-design status <design-id>
```

### Samples (`samples`)

Manage voice samples.

```bash
# List samples for a voice
elevenlabs samples list <voice-id>

# Delete a sample
elevenlabs samples delete <sample-id>
```

### Pronunciation (`pronunciation`)

Manage pronunciation dictionaries.

```bash
# List pronunciation dictionaries
elevenlabs pronunciation list

# Add pronunciation rule
elevenlabs pronunciation add --dictionary-id <id> --text "ElevenLabs" --phoneme "ɪˈlɛvənleɪbz"

# Add multiple rules from file
elevenlabs pronunciation add-rules --dictionary-id <id> --file rules.csv
```

### Projects (`projects`)

Manage long-form content projects.

```bash
# List projects
elevenlabs projects list

# Get project details
elevenlabs projects get <project-id>

# Delete project
elevenlabs projects delete <project-id>

# Convert project to audio
elevenlabs projects convert <project-id>

# List project snapshots
elevenlabs projects snapshots <project-id>

# Get project audio
elevenlabs projects audio <project-id> --output project.mp3
```

### Music (`music`)

Generate AI music.

```bash
# Generate music
elevenlabs music generate --prompt "Upbeat electronic dance track" --duration 30

# List generated music
elevenlabs music list

# Get music details
elevenlabs music get <music-id>

# Download music
elevenlabs music download <music-id> --output music.mp3

# Delete music
elevenlabs music delete <music-id>
```

### Phone (`phone`)

Manage phone numbers for voice calls.

```bash
# List phone numbers
elevenlabs phone list

# Get phone details
elevenlabs phone get <phone-id>

# Import phone number
elevenlabs phone import --phone-number +1234567890

# Update phone settings
elevenlabs phone update <phone-id> --agent-id <agent-id>

# Delete phone number
elevenlabs phone delete <phone-id>

# Test phone call
elevenlabs phone test <phone-id>
```

### Tools (`tools`)

Manage agent tools.

```bash
# List tools
elevenlabs tools list

# Get tool details
elevenlabs tools get <tool-id>

# Delete tool
elevenlabs tools delete <tool-id>
```

### Webhooks (`webhook`)

Manage webhooks for event notifications.

```bash
# List webhooks
elevenlabs webhook list

# Create webhook
elevenlabs webhook create --name "My Webhook" --url https://example.com/webhook --events voice.created,voice.deleted

# Delete webhook
elevenlabs webhook delete <webhook-id>
```

### Workspace (`workspace`)

Manage workspace settings and members.

```bash
# Get workspace info
elevenlabs workspace info

# List members
elevenlabs workspace members

# Remove member
elevenlabs workspace remove-member <user-id>

# List pending invites
elevenlabs workspace invites

# Invite member
elevenlabs workspace invite user@example.com --role editor

# Revoke invite
elevenlabs workspace revoke user@example.com

# List API keys
elevenlabs workspace api-keys

# List secrets
elevenlabs workspace secrets

# Add secret
elevenlabs workspace add-secret SECRET_NAME secret_value api_key

# Delete secret
elevenlabs workspace delete-secret SECRET_NAME

# Share resource
elevenlabs workspace share agent <agent-id> viewer

# Unshare resource
elevenlabs workspace unshare agent <agent-id>
```

### Audio Native (`audio-native`)

Manage embeddable audio players.

```bash
# List audio native configurations
elevenlabs audio-native list

# Get configuration
elevenlabs audio-native get <config-id>

# Create audio native
elevenlabs audio-native create --voice-id <voice-id>
```

### User & Usage

```bash
# Get user info
elevenlabs user info

# Get subscription details
elevenlabs user subscription

# Get usage statistics
elevenlabs usage stats

# Usage with date range
elevenlabs usage stats --start 1704067200 --end 1706745600

# Usage with breakdown
elevenlabs usage stats --breakdown voice
```

### Models

```bash
# List available models
elevenlabs models list

# Get model rates
elevenlabs models rates
```

### TTS with Timestamps

Generate speech with character-level timing data.

```bash
# Generate with timestamps
elevenlabs tts-timestamps "Hello, world!" --voice Brian --output speech.mp3

# Generate subtitles
elevenlabs tts-timestamps "Hello!" --subtitles subtitles.srt

# Specify output format
elevenlabs tts-timestamps "Hello!" --output-format wav_44100
```

### Streaming TTS

Stream audio generation with real-time output.

```bash
# Stream TTS
elevenlabs tts-stream "Hello, world!" --voice Brian

# Stream to file
elevenlabs tts-stream "Long text..." --output stream.mp3

# Stream and play
elevenlabs tts-stream "Hello!" --play

# With latency optimization (0-4, higher = faster)
elevenlabs tts-stream "Hello!" --latency 4
```

### Real-time TTS

Ultra-low latency WebSocket-based TTS.

```bash
# Real-time TTS
elevenlabs realtime-tts "Hello!" --voice Brian

# Save to file
elevenlabs realtime-tts "Hello!" --output realtime.mp3

# Play in real-time
elevenlabs realtime-tts "Hello!" --play
```

### Configuration (`config`)

Manage CLI configuration.

```bash
# Set config value
elevenlabs config set api_key your-key
elevenlabs config set default_voice Brian
elevenlabs config set default_model eleven_multilingual_v2
elevenlabs config set default_output_format mp3_44100_128

# Get config value
elevenlabs config get api_key

# List all config
elevenlabs config list

# Clear config
elevenlabs config clear
```

### Utilities

```bash
# Generate shell completions
elevenlabs completions bash > ~/.bash_completion.d/elevenlabs
elevenlabs completions zsh > "${fpath[1]}/_elevenlabs"
elevenlabs completions fish > ~/.config/fish/completions/elevenlabs.fish
elevenlabs completions powershell

# Update CLI to latest version
elevenlabs update

# Interactive mode (REPL)
elevenlabs interactive
```

### Global Options

```bash
# JSON output
elevenlabs --json voice list

# Quiet mode
elevenlabs -q tts "Hello" --output test.mp3

# Specify API key
elevenlabs --api-key your-key tts "Hello"

# Specify API base URL (for custom endpoints)
elevenlabs --base-url https://api.elevenlabs.io tts "Hello"

# Disable SSL verification (not recommended)
elevenlabs --insecure tts "Hello"
```

## Output Formats

Supported audio formats for TTS output:

| Format | Description |
|--------|-------------|
| `mp3_44100_128` | MP3 44.1kHz 128kbps (default) |
| `mp3_44100_192` | MP3 44.1kHz 192kbps |
| `pcm_16000` | PCM 16kHz (raw) |
| `pcm_22050` | PCM 22.05kHz (raw) |
| `pcm_44100` | PCM 44.1kHz (raw) |
| `wav_8000` | WAV 8kHz |
| `wav_16000` | WAV 16kHz |
| `wav_44100` | WAV 44.1kHz |
| `opus_48000_64` | Opus 48kHz 64kbps |
| `opus_48000_128` | Opus 48kHz 128kbps |
| `ulaw_8000` | μ-law 8kHz (telephony) |

## Models

### TTS Models

| Model | Description |
|-------|-------------|
| `eleven_multilingual_v2` | Best quality, 29 languages (default) |
| `eleven_flash_v2_5` | Lowest latency |
| `eleven_turbo_v2_5` | Balanced quality & speed |
| `eleven_v3` | Expressive, emotional speech |

### STT Models

| Model | Description |
|-------|-------------|
| `scribe_v1` | High accuracy transcription (default) |
| `scribe_v1_base` | Faster, lower cost |

## Related Resources

- **Main Repository**: [hongkongkiwi/elevenlabs-cli](https://github.com/hongkongkiwi/elevenlabs-cli)
- **Homebrew Tap**: [hongkongkiwi/homebrew-elevenlabs-cli](https://github.com/hongkongkiwi/homebrew-elevenlabs-cli)
- **Scoop Bucket**: [hongkongkiwi/scoop-elevenlabs-cli](https://github.com/hongkongkiwi/scoop-elevenlabs-cli)
- **ElevenLabs API Docs**: https://elevenlabs.io/docs/api-reference
- **API Keys**: https://elevenlabs.io/app/settings/api-keys

## Tags

elevenlabs, tts, text-to-speech, stt, speech-to-text, audio, voice, voice-cloning, voice-synthesis, ai, cli, command-line
