# edit

A command-line video editing toolkit powered by ffmpeg. A single bash script for overlays, trimming, resizing, audio manipulation, and more.

## Install

```bash
# Linux / macOS / WSL
curl -fsSL https://raw.githubusercontent.com/dbystruev/edit/main/edit -o edit && chmod +x edit
```

Requires: bash, ffmpeg, ffprobe. The script auto-detects and offers to install missing dependencies.

## Commands

| Command | Description |
|---------|-------------|
| [pic](#pic) | Overlay images or videos (picture-in-picture) |
| [text](#text) | Add text overlays |
| [stat](#stat) | Display file metadata |
| [crop](#crop) | Extract a portion of video by time |
| [cut](#cut) | Remove a portion of video by time |
| [insert](#insert) | Insert another video at a timestamp |
| [audio](#audio) | Replace or mix audio from another file |
| [mute](#mute) | Mute or reduce audio at a time range |
| [resize](#resize) | Resize video dimensions |
| [frame](#frame) | Extract a single frame as an image |
| [dedup](#dedup) | Remove duplicate/static frames |
| [split](#split) | Split video into subclips |
| [join](#join) | Concatenate multiple clips |
| [fade-in](#fade-in) | Fade in from black/silence |
| [fade-out](#fade-out) | Fade out to black/silence |
| [transcribe](#transcribe) | Transcribe audio to text |

## Time Formats

All time parameters (`--at`, `--from`, `--to`, `--duration`) support:

| Format | Example | Meaning |
|--------|---------|---------|
| Seconds | `10` or `10.5` | 10 seconds |
| MM:SS | `3:45` | 3 minutes 45 seconds |
| HH:MM:SS | `1:03:45` | 1 hour 3 minutes 45 seconds |
| Frame number | `f180` | Frame 180 |
| Negative | `-30` | 30 seconds from end of clip |

---

## pic

Overlay an image or video onto a video at a specified time.

```
edit pic INPUT OUTPUT MEDIA [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--at TIME` | Start time (default: 0) |
| `--duration TIME` | Duration (default: video duration for videos, 3s for images) |
| `--rect X1,Y1 X2,Y2` | Placement rectangle (pixels or percentages) |
| `--opacity VAL` | Opacity 0.0-1.0 (default: 1.0) |
| `--audio VAL` | Audio mix level 0.0-1.0 (default: 0 = no audio from overlay) |
| `--keep-last SEC` | Hold last frame for N seconds after video ends (default: 0) |

**Examples:**

```bash
# Insert a logo for 10 seconds
edit pic input.mp4 output.mp4 logo.png --at 5 --duration 10 --rect 10%,10% 90%,90%

# Insert a video clip as picture-in-picture (plays for its full duration)
edit pic input.mp4 output.mp4 clip.mp4 --at 7 --rect 5%,5% 45%,95%

# Two overlays with different timing
edit pic input.mp4 output.mp4 \
    map.mp4 --at 7 --rect 5%,5% 45%,95% --keep-last 1 \
    weather.mp4 --at 25 --duration 3

# Overlay with audio mixing (50% volume from overlay)
edit pic input.mp4 output.mp4 interview.mp4 --at 10 --rect 20%,20% 80%,80% --audio 0.5

# Semi-transparent watermark
edit pic input.mp4 output.mp4 watermark.png --at 0 --rect 80%,80% 100%,100% --opacity 0.3
```

## text

Add text overlays to a video. Requires ffmpeg with libfreetype (drawtext filter) or ImageMagick.

```
edit text INPUT OUTPUT TEXT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--at TIME` | Start time (default: 0) |
| `--duration TIME` | Duration (default: 3) |
| `--rect X1,Y1 X2,Y2` | Text placement rectangle |
| `--color COLOR` | Text color (default: white) |
| `--size SIZE` | Font size in pixels (default: 384) |
| `--font FONT` | Font face name |
| `--bold` | Bold text |

**Examples:**

```bash
# Simple text overlay
edit text input.mp4 output.mp4 "Hello World" --at 0 --duration 5

# Multiple texts at different times
edit text input.mp4 output.mp4 \
    "Introduction" --at 0 --rect 10%,10% 90%,90% \
    "Chapter 2" --at 30 \
    "Conclusion" --at 55

# Styled text
edit text input.mp4 output.mp4 "Title" --color red --size 72 --bold --at 2
```

## stat

Display detailed information about video or image files.

```
edit stat FILE [FILE...] [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--json` | Output in JSON format |

**Examples:**

```bash
edit stat video.mp4
edit stat *.jpeg
edit stat video.mp4 --json
```

## crop

Extract (keep) a portion of video by time. Frame-accurate via re-encoding.

```
edit crop INPUT OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--from TIME` | Start time to keep (negative = from end) |
| `--to TIME` | End time to keep (negative = from end) |

**Examples:**

```bash
# Keep section from 15s to 25s
edit crop input.mp4 output.mp4 --from 15 --to 25

# Keep only first 30 seconds
edit crop input.mp4 output.mp4 --to 30

# Keep only last 30 seconds
edit crop input.mp4 output.mp4 --from -30

# Keep from 1m30s to 2m15s
edit crop input.mp4 output.mp4 --from 1:30 --to 2:15

# Keep frame 84 onward
edit crop input.mp4 output.mp4 --from f84
```

## cut

Remove a portion of video by time.

```
edit cut INPUT OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--from TIME` | Start of section to remove (negative = from end) |
| `--to TIME` | End of section to remove (negative = from end) |

**Examples:**

```bash
# Remove section from 15s to 25s
edit cut input.mp4 output.mp4 --from 15 --to 25

# Remove first 30 seconds
edit cut input.mp4 output.mp4 --to 30

# Remove last 30 seconds
edit cut input.mp4 output.mp4 --from -30

# Remove frames 100 to 200
edit cut input.mp4 output.mp4 --from f100 --to f200
```

## insert

Insert another video into a main video at a timestamp.

```
edit insert MAIN INSERTED OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--at TIME` | Timestamp to insert at (default: 0) |

**Examples:**

```bash
edit insert main.mp4 clip.mp4 output.mp4 --at 10
```

## audio

Replace or mix audio from another file at a timestamp.

```
edit audio INPUT AUDIO_OR_VIDEO OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--at TIME` | Start time for new audio (default: 0) |
| `--duration TIME` | Duration (default: full) |
| `--mix LEVEL` | Mix level 0.0-1.0 (default: 1.0 = full replacement) |

**Examples:**

```bash
# Replace audio starting at 10s
edit audio video.mp4 music.mp3 output.mp4 --at 10

# Mix at 50% volume for 5 seconds
edit audio video.mp4 music.mp3 output.mp4 --at 10 --duration 5 --mix 0.5
```

## mute

Mute or reduce audio at a specified time range.

```
edit mute INPUT OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--at TIME` | Start time (default: 0) |
| `--duration TIME` | Duration (default: full) |
| `--mix LEVEL` | Audio level: 0.0 = silence, 0.5 = half volume (default: 0) |

**Examples:**

```bash
# Mute first 5 seconds
edit mute input.mp4 output.mp4 --at 0 --duration 5

# Reduce volume to 30% between 10s and 20s
edit mute input.mp4 output.mp4 --at 10 --duration 10 --mix 0.3

# Mute from 30s to end of video
edit mute input.mp4 output.mp4 --at 30
```

## resize

Resize video dimensions.

```
edit resize INPUT OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--width W` | Target width |
| `--height H` | Target height |
| `--scale P` | Scale factor (e.g., 0.5) |
| `--mode M` | Resize mode (see below) |

**Modes** (when both `--width` and `--height` are set):
| Mode | Description |
|------|-------------|
| `aspect-fit` | Fit inside box, pad with black bars (default) |
| `aspect-fill` | Fill box, crop excess |
| `center` | No scaling, center in new frame, pad or crop |
| `stretch` | Exact dimensions, may distort |

**Examples:**

```bash
# Scale by factor
edit resize input.mp4 output.mp4 --scale 0.5

# Fit inside 1920x1080, keeping aspect ratio
edit resize input.mp4 output.mp4 --width 1920 --height 1080

# Fill 1920x1080, cropping excess
edit resize input.mp4 output.mp4 --width 1920 --height 1080 --mode aspect-fill

# Center without scaling (pad if larger, crop if smaller)
edit resize input.mp4 output.mp4 --width 1920 --height 1080 --mode center

# Exact dimensions (may distort)
edit resize input.mp4 output.mp4 --width 1920 --height 1080 --mode stretch
```

## frame

Extract a single frame from video as an image.

```
edit frame INPUT OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--at TIME` | Time of frame to extract (default: 0) |

**Examples:**

```bash
# Extract frame at 10 seconds
edit frame input.mp4 frame.png --at 10

# Extract frame 180
edit frame input.mp4 frame.png --at f180

# Extract first frame (default)
edit frame input.mp4 frame.png
```

## dedup

Remove duplicate/static frames from video. Useful for removing pauses or static screens from recordings.

```
edit dedup INPUT OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--threshold N` | Sensitivity (default: 1024). Higher = more aggressive |

**Examples:**

```bash
# Default dedup
edit dedup input.mp4 output.mp4

# More aggressive
edit dedup input.mp4 output.mp4 --threshold 2048

# Less aggressive
edit dedup input.mp4 output.mp4 --threshold 512
```

## split

Split video into subclips at specified times.

```
edit split INPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--times T1,T2,T3` | Comma-separated split points |
| `--prefix NAME` | Output file prefix (default: input name) |

**Examples:**

```bash
# Split at 30s and 60s
edit split input.mp4 --times 30,60

# Split with custom prefix
edit split input.mp4 --times 30,60 --prefix clip
```

## join

Concatenate multiple clips into one.

```
edit join INPUT1 INPUT2 [INPUT3...] OUTPUT
```

**Examples:**

```bash
# Join two clips
edit join clip1.mp4 clip2.mp4 output.mp4

# Join multiple clips
edit join *.mp4 output.mp4
```

## fade-in

Apply fade-in effect from black/silence.

```
edit fade-in INPUT OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--duration TIME` | Fade duration (default: 3) |
| `--target WHAT` | What to fade: both (default), video, audio |

**Examples:**

```bash
edit fade-in input.mp4 output.mp4 --duration 5
edit fade-in input.mp4 output.mp4 --target video
```

## fade-out

Apply fade-out effect to black/silence at the end of a video.

```
edit fade-out INPUT OUTPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--duration TIME` | Fade duration (default: 3) |
| `--target WHAT` | What to fade: both (default), video, audio |

**Examples:**

```bash
edit fade-out input.mp4 output.mp4 --duration 5
edit fade-out input.mp4 output.mp4 --target audio
```

## transcribe

Transcribe audio/video to text with timestamps using local whisper inference. No API key needed.

**Requires:** `pip install openai-whisper`

```
edit transcribe INPUT [OPTIONS]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--model MODEL` | tiny, base, small (default), medium, large |
| `--language LANG` | Force language (e.g., ru, en, ja) |
| `--format FORMAT` | srt (default), txt, json, vtt |
| `--output FILE` | Output file (default: input name with new extension) |

**Examples:**

```bash
# Auto-detect language
edit transcribe input.mp4

# Russian transcription with medium model
edit transcribe input.mp4 --language ru --model medium

# Plain text output
edit transcribe input.mp4 --format txt
```

---

## Remote Testing via SSH Tunnel

To allow a remote server to SSH back into your Mac for local testing:

```bash
# 1. From your Mac, open a reverse tunnel (keep this session open):
ssh -R 20022:localhost:22 $SERVER

# 2. The server can now SSH into your Mac as $USER@localhost:
ssh -p 20022 $USER@localhost uptime && uname -a
ssh -p 20022 $USER@localhost 'export PATH="/opt/homebrew/bin:$PATH" && ffmpeg -version'
ssh -p 20022 $USER@localhost 'cd ~/Downloads/GitHub/dbystruev/edit && git pull'
```

If your Mac username differs from the server username, use `ssh -p 20022 $USER@localhost` where `$USER` is your Mac login name.

## License

MIT
