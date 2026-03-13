<div align="center">
  <!-- 顶部视觉 Banner -->
  <img src="https://img.shields.io/badge/AudioPlayer-Translate%20%26%20Expand-FF6B6B?style=for-the-badge&logo=audio-technica&logoColor=white" alt="AudioPlayer Banner">

  <!-- 核心状态徽章 -->
  <div style="margin: 10px 0;">
    <img src="https://img.shields.io/github/v/release/LINGXINGE/AudioPlayerTranslate-Expand?style=flat-square&label=Version&color=3498DB" alt="Version">
    <img src="https://img.shields.io/badge/Exiled-≥9.12.x-2ECC71?style=flat-square&label=Framework" alt="Exiled Version">
    <img src="https://img.shields.io/github/downloads/LINGXINGE/AudioPlayerTranslate-Expand/total?style=flat-square&label=Downloads&color=F39C12" alt="Total Downloads">
    <img src="https://img.shields.io/github/license/LINGXINGE/AudioPlayerTranslate-Expand?style=flat-square&label=License&color=9B59B6" alt="License">
  </div>

  # AudioPlayer Translate & Expand
  **Localized (Chinese) and extended AudioPlayer plugin for SCP: Secret Laboratory (Exiled Framework)**
</div>

---

## 📋 Table of Contents
- [Plugin Description](#plugin-description)
- [Downloads](#downloads)
- [Core Features](#core-features)
- [Command Usage](#command-usage)
- [Event Examples](#event-examples)
- [Configuration](#configuration)
- [Notes](#notes)
- [Authors & Feedback](#authors--feedback)

---

## Plugin Description
This is a **Chinese-localized and feature-extended** version of the original AudioPlayer plugin for SCP: Secret Laboratory, built on the Exiled framework. It retains all core functionalities of the original plugin while adding quality-of-life improvements and customizability.

> Original repository: [Antoniofo/AudioPlayer](https://github.com/Antoniofo/AudioPlayer)

---

## Downloads
| Framework | Minimum Version | Download Link |
|:---------:|:---------------:|:-------------:|
| Exiled    | ≥ 9.12.x        | [Latest Release ⬇️](https://github.com/LINGXINGE/AudioPlayerTranslate-Expand/releases/latest) |

---

## Core Features
🔊 **Enhanced Audio Control**
- Added `Volume` config option (0.0 ~ 1.0) to adjust global audio playback volume
- Support for `ps`-prefixed console commands (see [Command Usage](#command-usage))

🛠️ **Custom Events & Permissions**
- New custom event: `MusicPlayFinishEventArgs` (triggered when audio playback completes)
- Admin permission bypass: Allow admins to play custom audio without additional permissions

📝 **Quality of Life**
- Full Chinese localization for configs and in-game prompts
- Extended audio management commands (list/remove/clear loaded audio)

---

## Command Usage
All commands use the `ps` prefix (supports server console/remote admin tools).

| Command Format | Description | Example |
|:--------------|:------------|:--------|
| `ps play <File/URL>` | Play global audio | `ps play music.mp3` / `ps play https://example.com/audio.mp3` |
| `ps atplace <x> <y> <z> <Range> <File/URL>` | Play audio at specified coordinates (range-limited) | `ps atplace 10 20 30 50 bgm.mp3` |
| `ps stop [true]` | Stop global audio (add `true` to stop all local audio) | `ps stop` / `ps stop true` |
| `ps list` | List all audio files and loaded audio | `ps list` |
| `ps loaded` | List only loaded audio (in memory) | `ps loaded` |
| `ps info <AudioName>` | Query detailed info of a specific audio | `ps info music.mp3` |
| `ps remove <AudioName>` | Remove audio from memory | `ps remove music.mp3` |
| `ps clear` | Clear all loaded audio from memory | `ps clear` |
| `ps pause` | Pause global audio playback | `ps pause` |
| `ps resume` | Resume global audio playback (restart from beginning) | `ps resume` |
| `ps seek <Time(seconds)>` | Jump to specific timestamp in audio | `ps seek 30` |
| `ps progress` | Show current playback progress and status | `ps progress` |

---

## Event Examples
### 1. MusicPlayFinish Event
Triggered when audio playback completes (custom logic can be added):

<details>
<summary>Click to expand code example</summary>

```csharp
using Exiled.Events.EventArgs;
using Exiled.API.Features;

namespace YourPluginNamespace
{
    public class AudioEventHandler
    {
        // Register the event (add this to your plugin's OnEnabled method)
        public void RegisterEvents()
        {
            AudioPlayerTranslateExpand.Events.OnMusicFinish += OnMusicFinish;
        }

        // Event handler
        public static void OnMusicFinish(MusicPlayFinishEventArgs ev)
        {
            // Log debug info
            Log.Debug($"Audio playback completed: {ev.AudioName}");
            
            // Example: Send broadcast to all players
            Broadcast.SendToAll($"Audio {ev.AudioName} finished playing!", 5);
        }
    }
}
