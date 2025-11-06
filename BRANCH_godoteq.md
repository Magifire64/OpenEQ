# GodotEQ Branch Documentation

## Overview

The godoteq branch is an experimental port of OpenEQ to the Godot Engine (version 3.x). This branch completely reimplements the project using Godot's scene system and C# scripting, with a focus on networking and online gameplay features.

## Branch Status

**Status**: Experimental / Proof of Concept  
**Last Major Update**: Commit 9ccc66a ("Switched to controller/view structure")  
**Divergence from Master**: Complete rewrite - uses Godot Engine instead of custom rendering

## Current Implementation

### Technology Stack
- **Language**: C# (Godot/Mono)
- **Engine**: Godot Engine 3.x
- **Graphics**: Godot's built-in renderer
- **Architecture**: Model-View-Controller (MVC) pattern
- **Networking**: Custom EverQuest protocol implementation

### Project Structure

```
Controllers/            # MVC Controllers
  ├── BaseController.cs
  ├── LoginController.cs
  ├── WorldController.cs
  └── ZoneController.cs
Views/                  # Godot UI Views
  ├── LoginView.cs
  ├── WorldView.cs
  └── ZoneView.cs
NetClient/              # Network protocol implementation
Shared/                 # Shared network code
*.tscn                  # Godot scene files
  ├── Login.tscn
  ├── World.tscn
  └── Zone.tscn
project.godot           # Godot project configuration
OpenEQ.csproj          # C# project file
```

### Key Components

1. **MVC Architecture**
   - **Controllers** - Business logic, network communication
   - **Views** - Godot scene nodes, UI, rendering
   - **BaseController** - Abstract base for controller pattern

2. **Login System** (`Controllers/LoginController.cs`, `Views/LoginView.cs`)
   - Connects to EQ login server
   - Handles authentication
   - Retrieves server list
   - Server selection

3. **World System** (`Controllers/WorldController.cs`, `Views/WorldView.cs`)
   - Character management
   - Character creation/selection
   - Zone transfer initiation

4. **Zone System** (`Controllers/ZoneController.cs`, `Views/ZoneView.cs`)
   - Zone rendering
   - Player positioning
   - NPC/Mob spawning
   - Character animation
   - Camera controls

5. **Networking** (`NetClient/`, `Shared/`)
   - EverQuest network protocol
   - LoginStream - Login server communication
   - ZoneStream - Zone server communication
   - Packet encoding/decoding

6. **Asset Loading**
   - `ZoneReader.cs` - Loads converted zone files
   - `MobModel.cs` - Character/NPC models
   - `TextureLoader.cs` - Texture loading
   - Animation support

7. **Camera** (`FPSCamera.cs`)
   - First-person camera
   - WASD movement
   - Mouse look
   - Fly controls

## Features Implemented

### ✅ Working Features

1. **Login Flow**
   - Connect to login server (default 127.0.0.1:5999)
   - Username/password authentication
   - Server list retrieval
   - Server selection

2. **Networking**
   - EQ protocol implementation
   - Packet handling
   - Keep-alive connections
   - Event-driven architecture

3. **Zone Loading**
   - Load converted zone files
   - Display zone geometry
   - Texture mapping

4. **Character/Mob Rendering**
   - Load character models
   - Skeletal animation playback
   - Position and rotation
   - Animation loops (with "hacky animation looper")

5. **Camera Controls**
   - First-person view
   - WASD movement keys
   - Mouse look (arrow keys)
   - Fly mode (Space/Ctrl)
   - Turbo mode (Shift)

6. **MVC Pattern**
   - Clean separation of concerns
   - Controller/View communication
   - Event-based updates

7. **Godot Integration**
   - Uses Godot scenes (.tscn)
   - C# scripting in Godot
   - Godot's built-in physics (RigidBody for camera)

## Architecture Highlights

### Controller/View Pattern

Each game screen has:
- **Controller**: Manages state, network connection, business logic
- **View**: Godot Node, handles rendering and user input
- **Stream**: Network connection (LoginStream, ZoneStream)

Example flow:
```
LoginView (UI) → LoginController (logic) → LoginStream (network) → Server
```

### Network Event System

Controllers use events to notify views:
```csharp
conn.LoginSuccess += (_, success) => { ... };
conn.ServerList += (_, servers) => { ... };
conn.Spawned += (_, mob) => { ... };
```

### Godot Scene Hierarchy

```
Login.tscn          # Login screen
World.tscn          # Character select
Zone.tscn           # In-game zone
  ├── RigidBody     # Player physics body
  │   └── CameraHolder  # Camera mount point
  └── ZoneNode      # Zone geometry
```

## Known Issues & Bugs

### 🐛 Current Bugs

1. **Animation System**
   - Comment: "Added hacky animation looper"
   - Animation looping is not clean
   - May need proper animation state machine

2. **Network Stability**
   - "Fixed netcode bugs" suggests previous issues
   - May still have edge cases
   - Keep-alive implementation may need testing

3. **Commented Code**
   - Large blocks of commented code in ZoneView.cs
   - Mob instantiation code is disabled
   - Movement updates are not active
   - Suggests incomplete features

4. **Coordinate Conversion**
   - Manual coordinate flipping (Z axis: `* new Vector3(1, 1, -1)`)
   - May have orientation issues

5. **Model Loading**
   - Hardcoded paths (e.g., `@"c:\aaa\projects\openeq\converter\orc_chr.zip"`)
   - Path handling needs improvement

### ⚠️ Limitations

1. **Godot 3.x Dependency**
   - Tied to Godot 3.x (not Godot 4.x)
   - Mono/.NET implementation required
   - May not work with GDScript-only builds

2. **Single Player Focus**
   - Network code present but may not be fully tested
   - No server implementation
   - Requires actual EQ servers to test

