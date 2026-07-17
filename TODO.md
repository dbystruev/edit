# Implementation Plan

Cross-reference: [README.md](README.md) | [Completed items](DONE.md)

## Phase 1-5: Core Commands

- [x] All core commands implemented (pic, text, stat)
- [x] Full help system with examples for each command
- [x] Cross-platform support (Linux, macOS, WSL)

## Additional Commands

- [x] crop - frame-accurate video extraction by time
- [x] cut - remove portions of video by time
- [x] insert - insert video at timestamp
- [x] audio - replace/mix audio from another file
- [x] mute - silence or reduce audio at time range
- [x] resize - resize with aspect-fit, aspect-fill, center, stretch modes
- [x] frame - extract single frame as image
- [x] dedup - remove duplicate/static frames
- [x] split - split video into subclips
- [x] join - concatenate multiple clips
- [x] fade-in / fade-out - video and audio fade effects
- [x] transcribe - local whisper transcription

## Time Format Support

- [x] All commands support seconds, MM:SS, HH:MM:SS, frame numbers (fXXX), negative values

## Pic Command Enhancements

- [x] Video overlay (picture-in-picture) with dynamic playback
- [x] `--keep-last` option to hold last frame after PiP video ends
- [x] `--audio` option to mix overlay audio at correct timestamp
- [x] SAR normalization for correct aspect ratio when inserting resized videos
- [x] PTS-based overlay timing with eof_action=pass

## Resize Enhancements

- [x] aspect-fit / aspect-fill / center / stretch modes
- [x] SAR normalization (setsar=1) to prevent stretching
- [x] Single dimension auto-calculation (width-only or height-only)

## Documentation

- [x] Complete README with all commands, options, and examples
- [x] SSH tunnel instructions for remote Mac testing
