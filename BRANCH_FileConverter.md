# FileConverter Branch Documentation

## Overview

The FileConverter branch is a specialized branch focused on file conversion tools for EverQuest assets. This branch has been stripped down to focus primarily on the conversion pipeline, removing much of the rendering engine code present in the master branch.

## Branch Status

**Status**: Experimental / Tool-focused  
**Last Major Update**: Commit 495d78c ("no message")  
**Divergence from Master**: Significantly different - removes Engine, LegacyFileReader, and other rendering components

## Current Implementation

### Technology Stack
- **Language**: C# (.NET)
- **Focus**: File parsing and conversion
- **Architecture**: Standalone conversion tool

### Project Structure

The FileConverter branch has a simplified structure compared to master:

```
FileConverter/          # Main conversion application
  ├── Entities/         # Data structures for EQ file formats
  ├── Extensions/       # Helper methods
  ├── Wld/              # WLD file format parsers
  ├── Program.cs        # Entry point
OpenEQ/                 # Minimal OpenEQ viewer
converter/              # Additional conversion utilities
openeq.cfg              # Configuration file
```

#### Key Components

1. **FileConverter Application** (`FileConverter/`)
   - Main conversion tool
   - Parses .s3d and .wld files
   - Outputs converted asset files
   - Command-line interface

2. **Entities** (`FileConverter/Entities/`)
   - `Zone.cs` - Zone data representation
   - `Mesh.cs` - Mesh geometry
   - `Material.cs` - Material definitions
   - `CharFile.cs` - Character file data
   - `FragMesh.cs`, `FragRef.cs` - WLD fragment types
   - `VertexBuffer.cs` - Vertex data structures
   - `WorldHeader.cs` - Zone header information
   - `AniTree.cs` - Animation tree structures
   - `SkelPierceTrack.cs`, `SkelPierceTrackSet.cs` - Skeletal animation
   - `LightInfo.cs`, `LightSource.cs` - Lighting data
   - Various WLD fragment structures

3. **Extensions** (`FileConverter/Extensions/`)
   - `BinaryReaderExtensions.cs` - Reading EQ binary formats
   - `BinaryWriterExtensions.cs` - Writing converted formats
   - `StringExtensions.cs` - String manipulation helpers
   - `DictionaryExtensions.cs`, `IEnumerableExtensions.cs` - Collection helpers

4. **WLD Parsers** (`FileConverter/Wld/`)
   - Parsers for WLD file fragments
   - Zone geometry parsing
   - Model and animation parsing
   - Texture reference parsing

## Features Implemented

### ✅ Working Features

1. **Zone Conversion**
   - Reads .s3d archive files
   - Parses zone .wld files
   - Extracts zone geometry
   - Processes textures
   - Exports to intermediate format

2. **File Format Support**
   - .s3d archives (EverQuest archives)
   - .wld files (world/zone definitions)
   - Handles compressed files
   - Decodes WLD fragments

3. **Command-Line Interface**
   ```bash
   FileConverter.exe [Path to EQ] [zonename]
   
   Examples:
   # Convert all zones
   FileConverter.exe E:\Games\EQRoF2\
   
   # Convert single zone
   FileConverter.exe E:\Games\EQRoF2\ mischiefplane
   ```

4. **Batch Processing**
   - Can convert multiple zones at once
   - Filters zone files (ignores _obj, _chr suffixes)
   - Progress reporting

5. **Data Extraction**
   - Zone geometry and meshes
   - Texture references
   - Material definitions
   - Object placements
   - Light sources (basic)
   - Character models (partial)

## Components Removed vs Master

This branch **removes** the following from master:

- ❌ **Engine** - No rendering engine
- ❌ **CollisionManager** - No collision detection
- ❌ **Common/OESFile** - Different output format
- ❌ **ConverterApp/ConverterCore** - Replaced with FileConverter
- ❌ **ImageLib** - Different image handling
- ❌ **LegacyFileReader** - Replaced with WLD parsers in FileConverter
- ❌ **NetClient/Netcode** - No networking
- ❌ **Zlib.Portable** - May use different compression

## Known Issues & Bugs

### 🐛 Current Bugs

1. **Character File Parsing**
   - Comment in code: "Started working on _chr"
   - Character file parsing is incomplete
   - May not handle all character model variations

2. **Incomplete WLD Fragment Support**
   - Not all WLD fragment types are implemented
   - Some zones may fail to convert
   - Some geometry may be missing

3. **Error Handling**
   - Limited error messages
   - May crash on malformed files
   - No validation of input paths

4. **Output Format**
   - Output format is not well documented
   - May not be compatible with master branch OES format
   - No specification of output structure

5. **Texture Handling**
   - Texture extraction may be incomplete
   - No texture format conversion
   - Alpha channels may not be handled correctly

### ⚠️ Limitations

1. **No Rendering**
   - Cannot view converted assets
   - Must use external tools to verify output

2. **No Animation Support**
   - Animation data structures exist but may not be fully processed
   - No animation export