3. **Limited UI**
   - Basic login screen
   - Minimal in-game UI
   - No HUD elements

4. **Performance**
   - Godot's rendering may have different performance than custom engine
   - No optimization mentioned

## Not Yet Implemented

### 🚧 Missing Features

1. **Mob System (Partially Implemented)**
   - [ ] Mob spawning (commented out)
   - [ ] Mob movement updates (commented out)
   - [ ] Mob animations
   - [ ] Mob interactions

2. **Character System**
   - [ ] Character creation UI
   - [ ] Character customization
   - [ ] Equipment display
   - [ ] Character stats

3. **World Features**
   - [ ] Multiple zones
   - [ ] Zone transitions
   - [ ] Dynamic loading/unloading

4. **Gameplay**
   - [ ] Combat
   - [ ] NPCs and interactions
   - [ ] Inventory
   - [ ] Skills and spells

5. **UI/UX**
   - [ ] HUD
   - [ ] Chat window
   - [ ] Menus
   - [ ] Settings

6. **Networking**
   - [ ] Complete packet handling
   - [ ] All game commands
   - [ ] Trade, guilds, groups
   - [ ] Zone handoff

7. **Audio**
   - [ ] Sound effects
   - [ ] Music
   - [ ] Ambient audio

## Building and Running

### Prerequisites
- Godot Engine 3.x with Mono support
- .NET SDK compatible with Godot's Mono version
- Visual Studio or compatible IDE (optional)

### Setup Steps

1. **Install Godot Mono**
   ```
   Download from https://godotengine.org/
   Choose Mono version (C# support)
   ```

2. **Open Project**
   ```
   Launch Godot
   Import project from project.godot
   Let Godot build the C# project
   ```

3. **Configure Network**
   - Edit `LoginController.cs` to set server address:
     ```csharp
     public Tuple<string, int> Server { get; set; } 
         = new Tuple<string, int>("127.0.0.1", 5999);
     ```

4. **Run Project**
   - Press F5 in Godot Editor
   - Or use Play button

### Asset Requirements

Zone files need to be converted first:
- Use converter tool to generate zone files
- Place in expected location (check code for paths)

## Development Notes

### Godot Conventions

- Scenes (.tscn) define the structure
- C# scripts attached to nodes
- Use `_Ready()` for initialization
- Use `_Process(delta)` for per-frame updates

### Controller Registration

Views register with controllers:
```csharp
public override void _Ready() {
    Controller.Register(this);
    Controller.Connect();
}
```

### Attributes

Custom attributes used:
```csharp
[ControlGetter] RigidBody RigidBody = null;
[ControlGetter("RigidBody/CameraHolder")] Spatial CameraHolder = null;
```
These likely auto-populate from scene tree.

### Network Configuration

Default server: `127.0.0.1:5999`  
This is a local test server. Change for actual use.

## Comparison with Master Branch

| Feature | Master Branch | GodotEQ Branch |
|---------|--------------|----------------|
| Engine | Custom (Silk.NET) | Godot Engine |
| Language | C# | C# (Godot/Mono) |
| Rendering | OpenGL 4.x | Godot Renderer |
| UI Framework | NoesisGUI | Godot UI |
| Networking | Partial | More complete |
| Architecture | Monolithic | MVC |
| Scene Format | Code-based | .tscn files |
| Asset Loading | OES format | Different format |

## Advantages of Godot Port

1. **Mature Engine**: Godot handles rendering, physics, input
2. **Scene Editor**: Visual scene editing
3. **Cross-platform**: Godot's export system
4. **Community**: Godot community and resources
5. **Easier Development**: Less boilerplate code
6. **Built-in Tools**: Debugger, profiler, etc.

## Disadvantages of Godot Port

1. **Learning Curve**: Need to know Godot
2. **Engine Dependency**: Tied to Godot's update cycle
3. **Limited Control**: Less control over rendering pipeline
4. **Performance**: May not be as optimized for specific use case
5. **Mono Requirement**: Requires Mono Godot build

## Migration Path

### To continue Godot branch:

1. Uncomment mob spawning code
2. Implement remaining network packets
3. Add UI elements
4. Create proper asset pipeline
5. Test with real/private EQ servers

### To port improvements to Master:

1. Extract network protocol improvements
2. Port MVC architecture concepts
3. Update asset formats
4. Keep Godot-specific code separate

## Future Direction

This branch shows promise for:
- Online gameplay (networking is more complete)
- Rapid prototyping with Godot
- Educational purposes (Godot is easier to learn)

However, it needs:
- Completed mob system
- Full UI implementation
- Asset pipeline clarity
- Documentation
- Server software for testing

## Testing

### Manual Testing Checklist

- [ ] Launch application
- [ ] See login screen
- [ ] Enter credentials
- [ ] Connect to server
- [ ] View server list
- [ ] Select server
- [ ] Enter world
- [ ] Load zone
- [ ] Camera controls work
- [ ] Character renders
- [ ] Animations play

### Server Requirements

Requires EverQuest-compatible login and zone servers. Options:
- Private server software (EQEmu, etc.)
- Local test server
- Original EQ servers (not recommended/supported)

## Performance Notes

- Godot's renderer handles most optimization
- Large zones may need LOD or culling
- Animation updates should be efficient
- Network packets are frequent - watch bandwidth

## Conclusion

The godoteq branch represents a bold experiment to use an existing game engine rather than building from scratch. It has a cleaner architecture (MVC) and more complete networking than master, but is clearly unfinished with significant commented-out code. The branch shows potential but would require substantial work to reach feature parity with master or become a playable client.

Best used for:
- Exploring Godot for game dev
- Understanding EQ network protocol
- Experimenting with different architecture
- Learning MVC pattern in game context
