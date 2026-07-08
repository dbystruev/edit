# Implementation Plan

Cross-reference: [README.md](README.md) | [Completed items](DONE.md)

## Phase 1: Script Framework

- [x] Create `edit` bash script with shebang `#!/usr/bin/env bash`
- [x] Implement `-h` / `--help` flag to display usage information
- [x] Show help when script is run without any arguments
- [x] Implement `-y` / `--yes` flag to auto-confirm all prompts
- [x] Create dependency check function to verify ffmpeg/ffprobe are installed
- [x] Implement auto-install with user permission (apt, brew, choco detection)
- [x] Create main command dispatcher to route subcommands

## Phase 2: insert-picture Command

- [x] Parse required arguments: input video, output video, picture file
- [x] Parse `--at` parameter: accept seconds (decimal) or frame number
- [x] Parse `--duration` parameter: accept seconds (decimal) or frame count, default 3s
- [x] Parse `--appear` parameter: fade-in modes (none, quick, slow)
- [x] Parse `--disappear` parameter: fade-out modes (none, quick, slow)
- [x] Parse `--rect` parameter: two corner coordinates (pixels or percentages)
- [x] Parse `--mode` parameter: aspect-fit (default), aspect-fill, scale
- [x] Parse `--opacity` parameter: 0.0-1.0 or 0%-100% (default opaque=1.0)
- [x] Parse `--transparent-bg` parameter: yes/no for PNG/GIF transparency
- [x] Validate all input files exist before processing
- [x] Auto-detect video resolution for percentage coordinate conversion
- [x] Generate ffmpeg filter complex for picture overlay
- [x] Implement fade-in/fade-out using ffmpeg fade filter
- [x] Handle aspect-fit: fit image within rect preserving aspect ratio
- [x] Handle aspect-fill: fill rect cropping image preserving aspect ratio
- [x] Handle scale: stretch image to fill rect exactly
- [x] Execute ffmpeg command and handle errors

## Phase 3: insert-text Command

- [x] Parse required arguments: input video, output video, text string
- [x] Parse `--at` parameter (same as insert-picture)
- [x] Parse `--duration` parameter (same as insert-picture)
- [x] Parse `--appear` / `--disappear` parameters (same as insert-picture)
- [x] Parse `--rect` parameter for text placement area
- [x] Parse `--color` parameter: named colors or hex values
- [x] Parse `--size` parameter: font size in pixels
- [x] Parse `--font` parameter: font face name (system default)
- [x] Parse `--bold` flag for bold text
- [x] Parse `--italic` flag for italic text
- [x] Parse `--outline` parameter: text outline width and color
- [x] Parse `--shadow` parameter: drop shadow offset and color
- [x] Parse `--bg-color` parameter: text background color
- [x] Generate ffmpeg drawtext filter with all options
- [x] Handle text wrapping within rectangle bounds
- [x] Execute ffmpeg command and handle errors

## Phase 4: stats Command

- [x] Parse input file argument
- [x] Run ffprobe to extract video metadata
- [x] Display: file size (human readable)
- [x] Display: container format
- [x] Display: video codec
- [x] Display: resolution (width x height)
- [x] Display: frame rate
- [x] Display: duration / length
- [x] Display: total frame count
- [x] Display: bitrate
- [x] Display: pixel format
- [x] For images: display color depth, channels, DPI if available
- [x] Format output as readable table

## Phase 5: Polish & Testing

- [ ] Add error handling for invalid inputs
- [ ] Add progress indication for long operations
- [ ] Test on Linux (Ubuntu/Debian)
- [ ] Test on macOS
- [ ] Test on WSL
- [x] Add examples to --help output
- [ ] Create test video/images for verification
