# Master Branch Documentation

## Overview

The master branch is the primary development branch of OpenEQ. It uses Silk.NET for modern OpenGL rendering and NoesisGUI for the user interface. This branch represents the most stable and actively maintained version of the project.

## Current Implementation

### Technology Stack
- **Language**: C# (.NET)
- **Graphics API**: OpenGL 4.x
- **Graphics Library**: Silk.NET (formerly OpenTK)
- **UI Framework**: NoesisGUI
- **Scripting**: IronPython for UI scripting
- **Math Library**: System.Numerics

### Architecture

#### Core Components

1. **Engine** (`Engine/`)
   - `EngineCore.cs` - Main game loop, window management, and rendering
   - `DeferredPathway.cs` - Deferred rendering pipeline implementation
   - `FpsCamera.cs` - First-person camera controls
   - `Material.cs` - Material system for textures and shaders
   - `Mesh.cs`, `Model.cs` - 3D geometry and model management
   - `AnimatedMesh.cs`, `AniModel.cs`, `AniModelInstance.cs` - Skeletal animation support
   - `FrameBuffer.cs` - Framebuffer objects for rendering
   - `Shader.cs` - Shader program management
   - `Texture.cs` - Texture loading and management
   - `Buffer.cs` - OpenGL buffer abstractions

2. **OpenEQ** (`OpenEQ/`)
   - `App.cs` - Application entry point
   - `Controller.cs` - Main controller coordinating Engine and UI
   - `Loader.cs` - Asset loading from converted EQ files
   - `UI/` - User interface XAML and Python scripts
   - `Materials/` - Material definitions

3. **LegacyFileReader** (`LegacyFileReader/`)
   - Parsers for EverQuest file formats (.s3d, .wld)
   - Zone geometry readers
   - Model and texture readers

4. **ConverterApp/ConverterCore** (`ConverterApp/`, `ConverterCore/`)
   - Command-line tool to convert EQ assets to OES format
   - Converts zones, models, and textures
   - Generates `.oes.zip` files for runtime use

5. **Common** (`Common/`)
   - `OESFile.cs` - OpenEQ Storage format reader/writer
   - `Extensions.cs` - Utility extension methods

6. **ImageLib** (`ImageLib/`)
   - Image loading and conversion
   - Uses SixLabors.ImageSharp library

7. **CollisionManager** (`CollisionManager/`)
   - `Octree.cs` - Spatial partitioning for collision detection
   - `AABB.cs` - Axis-aligned bounding box
   - `Triangle.cs`, `Plane.cs` - Geometric primitives

8. **Netcode/NetClient** (`Netcode/`, `NetClient/`)
   - Network protocol implementation (experimental)
   - EverQuest client protocol parsing

9. **Zlib.Portable** (`Zlib.Portable/`)
   - Compression library for .s3d archive handling

## Features Implemented

### ✅ Working Features

1. **Zone Loading**
   - Loads converted EverQuest zones from OES format
   - Renders zone geometry with textures
   - Proper lighting and materials

2. **Model Loading**
   - Character model loading and display
   - Skeletal animation support
   - Animation playback (e.g., "C05" animation)

3. **Rendering**
   - Deferred rendering pipeline
   - Point light support
   - Texture mapping with proper UVs
   - Camera movement (WASD + mouse look)

4. **UI System**
   - NoesisGUI integration
   - XAML-based UI definitions
   - Python scripting for UI logic
   - Hot-reload functionality (ReloadUI event)

5. **File Format Support**
   - Reads .s3d (EQ archives)
   - Reads .wld (world files)
   - Converts to .oes.zip format

6. **Camera Controls**
   - First-person camera
   - Mouse look
   - WASD movement
   - Fly mode

## Known Issues & Bugs

### 🐛 Current Bugs

1. **Performance Issues**
   - Large zones may have frame rate drops
   - No frustum culling implemented
   - No level-of-detail (LOD) system

2. **Rendering Issues**
   - Some transparency/alpha blending may not work correctly
   - Lighting calculations may need tuning
   - No shadows implemented

3. **Animation System**
   - Animation system is basic
   - No animation blending
   - Limited animation control

4. **UI System**
   - Python UI scripting is functional but may have edge cases
   - UI hot-reload may have memory leaks (WeakReference usage)

5. **File Loading**
   - Must pre-convert files using ConverterApp
   - No direct loading of EQ files at runtime
   - Hardcoded paths in App.cs (e.g., `../ConverterApp/`)

### ⚠️ Limitations

1. **Networking**
   - NetClient/Netcode are present but not fully integrated
   - No multiplayer functionality
   - Cannot connect to live EQ servers

