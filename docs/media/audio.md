---
title: "Audio"
icon: "🔊"
created: 2026-04-22
updated: 2026-04-22
---

# Audio

s&box has a full audio system including 2D/3D sound playback, music, voice chat, positional audio, an audio effects pipeline, and lip sync support.

## Components

Audio components can be added to any GameObject in the editor.

| Component | Description |
|---|---|
| `SoundPointComponent` | Plays a sound at a point in the world |
| `SoundBoxComponent` | Plays a sound within a box area |
| `SoundscapeTrigger` | Plays a soundscape when the listener enters the trigger area |
| `AudioListener` | Overrides where the client hears audio from, instead of the camera |
| `LipSyncComponent` | Drives morphs with lipsync from sounds |
| `VoiceComponent` | Records and transmits microphone input to other players |

## MusicPlayer

`MusicPlayer` is for music tracks and streaming audio that need programmatic control over playback, positioning, or visualization. Use Sound components for in-game sound effects.

```csharp
var music = MusicPlayer.Play( FileSystem.Mounted, "music/theme.ogg" );
music.Volume = 0.8f;
music.Repeat = true;
```

Streaming from a URL:

```csharp
var music = MusicPlayer.PlayUrl( "https://example.com/stream.ogg" );
```

By default, `MusicPlayer` plays globally. To place it in world-space:

```csharp
music.ListenLocal = false;
music.Position = WorldPosition;
```

Audio visualization:

```csharp
ReadOnlySpan<float> spectrum = music.Spectrum;   // 512-element FFT magnitudes
float amplitude = music.Amplitude;               // approximate loudness
```

### Supported Formats

| Codec | Containers |
|---|---|
| Opus | .ogg, .opus, .webm |
| Vorbis | .ogg, .webm |
| FLAC | .flac |
| MP3 | .mp3 |
| WAV / PCM | .wav |
| AAC | MP4 (Windows only via Media Foundation, not recommended) |

:::warning
AAC is only available on Windows via system decoders and is not recommended. Use Opus instead.
:::
