# OpenEQ

OpenEQ is an open-source project aimed at creating a viewer and tools for exploring EverQuest game assets and zones. The project includes multiple experimental branches using different game engines and approaches.

## Overview

This project reads and renders EverQuest game files, including zones, models, textures, and other assets from the original EverQuest game. It is primarily an educational and preservation project for exploring classic EverQuest content.

## Project Structure

The repository contains several major components:

### Core Libraries
- **Engine** - Core rendering engine (OpenGL/Silk.NET based)
- **LegacyFileReader** - Parses legacy EverQuest file formats (.s3d, .wld, etc.)
- **Common** - Shared utilities and OES file format support
- **ImageLib** - Image loading and texture handling
- **Zlib.Portable** - Compression/decompression for EQ archives

### Tools
- **ConverterApp** - Command-line tool for converting EQ assets
- **ConverterCore** - Core conversion logic
- **OesDumper** - Tool for dumping OES file contents

### Networking (Experimental)
- **Netcode** - Network protocol implementation
- **NetClient** - Client-side networking

### Utilities
- **CollisionManager** - Collision detection and spatial partitioning

## Branch Overview

This repository contains multiple branches representing different approaches and experimental features:

| Branch | Status | Description |
|--------|--------|-------------|
| **master** | Active | Main development branch using Silk.NET for rendering |
| **FileConverter** | Experimental | Focused on file conversion tools |
| **godoteq** | Experimental | Port to Godot Engine |
| **old-xenko** | Archived | Previous implementation using Xenko/Stride engine |
| **feature/Dae-TestPass** | Experimental | DAE file format testing and improvements |
| **dependabot/...** | Maintenance | Automated dependency updates |

### Branch Details

For detailed information about each branch, including implementation details, current status, known issues, and remaining work, see the following documents:

- [Master Branch Documentation](BRANCH_master.md) - Main development branch
- [FileConverter Branch Documentation](BRANCH_FileConverter.md) - File conversion tools
- [GodotEQ Branch Documentation](BRANCH_godoteq.md) - Godot Engine port
- [Old-Xenko Branch Documentation](BRANCH_old-xenko.md) - Legacy Xenko implementation
- [Feature/Dae-TestPass Branch Documentation](BRANCH_feature-Dae-TestPass.md) - DAE testing
- [Dependabot Branch Documentation](BRANCH_dependabot.md) - Dependency updates

## Building the Project

### Prerequisites
- .NET SDK (version varies by branch - see individual branch documentation)
- Visual Studio or compatible IDE (recommended)

### Master Branch
```bash
dotnet restore
dotnet build
```

### Running
```bash
cd OpenEQ
dotnet run -- <path_to_zone>
```

## Technologies Used

- **C#/.NET** - Primary development language
- **OpenGL** - Graphics rendering (via Silk.NET or OpenTK)
- **Silk.NET** (master) - Modern .NET OpenGL bindings
- **OpenTK** (some branches) - OpenGL bindings for .NET
- **NoesisGUI** (master) - UI framework
- **Godot Engine** (godoteq branch) - Alternative game engine
- **Xenko/Stride** (old-xenko branch) - Previous game engine choice

## Project Goals

1. **Asset Viewing** - View and explore EverQuest zones and models
2. **File Conversion** - Convert legacy EQ formats to modern formats
3. **Preservation** - Document and preserve EverQuest file formats
4. **Education** - Learn about game engine architecture and 3D rendering

## File Formats Supported

- **.s3d** - EverQuest archive files
- **.wld** - World geometry and object definitions
- **.oes** - Custom OpenEQ storage format
- **.dae** - COLLADA 3D model format (experimental)
- Various texture formats

## Current State

The project is in active development with experimental features across multiple branches. The master branch is the most stable and actively maintained. Other branches represent different architectural experiments and engine ports.

## Contributing

This appears to be a personal project in development. Check individual branch documentation for specific areas that need work.

## Additional Documentation

### Technical Documentation

- **Network Protocol**: See [Netcode/netdocs.md](Netcode/netdocs.md) - Detailed EverQuest network protocol documentation
- **Zone File Format**: See [converter/zonefmt.md](converter/zonefmt.md) - OES zone file format specification
- **Character File Format**: See [converter/charfmt.md](converter/charfmt.md) - OES character file format specification

### Existing Documentation

The repository also contains:
- Network protocol documentation in `Netcode/` directory
- File format specifications in `converter/` directory
- These provide low-level technical details about EQ protocols and custom file formats

## Legal Notice

This project is for educational and preservation purposes. EverQuest is a trademark of Daybreak Game Company LLC. This project is not affiliated with or endorsed by Daybreak Game Company.

## License

Please check the repository for license information.
