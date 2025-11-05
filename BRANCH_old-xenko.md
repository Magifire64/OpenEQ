# Old-Xenko Branch Documentation

## Overview

The old-xenko branch represents a previous implementation of OpenEQ using the Xenko game engine (now known as Stride). This branch integrated zone conversion directly into the game and focused on rendering optimizations like mipmap generation. It appears to be archived/deprecated in favor of the current master branch approach.

## Branch Status

**Status**: Archived / Legacy  
**Last Major Update**: Commit 5038d38 ("Added ahead-of-time mipmap generation...")  
**Divergence from Master**: Completely different architecture using Xenko engine  
**Reason for Deprecation**: Likely switched to Silk.NET approach (master branch)

## Current Implementation

### Technology Stack
- **Language**: C# (.NET Framework)
- **Engine**: Xenko (now Stride) - 3D game engine
- **Graphics**: Xenko's rendering pipeline
- **Platform**: Windows desktop (OpenEQ.Windows project)
- **Project Structure**: Multi-project solution

### Project Structure

```
OpenEQ.sln              # Solution file
OpenEQ/
  ├── OpenEQ.Game/      # Core game logic
  │   ├── FileConverter/    # Integrated conversion tools
  │   ├── Chat/             # Chat system
  │   ├── Classes/          # EQ class types
  │   ├── Configuration/    # App configuration
  │   └── *.cs              # Game scripts
  └── OpenEQ.Windows/   # Windows platform entry point
Tools/                  # Additional tools
converter/              # Converter utilities
openeq.cfg              # Configuration file
```

### Key Components

1. **Xenko Engine Integration**
   - Uses Xenko's entity-component system
   - Xenko's scene graph and rendering
   - Built-in asset pipeline
   - SyncScript for game logic

2. **Game Project** (`OpenEQ.Game/`)
   - `BasicCameraController.cs` - Camera controls (WASD, mouse)
   - `FPSCounter.cs` - Performance monitoring
   - `Extensions.cs` - Utility extensions
   - `DESCrypto.cs` - Encryption for EQ protocol

3. **FileConverter** (`OpenEQ.Game/FileConverter/`)
   - Integrated into the game (not separate tool)
   - `S3DConverter.cs` - S3D archive converter
   - `WldConverter.cs` - WLD file converter
   - `Program.cs` - Conversion entry point
   - Same entities as FileConverter branch

4. **Networking** (Partial)
   - Chat server implementation (`Chat/ChatServer.cs`)
   - Network communication halts noted in commits
   - Character creation attempted but incomplete

5. **Configuration** (`Configuration/OpenEQConfiguration.cs`)
   - Login server address and port
   - Server list management
   - Application settings

6. **Classes System** (`Classes/`)
   - EverQuest class types (Warrior, Cleric, etc.)
   - Class type extensions and helpers

7. **Windows Platform** (`OpenEQ.Windows/`)
   - Entry point (`OpenEQApp.cs`)
   - Platform-specific setup
   - Xenko game initialization

## Features Implemented

### ✅ Working Features

1. **Xenko Engine**
   - Full 3D rendering via Xenko
   - Entity-component architecture
   - Scene management
   - Built-in physics

2. **Camera Controls**
   - WASD movement
   - Q/E for up/down
   - Mouse look (right-click drag)
   - Numpad rotation
   - Shift for speed boost
   - Touch input support (multi-platform)

3. **Zone Rendering**
   - Integrated zone conversion
   - Direct loading of EQ zones
   - Zone geometry display
   - Texture mapping

4. **Mipmap Generation**
   - Ahead-of-time mipmap generation
   - Alpha scaling for transparent textures
   - Handles non-mipped textures
   - Note: "Incredibly slow" per commit message

5. **File Conversion**
   - S3D archive parsing
   - WLD file conversion
   - Integrated into game runtime
   - Can convert on-the-fly

