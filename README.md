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

## Requirements

- bash
- ffmpeg / ffprobe
- ImageMagick (optional, for advanced image processing)

The script auto-detects and offers to install missing dependencies.

## Documentation

- [Implementation plan](TODO.md)
- [Completed items](DONE.md)

## License

MIT
