# RHClaw — RunningHub Skill for OpenClaw

Universal media generation skill for [OpenClaw](https://github.com/openclaw/openclaw), powered by [RunningHub](https://www.runninghub.cn) API.

**170+ endpoints** covering image, video, audio, 3D model generation, and multimodal text understanding.

## Capabilities

| Category | Endpoints | Tasks |
|----------|-----------|-------|
| **Image** | 42 | text-to-image, image-to-image, image upscale, Midjourney-style |
| **Video** | 94 | text-to-video, image-to-video, start-end frames, video extend/edit, motion control |
| **Audio** | 8 | text-to-speech, music generation, voice clone |
| **3D** | 12 | text-to-3D, image-to-3D, multi-image-to-3D |
| **Text** | 14 | image-to-text, video-to-text, text-to-text |

## Quick start

In your OpenClaw chat, say:

> Install the RunningHub skill from https://github.com/HM-RunningHub/OpenClaw_RH_Skills

The assistant will clone the repo, copy files to the workspace, and guide you through API key setup.

### Prerequisites

- **API Key** — Get one from [RunningHub API Management](https://www.runninghub.cn/enterprise-api/sharedApi) (click "新建")
- **Wallet balance** — [Recharge here](https://www.runninghub.cn/vip-rights/4) — API calls require funds

## Usage

Once installed, just talk to your OpenClaw assistant:

- *"Generate a picture of a dog playing in the park"*
- *"Turn this photo into a video"*
- *"Create background music for my video"*
- *"Upscale this image to 4K"*
- *"Convert this image to a 3D model"*

The assistant automatically selects the best RunningHub endpoint based on your request.

## Architecture

```
runninghub/
├── SKILL.md                        # OpenClaw skill definition (routing table + examples)
├── scripts/
│   ├── runninghub.py               # Universal API client (170+ endpoints)
│   └── build_capabilities.py       # Generates capabilities.json from models_registry.json
└── data/
    └── capabilities.json           # Full endpoint catalog (auto-generated)
```

## Script modes

| Mode | Command | Purpose |
|------|---------|---------|
| **Check** | `--check` | Verify API key + check wallet balance |
| **List** | `--list [--type T] [--task T]` | Browse available endpoints |
| **Info** | `--info ENDPOINT` | View endpoint parameters |
| **Execute** | `--endpoint EP --prompt "..." -o /tmp/out` | Run with specific endpoint |
| **Auto** | `--task TASK --prompt "..." -o /tmp/out` | Auto-select best endpoint |

## Updating capabilities

When RunningHub adds new API endpoints, regenerate the catalog:

```bash
python3 scripts/build_capabilities.py \
  --registry /path/to/ComfyUI_RH_OpenAPI/models_registry.json \
  --output data/capabilities.json
```

Then copy the updated `capabilities.json` to the OpenClaw workspace.