6. **Configuration System**
   - Login server configuration
   - Server list sorting
   - Application settings file (openeq.cfg)

7. **FPS Counter**
   - Performance monitoring
   - Frame rate display

## Known Issues & Bugs

### 🐛 Current Bugs

1. **Network Communication Halts**
   - Commit note: "Doesn't get past char select though. Appears that network communication halts. Not sure why yet."
   - Character creation partially works
   - Cannot proceed past character selection
   - Network protocol incomplete

2. **Mipmap Generation Performance**
   - Commit note: "incredibly slow mipmap generation"
   - Performance bottleneck
   - May cause long load times

3. **Character System Incomplete**
   - Hardcoded character: warrior, barbarian, hardcoded name
   - Cannot customize characters
   - Limited character support

4. **Xenko Engine Dependency**
   - Tied to specific Xenko beta version
   - Xenko is now Stride (name change)
   - May not work with latest Stride versions

5. **Test Account Exposure**
   - Commit note: "Removed a test account password"
   - Suggests potential security issues in development

### ⚠️ Limitations

1. **Platform Support**
   - Windows-only (OpenEQ.Windows project)
   - No Linux/Mac support shown
   - Touch input present but platform unclear

2. **Engine Maturity**
   - Xenko/Stride is less mature than Unity/Unreal
   - Smaller community and ecosystem
   - Documentation may be limited

3. **Network Features**
   - Login mostly works
   - Character select doesn't work
   - Cannot enter game world
   - No zone server connection

4. **Asset Pipeline**
   - Requires EQ assets
   - Conversion integrated but slow
   - No asset streaming

## Not Yet Implemented

### 🚧 Missing Features

1. **Network Completion**
   - [ ] Fix character select network halt
   - [ ] Complete zone server connection
   - [ ] Implement all packet types
   - [ ] Full protocol support

2. **Character System**
   - [ ] Character customization
   - [ ] All races and classes
   - [ ] Equipment display
   - [ ] Character animations

3. **Gameplay**
   - [ ] Movement in world
   - [ ] NPC interactions
   - [ ] Combat
   - [ ] Inventory system
   - [ ] Skills and spells

4. **Performance**
   - [ ] Optimize mipmap generation
   - [ ] Texture streaming
   - [ ] Level-of-detail (LOD)
   - [ ] Frustum culling

5. **UI**
   - [ ] Login screen
   - [ ] Character select UI
   - [ ] In-game HUD
   - [ ] Chat window

6. **Audio**
   - [ ] Sound effects
   - [ ] Music
   - [ ] 3D audio

7. **Multi-platform**
   - [ ] Linux support
   - [ ] Mac support
   - [ ] Console ports

## Building and Running

### Prerequisites
- Visual Studio 2017 or later
- Xenko game engine (specific beta version)
  - Note: Xenko is now called Stride Engine
  - May need to install legacy Xenko version
- .NET Framework 4.6.1 or later
- Windows OS

### Build Steps

```bash
# Open solution
Open OpenEQ.sln in Visual Studio

# Restore NuGet packages
Right-click solution → Restore NuGet Packages

# Build solution
Build → Build Solution (F6)

# Run
Set OpenEQ.Windows as startup project
Press F5 to run
```

### Configuration

Edit `openeq.cfg` to configure:
- Login server address
- Login server port
- Other settings

### Asset Requirements

Place EverQuest asset files in expected location (check code for paths).

## Development Notes

### Xenko/Stride Engine

Xenko key concepts:
- **Entities**: Game objects with components
- **Components**: Behavior and data (Transform, Camera, Model, Script, etc.)
- **Scripts**: C# classes that inherit from SyncScript or AsyncScript
- **Scenes**: Hierarchies of entities
- **Assets**: Textures, models, materials managed by engine

### SyncScript vs AsyncScript

- **SyncScript**: Update() called every frame (used by BasicCameraController)
- **AsyncScript**: Async/await based for background tasks

