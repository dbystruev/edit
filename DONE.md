# Completed Items

Cross-reference: [README.md](README.md) | [Implementation plan](TODO.md)

## Phase 1: Script Framework

- [x] Created `edit` bash script with shebang `#!/usr/bin/env bash`
- [x] Implemented `-h` / `--help` flag to display usage information
- [x] Script shows help when run without any arguments
- [x] Implemented `-y` / `--yes` flag to auto-confirm all prompts
- [x] Created dependency check function to verify ffmpeg/ffprobe are installed
- [x] Implemented auto-install with user permission (apt, brew, choco, winget, dnf, yum, pacman, zypper)
- [x] Created main command dispatcher to route subcommands

## Phase 2: pic Command (image overlay)

- [x] Parse required arguments: input video, output video, picture file
- [x] Parse `--at` parameter: accept seconds (decimal) or frame number (prefix with 'f')
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

## Phase 3: text Command (text overlay)

- [x] Parse required arguments: input video, output video, text string
- [x] Parse `--at` parameter (same as pic)
- [x] Parse `--duration` parameter (same as pic)
- [x] Parse `--appear` / `--disappear` parameters (same as pic)
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
- [x] Execute ffmpeg command and handle errors

## Phase 4: stat Command (file info)

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
- [x] Format output as readable table
- [x] Added `--json` and `--short` output options

## Phase 5: Polish & Testing

- [x] Added examples to --help output
- [x] Created dedicated help for each subcommand
- [x] Simplified command names: pic, text, stat
