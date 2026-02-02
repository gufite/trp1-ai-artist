# AI Content Codebase Architecture

## Overview
This is a multi-provider AI content generation framework that supports music, video, and image generation through a unified CLI interface and programmatic API.

## Directory Structure

```
src/ai_content/
├── cli/              # Command-line interface
│   └── main.py       # Typer-based CLI commands
├── config/           # Configuration management
│   ├── loader.py     # YAML config loading
│   └── settings.py   # Environment variable settings
├── core/             # Core framework components
│   ├── exceptions.py # Custom exceptions
│   ├── job_tracker.py # Job tracking system
│   ├── provider.py   # Provider base class
│   ├── registry.py   # Provider registration system
│   └── result.py     # Result data structures
├── integrations/     # External service integrations
│   ├── archive.py    # Internet Archive integration
│   ├── media.py      # FFmpeg media processing
│   └── youtube.py    # YouTube upload capabilities
├── pipelines/        # Multi-step generation workflows
│   ├── base.py       # Pipeline base class
│   ├── full.py       # Complete music video pipeline
│   ├── music.py      # Music generation pipeline
│   └── video.py      # Video generation pipeline
├── presets/          # Pre-configured style templates
│   ├── music.py      # Music style presets (11 presets)
│   └── video.py      # Video style presets (7 presets)
├── providers/        # AI service provider implementations
│   ├── google/       # Google Gemini providers
│   │   ├── imagen.py # Image generation (Imagen 3)
│   │   ├── lyria.py  # Music generation (Lyria)
│   │   └── veo.py    # Video generation (Veo 2)
│   ├── aimlapi/      # AIMLAPI providers
│   │   ├── client.py # API client
│   │   └── minimax.py # MiniMax music with vocals
│   └── kling/        # KlingAI providers
│       └── direct.py # Kling video generation
└── utils/            # Utility functions
    ├── file_handlers.py # File I/O operations
    ├── lyrics_parser.py # Lyrics parsing
    └── retry.py         # Retry logic with exponential backoff
```

## Key Architecture Patterns

### 1. Provider Registry Pattern
- All providers inherit from `BaseProvider` class
- Providers self-register using the `@registry.register()` decorator
- Registry manages provider discovery and instantiation
- Supports multiple providers for same content type (e.g., lyria vs minimax for music)

### 2. Pipeline Orchestration
- Pipelines compose multiple generation steps
- Base pipeline provides common workflow (validate → generate → postprocess)
- Specialized pipelines for music, video, and full music video creation
- Pipelines can chain provider outputs (e.g., music → video → combine)

### 3. Preset System
- Presets are frozen dataclasses with prompt templates
- Music presets include: prompt, BPM, mood, tags
- Video presets include: prompt, aspect ratio, duration, style keywords
- Presets optimize for consistent, high-quality results

### 4. Configuration Management
- Environment variables loaded from `.env` file
- YAML config files for provider-specific settings
- Settings class validates required API keys
- Supports provider-specific configuration overrides

### 5. Job Tracking
- Tracks generation jobs with status (pending, processing, completed, failed)
- Persists job history to local database
- Supports async API providers (e.g., MiniMax)
- Can sync job status and download results later

## Data Flow

```
User Input (CLI/API)
    ↓
Preset System (optional) → Prompt Enhancement
    ↓
Provider Registry → Select Provider
    ↓
Provider Implementation → API Call
    ↓
Job Tracker → Track Status
    ↓
Result Object → File Download
    ↓
Output File (exports/ directory)
```

## Key Design Decisions

1. **Provider Abstraction**: Each AI service has a dedicated provider class, making it easy to add new services without changing core logic.

2. **Preset-Driven Design**: Presets encapsulate best practices for each style, reducing user complexity while allowing full customization.

3. **Async Job Support**: Some providers (MiniMax) are asynchronous, so the framework supports job tracking and polling.

4. **File-Based Output**: All generated content is saved to the `exports/` directory with timestamps and provider names.

5. **CLI-First Experience**: While programmatic API is available, the CLI provides the primary user interface with rich formatting.

## Extension Points

To add a new provider:
1. Create a new provider class inheriting from `BaseProvider`
2. Implement required methods: `generate()`, `_call_api()`
3. Register provider using `@registry.register()` decorator
4. Add provider to `providers/__init__.py`

To add a new preset:
1. Create preset dataclass in `presets/music.py` or `presets/video.py`
2. Add to preset registry dictionary
3. Preset automatically becomes available in CLI

## Dependencies

- **Typer**: CLI framework with rich formatting
- **Pydantic**: Data validation and settings management
- **Requests**: HTTP API calls
- **FFmpeg**: Media processing (combining audio/video)
- **Internet Archive**: Optional archival of generated content
- **YouTube API**: Optional YouTube upload functionality