### Camera Controller

The BasicCameraController is a reusable Xenko script:
- Attach to camera entity
- Configurable speeds
- Multi-input support (keyboard, mouse, touch, gamepad)

### Integration with Xenko Asset Pipeline

Xenko has its own asset system. The OpenEQ project likely:
1. Converts EQ assets to Xenko formats
2. Uses Xenko's texture and model formats
3. Leverages Xenko's material system

## Comparison with Other Branches

| Feature | Master | old-xenko | godoteq | FileConverter |
|---------|--------|-----------|---------|---------------|
| Engine | Silk.NET | Xenko/Stride | Godot | None |
| Platform | Windows/Linux | Windows | Cross-platform | Windows |
| Rendering | Custom OpenGL | Xenko renderer | Godot renderer | N/A |
| Networking | Partial | Partial (buggy) | More complete | None |
| Conversion | External tool | Integrated | Unknown | Focus |
| UI Framework | NoesisGUI | Xenko UI | Godot UI | CLI |
| Status | Active | Archived | Experimental | Experimental |

## Why Was This Branch Abandoned?

Likely reasons (inferred):

1. **Network Issues**: Character select doesn't work, blocking progress
2. **Performance**: Mipmap generation is too slow
3. **Engine Maturity**: Xenko/Stride less mature than alternatives
4. **Control**: Custom engine (master) provides more control
5. **Simplicity**: Silk.NET is simpler than full game engine

## Advantages of Xenko Approach

1. **Full Engine**: Complete game engine with editor
2. **Asset Pipeline**: Built-in asset management
3. **Visual Editor**: Scene editor for level design
4. **Cross-platform**: Xenko supports multiple platforms
5. **Modern Rendering**: PBR, post-processing, etc.

## Disadvantages of Xenko Approach

1. **Dependency**: Tied to Xenko/Stride updates
2. **Learning Curve**: Need to learn Xenko
3. **Performance**: Engine overhead
4. **Debugging**: Harder to debug engine issues
5. **Community**: Smaller community than Unity/Unreal

## Migration Path

### To continue this branch:

1. Fix network communication halt
2. Optimize mipmap generation
3. Update to latest Stride engine
4. Complete character system
5. Implement missing packets

### To port improvements to Master:

1. Extract mipmap generation code
2. Port configuration system
3. Use camera control ideas
4. Keep Xenko-specific code separate

### To upgrade to Stride:

1. Install Stride engine (Xenko's successor)
2. Update package references
3. Fix any API changes
4. Test thoroughly

## Testing

No automated tests present. Manual testing:

1. Launch OpenEQ.Windows
2. Should see Xenko game window
3. Camera controls should work
4. Zone should render (if assets present)
5. Network likely fails at character select

## Performance Notes

- Mipmap generation is a bottleneck
- Xenko renderer performance depends on scene complexity
- FPS counter shows real-time performance
- Camera movement should be smooth

## Historical Context

This branch represents an earlier development phase where the team:
1. Chose Xenko as the engine
2. Integrated conversion into runtime
3. Worked on networking
4. Hit roadblocks (performance, network bugs)
5. Eventually pivoted to master branch approach

## Conclusion

The old-xenko branch is a valuable historical artifact showing an alternate implementation using a full-featured game engine. While it had promise with integrated conversion and a complete rendering pipeline, performance issues and network bugs led to its abandonment in favor of the simpler, more controlled approach in the master branch.

**Best used for:**
- Understanding Xenko/Stride engine usage
- Learning from architectural decisions
- Historical reference
- Salvaging specific features (config system, camera controls)

**Not recommended for:**
- New development
- Production use
- Extending OpenEQ

**Recommendation**: Use master branch for new work, but study this branch for:
- Mipmap generation approach (after optimization)
- Configuration system design
- Camera controller patterns
- Integration ideas

This branch is essentially archived but contains useful code that could be ported to active branches.