3. **Incomplete Object Types**
   - Some placeable objects may not convert
   - Dynamic objects may be missing
   - Effects and particles not supported

4. **Memory Usage**
   - Large zones may consume significant memory
   - No streaming or progressive loading

## Not Yet Implemented

### 🚧 Missing Features

1. **Character Models**
   - [ ] Complete _chr file parsing
   - [ ] Character texture support
   - [ ] Equipment/armor handling
   - [ ] Character animations

2. **Advanced WLD Features**
   - [ ] All fragment types
   - [ ] Particle effects
   - [ ] Dynamic lighting
   - [ ] Sound references
   - [ ] Region definitions

3. **Output Options**
   - [ ] Multiple output formats
   - [ ] Compression options
   - [ ] LOD generation
   - [ ] Mesh optimization

4. **Validation**
   - [ ] Input file validation
   - [ ] Output verification
   - [ ] Error reporting
   - [ ] Diagnostic tools

5. **Newer Expansion Support**
   - [ ] Support for post-classic expansions
   - [ ] Newer file format versions
   - [ ] Extended features

6. **Performance**
   - [ ] Multi-threaded conversion
   - [ ] Progress bars
   - [ ] Cancellation support
   - [ ] Incremental conversion

## Building and Running

### Prerequisites
- .NET Framework or .NET Core (check .csproj for version)
- Visual Studio or compatible IDE

### Build Steps
```bash
# Build the solution
dotnet build OpenEQ.sln

# Or build FileConverter specifically
dotnet build FileConverter/FileConverter.csproj
```

### Running the Converter

```bash
cd FileConverter
dotnet run -- <path_to_eq_install> [zone_name]

# Example: Convert all zones
dotnet run -- "C:\Games\EverQuest\"

# Example: Convert specific zone
dotnet run -- "C:\Games\EverQuest\" gfaydark
```

### Expected Input
- Path to EverQuest installation directory
- Directory should contain .s3d files
- Zone files typically named: `zonename.s3d`, `zonename_obj.s3d`, `zonename_chr.s3d`, etc.

### Output
Output location and format depend on the implementation. Check the code for specifics.

## Development Notes

### Code Organization

1. **Entity Classes**: Represent EQ file structures directly
2. **Extensions**: Helper methods for binary I/O
3. **WLD Parsers**: State machines for parsing complex WLD fragments

### WLD File Format

WLD files contain "fragments" - chunks of data with:
- Fragment type ID
- Fragment name reference
- Fragment-specific data

Common fragment types:
- 0x03 - Texture names
- 0x04 - Texture references  
- 0x05 - Texture bitmap info
- 0x2D - Mesh definitions
- 0x36 - Mesh references
- And many more...

### Key Classes to Understand

- `Program.cs` - Entry point and conversion orchestration
- `Zone.cs` - Main zone data structure
- `Mesh.cs` - Geometry representation
- Binary extensions - Low-level file reading

## Comparison with Master Branch

| Feature | Master Branch | FileConverter Branch |
|---------|--------------|---------------------|
| Rendering | ✅ Full engine | ❌ None |
| Conversion | ✅ Via ConverterApp | ✅ Main focus |
| File Support | ✅ Via LegacyFileReader | ✅ Custom parsers |
| Networking | ⚠️ Partial | ❌ None |
| UI | ✅ NoesisGUI | ❌ None |
| Output Format | OES (.oes.zip) | Unknown/custom |

## Migration Path

### To use FileConverter output with Master branch:

1. You may need to convert the output format
2. Check output structure compatibility
3. May need to write adapter code
4. Consider using master's ConverterApp instead

### To merge FileConverter improvements into Master:

1. Extract WLD parsing improvements
2. Port to LegacyFileReader
3. Maintain compatibility with OES format
4. Add tests for new features

## Future Direction

This branch appears to be an experiment in simplifying the conversion pipeline. Possible directions:

1. **Merge back to Master**: Port improvements to master branch
2. **Standalone Tool**: Develop as independent conversion utility
3. **Archive**: Keep as historical reference for alternate approach

## Testing

No automated tests present. Manual testing:

1. Run converter on known zones
2. Inspect output files
3. Verify no crashes or errors
4. Check file sizes and structure

Test zones:
- **gfaydark** - Good starter zone
- **qeynos** - Medium complexity
- **mischiefplane** - Complex geometry

## Performance Notes

- Conversion speed depends on zone complexity
- Memory usage scales with zone size
- No progress indicators in current implementation
- Single-threaded processing

## Conclusion

The FileConverter branch represents an experimental approach to asset conversion, stripping away the rendering engine to focus solely on file format parsing and conversion. It may contain valuable WLD parsing code that could be ported back to the master branch, but the branch itself appears incomplete and is missing documentation on its output format and intended use cases.

This branch is best used as:
- Reference for WLD file format parsing
- Inspiration for conversion tool improvements
- Historical record of alternate architecture
