# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Digital Logic Simulator - A Unity-based application for building and simulating digital logic circuits. Built by SebastianLague as part of the "Exploring How Computers Work" YouTube series.

**Current Version:** 2.1.6

## Build and Development

This is a Unity C# project with no external build system.

**To Build:**
- Open project in Unity Editor
- File > Build Settings > Build
- Main scene: `Assets/Build/DLS.unity`
- Supported platforms: Windows, WebGL

**To Run Tests:**
- Unity Editor > Window > Test Runner > EditMode tab > Run All
- Tests located in `Assets/Scripts/Description/Serialization/Newtonsoft/Tests/Editor/`

## Architecture

### Core Namespaces

- **DLS.Game** - Main game controller, project management, chip library
- **DLS.Simulation** - Multi-threaded simulation engine
- **DLS.Description** - Data models for chips, projects, settings
- **DLS.Graphics** - UI and world rendering
- **DLS.SaveSystem** - JSON persistence layer
- **Seb.Vis** - Custom visualization framework (not standard Unity UI)

### Key Files

- `Assets/Scripts/Game/Main/Main.cs` - Static app state manager
- `Assets/Scripts/Game/Main/UnityMain.cs` - Unity MonoBehaviour entry point
- `Assets/Scripts/Simulation/Simulator.cs` - Core simulation loop (runs on separate thread)
- `Assets/Scripts/Game/Interaction/ChipInteractionController.cs` - Main user interaction handler
- `Assets/Scripts/SaveSystem/Serializer.cs` - JSON serialization using Newtonsoft.Json
- `Assets/Scripts/Graphics/UI/UIDrawer.cs` - Main UI rendering coordinator

### Data Flow

```
Player Input → ChipInteractionController → Project State → Simulator
                        ↓                                       ↓
                  Save System ←───────────────────── Simulation Output
                        ↓
                 File System (JSON)
```

### Assembly Structure

The project uses Unity assembly definitions for modular organization:
- `DLS.asmdef` - Main game assembly
- `DLS.Description.asmdef` - Data models
- `Seb.asmdef` - Utility library
- `DLS.Dev.asmdef` - Development tools

## Important Implementation Details

### Simulation Engine
- Multi-threaded: Uses concurrent queues for thread-safe modifications from main thread
- Order-pass algorithm determines chip traversal order for handling feedback loops
- Supports 3-state logic in `PinState.cs`

### Custom UI Framework
- Uses `Seb.Vis.UI` instead of standard Unity UI
- All menus in `Assets/Scripts/Graphics/UI/Menus/`

### Save System
- All data stored as JSON using Newtonsoft.Json
- Paths managed in `SavePaths.cs`
- Version migration support in `UpgradeHelper.cs`

## Test Data

Pre-built chip definitions for testing are in `TestData/Projects/MainTest/Chips/` (80+ JSON files including AND, OR, XOR, flip-flops, memory, ALU components).

## Contributing

From README: Performance/UX improvements and bug fixes are welcomed. New features are less likely to be accepted. Use GitHub Discussions for development questions.