2. **Asset Pipeline**
   - Two-step process: convert then load
   - No in-game asset browser

3. **UI**
   - Limited UI elements
   - No in-game menus or HUD

## Not Yet Implemented

### 🚧 Planned/Missing Features

1. **Rendering Enhancements**
   - [ ] Frustum culling
   - [ ] Occlusion culling
   - [ ] Level-of-detail (LOD) system
   - [ ] Shadow mapping
   - [ ] Screen-space ambient occlusion (SSAO)
   - [ ] Better transparency handling
   - [ ] Particle effects

2. **Game Features**
   - [ ] Character controller (movement, jumping)
   - [ ] NPC rendering and animation
   - [ ] Zone streaming
   - [ ] Sound and music
   - [ ] Weather effects
   - [ ] Day/night cycle

3. **Tools**
   - [ ] In-game asset browser
   - [ ] Debug visualization tools
   - [ ] Performance profiler
   - [ ] Material editor

4. **File Formats**
   - [ ] More comprehensive .wld parsing
   - [ ] Support for newer EQ expansions
   - [ ] Texture format variations

5. **UI Improvements**
   - [ ] Main menu
   - [ ] Zone selection interface
   - [ ] Settings menu
   - [ ] In-game HUD

6. **Networking**
   - [ ] Full network protocol implementation
   - [ ] Server browser
   - [ ] Character management

## Building and Running

### Prerequisites
- .NET SDK 6.0 or later
- Visual Studio 2019+ or compatible IDE

### Build Steps
```bash
# Restore dependencies
dotnet restore

# Build the solution
dotnet build OpenEQ.sln

# Build the converter (if needed)
dotnet build ConverterApp/ConverterApp.csproj
```

### Converting Assets
Before running OpenEQ, you need to convert EQ zone files:

```bash
cd ConverterApp
dotnet run -- <path_to_eq_zone_files> <zone_name>
```

This will create `<zone_name>_oes.zip` files.

### Running OpenEQ
```bash
cd OpenEQ
dotnet run -- <zone_name>
# Example: dotnet run -- gfaydark
```

The application expects converted files in `../ConverterApp/`.

## Development Notes

### Code Style
- Uses C# 7.0+ features (tuples, pattern matching)
- Static imports used extensively (e.g., `using static OpenEQ.Engine.Globals`)
- IronPython for UI scripting

### Key Classes to Understand
- `EngineCore` - Main game loop and rendering
- `Controller` - Coordinates engine and asset loading
- `Loader` - Asset loading logic
- `DeferredPathway` - Rendering pipeline

### Deferred Rendering
The engine uses deferred rendering:
1. G-buffer pass renders geometry to multiple render targets
2. Lighting pass calculates lighting using G-buffer data
3. Final composition

### Python UI Scripting
UI files in `OpenEQ/UI/` can be:
- `.xaml` - Pure XAML UI definitions
- `.py` - Python scripts that can load XAML with bindings

Python scripts have access to:
- `controller` - Controller instance
- `engine` - EngineCore instance
- `bind()` - Helper for event binding
- `loadXaml()` - Load XAML with Python expression bindings

## Future Direction

The master branch is actively developed and represents the "official" version of OpenEQ. Focus areas:

1. **Stability** - Fixing existing bugs and improving performance
2. **Feature Completeness** - Implementing missing rendering features
3. **User Experience** - Better UI and controls
4. **Documentation** - Improving code comments and docs

## Migration Notes

If coming from other branches:
- **From old-xenko**: Master uses Silk.NET instead of Xenko/Stride
- **From godoteq**: Master is pure C# without Godot engine
- **From FileConverter**: Master includes conversion tools but adds rendering

## Dependencies

Major NuGet packages:
- NoesisGUI - UI framework
- Silk.NET - OpenGL bindings
- SixLabors.ImageSharp - Image loading
- IronPython - Python scripting
- System.Numerics - Math library

## Testing

Currently, there is no automated test suite. Testing is manual:
1. Convert test zones
2. Run application
3. Verify rendering and controls
4. Check console for errors

## Performance Considerations

- **Vertex Count**: Large zones have high vertex counts
- **Draw Calls**: Many draw calls per frame
- **Memory**: Textures consume significant memory
- **Frame Time**: Target is 60 FPS but varies by scene complexity

Optimization opportunities:
- Implement frustum culling
- Batch draw calls
- Use instancing for repeated geometry
- Implement LOD system

## Conclusion

The master branch is the most complete and stable version of OpenEQ, suitable for viewing EverQuest zones and models. It has a solid foundation but needs additional features and optimizations to become a full-featured viewer or game engine.
