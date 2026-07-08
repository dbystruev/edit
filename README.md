# edit

A command-line video editing toolkit powered by ffmpeg. Insert pictures and text overlays onto videos, get detailed file stats, all from a single unified script.

## Quick start

```bash
# Install (Linux/macOS/WSL)
curl -fsSL https://raw.githubusercontent.com/dbystruev/edit/main/edit -o edit && chmod +x edit

# Show help
./edit -h

# Insert a logo on a video
./edit pic input.mp4 output.mp4 logo.png --at 5 --duration 10 --rect 10%,10% 90%,90%

# Insert text overlay
./edit text input.mp4 output.mp4 "Hello World" --at 2 --duration 5 --color red --size 72

# Get video stats
./edit stat input.mp4
```

## Commands

| Command | Description |
|---------|-------------|
| `pic` | Overlay an image onto a video at a specified time |
| `text` | Add text overlay to a video with font/color control |
| `stat` | Display detailed info about a video or image file |

## Features

### pic command
- Insert multiple images in sequence
- Aspect-fit, aspect-fill, or scale modes
- Fade-in/fade-out effects
- Opacity control
- Load parameters from file (`--file`)

### text command
- Color, size, font control
- Bold, italic styling
- Text outline and shadow
- Background color with padding

### stat command
- Video and image metadata
- JSON output option

## Requirements

- bash
- ffmpeg / ffprobe

The script auto-detects and offers to install missing dependencies.

## Documentation

- [Implementation plan](TODO.md)
- [Completed items](DONE.md)

## License

MIT
