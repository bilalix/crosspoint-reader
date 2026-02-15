# Architecture Overview

CrossPoint is firmware for the Xteink X4 built on ESP32-C3 with PlatformIO and Arduino.
The system is organized around activities (screens), shared app state, and EPUB rendering.

## Main runtime flow

- Entry point: `src/main.cpp`
- Boot initializes GPIO, serial logging, SD storage, settings, and display
- App then transitions into the current activity (home, reader, settings, network, etc.)
- Main loop updates input, runs current activity, handles sleep/power logic

## Key directories

- `src/activities/`: UI and behavior per screen or flow
- `src/components/`: UI themes and shared UI support
- `src/network/`: web server, OTA, and network helpers
- `src/`: app-level state, settings, and orchestration
- `lib/Epub/`: EPUB parsing, layout, hyphenation, rendering data models
- `open-x4-sdk/`: hardware-facing SDK used via submodule

## State and persistence

- `CrossPointSettings` stores user settings and is loaded at startup
- `CrossPointState` stores app state such as last opened book and sleep context
- EPUB metadata, sections, and progress are cached on SD card under `.crosspoint/`

## Embedded constraints that shape design

- ESP32-C3 has limited RAM, so aggressive SD caching is used
- Display updates are expensive, so activities and rendering optimize refresh behavior
- Long-running work must avoid blocking the main loop too heavily

## Scope guardrails

Before implementing larger ideas, check:

- [SCOPE.md](../../SCOPE.md)
- [GOVERNANCE.md](../../GOVERNANCE.md)
