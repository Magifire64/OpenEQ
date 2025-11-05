# OpenEQ Documentation Index

This file provides a complete index of all documentation available in the OpenEQ repository.

## Overview Documents

### Main README
📄 [README.md](README.md) - Start here!
- Project overview
- Technology stack
- Branch comparison table
- Quick start guide
- Build instructions

## Branch Documentation

Each branch has detailed documentation covering:
- Current implementation and architecture
- Working features (✅)
- Known bugs and issues (🐛)
- Not yet implemented features (🚧)
- Building and running instructions
- Migration notes

### Active Development

📄 [BRANCH_master.md](BRANCH_master.md) **(~8.8KB)**
- **Status**: Active development
- **Engine**: Silk.NET/OpenGL
- **UI**: NoesisGUI
- Main branch with custom rendering engine

### Experimental Branches

📄 [BRANCH_FileConverter.md](BRANCH_FileConverter.md) **(~9.9KB)**
- **Status**: Experimental
- **Focus**: File conversion tools
- Stripped-down branch focused on parsing and converting EQ files

📄 [BRANCH_godoteq.md](BRANCH_godoteq.md) **(~11.3KB)**
- **Status**: Experimental
- **Engine**: Godot Engine 3.x
- MVC architecture with improved networking

📄 [BRANCH_feature-Dae-TestPass.md](BRANCH_feature-Dae-TestPass.md) **(~11.6KB)**
- **Status**: Experimental
- **Base**: old-xenko + improvements
- Includes PacketRipper analysis tool

### Archived Branches

📄 [BRANCH_old-xenko.md](BRANCH_old-xenko.md) **(~11.6KB)**
- **Status**: Archived
- **Engine**: Xenko/Stride
- Previous implementation, now deprecated

### Maintenance Branches

📄 [BRANCH_dependabot.md](BRANCH_dependabot.md) **(~8.7KB)**
- **Status**: Pending review
- **Type**: Automated security update
- ImageSharp 1.x → 2.x upgrade (RECOMMENDED TO MERGE)

## Technical Documentation

### Network Protocol
📄 [Netcode/netdocs.md](Netcode/netdocs.md)
- UDP protocol specification
- Packet encoding/decoding
- Session management
- Compression and CRC

📄 [Netcode/zaelanet.md](Netcode/zaelanet.md)
- Additional network protocol details

### File Formats
📄 [converter/zonefmt.md](converter/zonefmt.md)
- OES zone file format specification
- Data structures
- Material and mesh definitions
- Placeable objects and lights

📄 [converter/charfmt.md](converter/charfmt.md)
- OES character file format specification
- Model and animation data

## Documentation Summary

| Document | Size | Status | Audience |
|----------|------|--------|----------|
| README.md | 4.4KB | Current | Everyone |
| BRANCH_master.md | 8.8KB | Current | Developers |
| BRANCH_FileConverter.md | 9.9KB | Reference | Tool developers |
| BRANCH_godoteq.md | 11.3KB | Reference | Godot devs |
| BRANCH_old-xenko.md | 11.6KB | Historical | Researchers |
| BRANCH_feature-Dae-TestPass.md | 11.6KB | Reference | Network devs |
| BRANCH_dependabot.md | 8.7KB | Action needed | Maintainers |
| Netcode/netdocs.md | N/A | Current | Protocol devs |
| converter/zonefmt.md | N/A | Current | Format devs |
| converter/charfmt.md | N/A | Current | Format devs |

**Total Documentation**: ~72KB of branch documentation + technical specs

## How to Use This Documentation

### If you're new to OpenEQ
1. Read [README.md](README.md) first
2. Read [BRANCH_master.md](BRANCH_master.md) for current implementation
3. Check technical docs if working with specific features

### If you're a developer
1. Read the branch doc for your work area
2. Check technical specs for low-level details
3. Reference other branches for alternative approaches

### If you're a maintainer
1. Review [BRANCH_dependabot.md](BRANCH_dependabot.md) - action needed!
2. Check all branch docs for status updates
3. Use as reference for architectural decisions

### If you're researching EQ file formats
1. Start with [converter/zonefmt.md](converter/zonefmt.md)
2. Check [converter/charfmt.md](converter/charfmt.md)
3. Review converter code in various branches

### If you're working on networking
1. Read [Netcode/netdocs.md](Netcode/netdocs.md)
2. Check [BRANCH_feature-Dae-TestPass.md](BRANCH_feature-Dae-TestPass.md) for PacketRipper
3. Review [BRANCH_godoteq.md](BRANCH_godoteq.md) for working implementation

## Documentation Goals

This documentation aims to:
- ✅ Explain what each branch does
- ✅ Document current implementation status
- ✅ List known bugs and issues
- ✅ Identify incomplete features
- ✅ Provide build instructions
- ✅ Enable informed decision-making
- ✅ Preserve project knowledge

## Contributing to Documentation

When updating code, please also update relevant documentation:

1. **Code changes**: Update the corresponding BRANCH_*.md
2. **New features**: Add to "Features Implemented" section
3. **Bug fixes**: Update "Known Issues" section
4. **API changes**: Update technical docs
5. **Breaking changes**: Document migration path

## Documentation Maintenance

Last updated: 2025-11-05

Maintained by: GitHub Copilot (initial creation)

Please keep documentation up-to-date as the project evolves!
