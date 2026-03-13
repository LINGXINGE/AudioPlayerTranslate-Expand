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
 **Enhanced Audio Control**
- Added `Volume` config option (0.0 ~ 1.0) to adjust global audio playback volume
- Support for `ps`-prefixed console commands (see [Command Usage](#command-usage))

 **Custom Events & Permissions**
- New custom events (please look code)
- Admin permission bypass: Allow admins to play custom audio without additional permissions

 **Quality of Life**
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
Triggered when audio playback completes.
<details>
<summary>Click to expand code example</summary>

```csharp
using Exiled.API.Features;
using AudioPlayerManager;

namespace YourPluginNamespace
{
    public class AudioEventHandler
    {
        public void RegisterEvents()
        {
            AudioCustomEvent.MusicPlayFinish += OnMusicPlayFinish;
        }

        public void OnMusicPlayFinish(MusicPlayFinishEventArgs ev)
        {
            Log.Debug($"Audio playback completed: {ev.ClipName}");
            Broadcast.SendToAll($"Audio {ev.ClipName} finished playing!", 5);
        }
    }
}
```
</details>

### 2. AudioPlayStart Event
Triggered when audio starts playing.
<details>
<summary>Click to expand code example</summary>

```csharp
using Exiled.API.Features;
using AudioPlayerManager;
using UnityEngine;

namespace YourPluginNamespace
{
    public class AudioEventHandler
    {
        public void RegisterEvents()
        {
            AudioCustomEvent.AudioPlayStart += OnAudioPlayStart;
        }

        public void OnAudioPlayStart(AudioPlayStartEventArgs ev)
        {
            Log.Debug($"Audio started: {ev.ClipName}");
            Log.Info($"Global: {ev.IsGlobal} | From Web: {ev.FromWeb}");
        }
    }
}
```
</details>

### 3. AudioPlayStop Event
Triggered when audio stops playing.
<details>
<summary>Click to expand code example</summary>

```csharp
using Exiled.API.Features;
using AudioPlayerManager;
using UnityEngine;

namespace YourPluginNamespace
{
    public class AudioEventHandler
    {
        public void RegisterEvents()
        {
            AudioCustomEvent.AudioPlayStop += OnAudioPlayStop;
        }

        public void OnAudioPlayStop(AudioPlayStopEventArgs ev)
        {
            Log.Debug($"Audio stopped: {ev.ClipName}");
            Broadcast.SendToAll($"Audio {ev.ClipName} has been stopped", 3);
        }
    }
}
```
</details>

### 4. AudioLoadComplete Event
Triggered when audio is loaded successfully.
<details>
<summary>Click to expand code example</summary>

```csharp
using Exiled.API.Features;
using AudioPlayerManager;

namespace YourPluginNamespace
{
    public class AudioEventHandler
    {
        public void RegisterEvents()
        {
            AudioCustomEvent.AudioLoadComplete += OnAudioLoadComplete;
        }

        public void OnAudioLoadComplete(AudioLoadCompleteEventArgs ev)
        {
            Log.Debug($"Audio loaded: {ev.ClipName}");
            Log.Info($"Source: {ev.SourcePath} | From Web: {ev.FromWeb}");
        }
    }
}
```
</details>

### 5. AudioLoadFailed Event
Triggered when audio fails to load.
<details>
<summary>Click to expand code example</summary>

```csharp
using Exiled.API.Features;
using AudioPlayerManager;

namespace YourPluginNamespace
{
    public class AudioEventHandler
    {
        public void RegisterEvents()
        {
            AudioCustomEvent.AudioLoadFailed += OnAudioLoadFailed;
        }

        public void OnAudioLoadFailed(AudioLoadFailedEventArgs ev)
        {
            Log.Error($"Failed to load audio: {ev.ClipName}");
            Log.Error($"Error: {ev.ErrorMessage}");
        }
    }
}
```
</details>

---

### Full Event Registration (All Events)
<details>
<summary>Click to expand full example</summary>

```csharp
using Exiled.API.Features;
using AudioPlayerManager;
using System;

namespace YourPluginNamespace
{
    public class YourPlugin : Plugin<Config>
    {
        public override string Name => "YourAudioPlugin";
        public override string Author => "YourName";
        public override Version Version => new(1, 0, 0);
        public override Version RequiredExiledVersion => new(9, 12, 0);

        private AudioEventHandler _handler;

        public override void OnEnabled()
        {
            _handler = new AudioEventHandler();
            _handler.RegisterEvents();
            base.OnEnabled();
        }

        public override void OnDisabled()
        {
            _handler.UnregisterEvents();
            _handler = null;
            base.OnDisabled();
        }
    }

    public class AudioEventHandler
    {
        public void RegisterEvents()
        {
            AudioCustomEvent.AudioPlayStart += OnAudioPlayStart;
            AudioCustomEvent.AudioPlayStop += OnAudioPlayStop;
            AudioCustomEvent.AudioLoadComplete += OnAudioLoadComplete;
            AudioCustomEvent.AudioLoadFailed += OnAudioLoadFailed;
            AudioCustomEvent.MusicPlayFinish += OnMusicPlayFinish;
        }

        public void UnregisterEvents()
        {
            AudioCustomEvent.AudioPlayStart -= OnAudioPlayStart;
            AudioCustomEvent.AudioPlayStop -= OnAudioPlayStop;
            AudioCustomEvent.AudioLoadComplete -= OnAudioLoadComplete;
            AudioCustomEvent.AudioLoadFailed -= OnAudioLoadFailed;
            AudioCustomEvent.MusicPlayFinish -= OnMusicPlayFinish;
        }

        public void OnAudioPlayStart(AudioPlayStartEventArgs ev) => 
            Log.Debug($"Playback started: {ev.ClipName}");

        public void OnAudioPlayStop(AudioPlayStopEventArgs ev) => 
            Log.Debug($"Playback stopped: {ev.ClipName}");

        public void OnAudioLoadComplete(AudioLoadCompleteEventArgs ev) => 
            Log.Debug($"Loaded: {ev.ClipName}");

        public void OnAudioLoadFailed(AudioLoadFailedEventArgs ev) => 
            Log.Error($"Load failed: {ev.ClipName} | {ev.ErrorMessage}");

        public void OnMusicPlayFinish(MusicPlayFinishEventArgs ev) => 
            Log.Debug($"Playback finished: {ev.ClipName}");
    }

    public class Config : IConfig
    {
        public bool IsEnabled { get; set; } = true;
        public bool Debug { get; set; } = false;
    }
}
```
</details>
