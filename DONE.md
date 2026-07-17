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

## Phase 2: pic Command

- [x] Image overlay with --at, --duration, --rect, --mode, --opacity
- [x] Video overlay (picture-in-picture) with dynamic playback
- [x] Sequence mode for multiple overlays
- [x] Parameter file loading (`--file`)
- [x] `--audio` option for mixing overlay audio at --at timestamp (using adelay)
- [x] `--keep-last` option to hold last frame after PiP video ends
- [x] SAR normalization in overlay filter to prevent stretching

## Phase 3: text Command

- [x] Text overlay with --at, --duration, --rect, --color, --size, --font, --bold
- [x] Sequence mode for multiple texts
- [x] ImageMagick fallback for macOS

## Phase 4: stat Command

- [x] Display: file size, format, codec, resolution, frame rate, duration, frames, bitrate
- [x] `--json` output option

## Phase 5: Additional Commands

- [x] **crop** - frame-accurate extraction (re-encoding, not stream copy)
- [x] **cut** - remove portions of video by time
- [x] **insert** - insert video at timestamp
- [x] **audio** - replace/mix audio from another file
- [x] **mute** - silence or reduce audio at time range
- [x] **resize** - aspect-fit, aspect-fill, center, stretch modes with SAR normalization
- [x] **frame** - extract single frame as image
- [x] **dedup** - remove duplicate/static frames
- [x] **split** - split video into subclips at specified times
- [x] **join** - concatenate multiple clips
- [x] **fade-in** / **fade-out** - video and audio fade effects
- [x] **transcribe** - local whisper transcription (auto-installs)

## Time Format Support

- [x] All time parameters support seconds, MM:SS, HH:MM:SS, fXXX (frame numbers), negative values

## Documentation

- [x] Complete README.md with all commands, options, tables, and examples
- [x] SSH tunnel instructions for remote Mac testing
- [x] Help text with examples for every command

## Platform Notes

- macOS: ffmpeg 8.1.2, drawtext unavailable (ImageMagick fallback for text)
- Tested on Ubuntu (ffmpeg 4.4.2) and macOS (ffmpeg 8.1.2)
