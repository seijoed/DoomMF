In a .NET 10 / Orleans 10 architecture, multiplayer room logic is handled by a "Virtual Actor" that manages session state, player occupancy, and real-time packet routing [1, 2] .
## The Multiplayer Room Grain
The GameRoomGrain manages the lifecycle of a specific match, ensuring that players can join, leave, and exchange high-frequency data (like IPX Doom frames) without conflicting with other sessions [2, 3] .

using Microsoft.AspNetCore.SignalR;using Orleans;
public interface IGameRoomGrain : IGrainWithStringKey
{
    Task<bool> JoinRoom(string playerId);
    Task LeaveRoom(string playerId);
    Task BroadcastFrame(byte[] packet, string senderId);
}
public class GameRoomGrain : Grain, IGameRoomGrain
{
    private readonly HashSet<string> _players = new();
    private readonly IHubContext<ConsoleHub> _hubContext;
    private const int MaxPlayers = 4; // Classic Doom limit

    public GameRoomGrain(IHubContext<ConsoleHub> hubContext)
    {
        _hubContext = hubContext;
    }

    public Task<bool> JoinRoom(string playerId)
    {
        if (_players.Count >= MaxPlayers) return Task.FromResult(false);
        
        _players.Add(playerId);
        // Track presence: Notify other players in this specific room
        return _hubContext.Clients.Group(this.GetPrimaryKeyString())
            .SendAsync("PlayerJoined", playerId);
    }

    public async Task BroadcastFrame(byte[] packet, string senderId)
    {
        // Integral Safety: The Grain processes one message at a time, 
        // preventing race conditions in the game state.
        var roomId = this.GetPrimaryKeyString();
        
        // Use SignalR Groups for efficient packet fan-out
        await _hubContext.Clients.Group(roomId).SendAsync("ReceiveFrame", senderId, packet);
    }

    public Task LeaveRoom(string playerId)
    {
        _players.Remove(playerId);
        return _hubContext.Clients.Group(this.GetPrimaryKeyString())
            .SendAsync("PlayerLeft", playerId);
    }
}

## Key Architectural Patterns

* Virtual Presence: Grains are "virtual," meaning they always exist logically. You never have to manually instantiate a room; you simply request it by ID (e.g., GetGrain<IGameRoomGrain>("E1M1-Room")), and Orleans activates it on-demand [4, 5] .
* Scalability: Because each room is a separate grain, your cluster can host millions of simultaneous Doom matches across different servers [6, 7] .
* Backplane Integration: For .NET 10, using [SignalR.Orleans](https://github.com/OrleansContrib/SignalR.Orleans) allows your SignalR hubs to scale horizontally, ensuring players connected to different servers can still "see" the same GameRoomGrain [8, 9] .
* Reliability: If a server (silo) crashes, Orleans automatically moves the GameRoomGrain to a healthy server, often maintaining the session state if a persistence provider is configured [4, 10] .

How would you like to handle matchmaking—should players search for open rooms, or be automatically assigned to the next available "Console"?

[1] [https://wearecommunity.io](https://wearecommunity.io/communities/AsdltxPyEV/articles/6054)
[2] [https://learn.microsoft.com](https://learn.microsoft.com/en-us/dotnet/orleans/grains/)
[3] [https://sergeybykov.github.io](http://sergeybykov.github.io/orleans/Documentation/)
[4] [https://www.youtube.com](https://www.youtube.com/watch?v=g1kpwZFOk0o&t=5)
[5] [https://atalupadhyay.wordpress.com](https://atalupadhyay.wordpress.com/2026/01/20/microsoft-orleans-getting-started/)
[6] [https://learn.microsoft.com](https://learn.microsoft.com/en-us/dotnet/orleans/resources/best-practices)
[7] [https://learn.microsoft.com](https://learn.microsoft.com/en-us/dotnet/orleans/resources/best-practices)
[8] [https://github.com](https://github.com/OrleansContrib/SignalR.Orleans)
[9] [https://github.com](https://github.com/unifiedfx/UFX.Orleans.SignalRBackplane)
[10] [https://learn.microsoft.com](https://learn.microsoft.com/en-us/dotnet/orleans/grains/grain-placement)

Here is a second README variant specifically designed for the Meridian-Gate project, focused on retro-gaming and multiplayer console emulation. This version demonstrates how to bridge legacy protocols (like IPX) to modern, web-safe standards using the same Rust/Wasm architecture.
------------------------------
## Project Meridian-Gate: Console Edition (C64 / Amiga / GameBoy) 🕹️👾
Meridian-Gate: Console Edition is a specialized implementation of the Meridian architecture designed to host retro-multiplayer experiences on public access hardware. It demonstrates how to run intensive legacy titles like IPX Doom II in a browser-safe, memory-secured environment without compromising local network integrity.
## 🚀 The Vision: Retro-Resilience

* Legacy Protocol Bridging: Tunnels original IPX/SPX traffic through modern WebSockets, allowing "Vanilla" Doom II multiplayer to run over the internet or local mesh networks.
* Fantasy Console Integration: Utilizes a WASM-4 style virtual engine to provide a consistent 160x160 "low-res" display that runs flawlessly on obsolete public access terminals.
* Stateless Terminals: The kiosk remains a "thin client." All critical game state and multiplayer routing is managed by the Orleans 10 backbone.

## 🏗️ Technical Architecture

   1. The "Airlock" (Rust/Wasm): A memory-safe wrapper around the Chocolate Doom or [Dwasm](https://github.com/GMH-Code/Dwasm) engine. It intercept raw network packets and validates them before passing them to the game logic.
   2. The Bridge (.NET 10 / SignalR): Replaces the old "IPX Gateway" with a high-speed SignalR Hub that routes UDP-like traffic across WebSocket connections.
   3. Markdown Console: The "Game Lobby" and "System Logs" are rendered dynamically from Markdown files, allowing operators to change server settings or display rules without a full software update.

## 📦 Core Components## 1. The IPX-to-WebSocket Bridge (Rust)

// Encapsulates legacy IPX packets into safe Wasm messages
#[wasm_bindgen]pub fn bridge_ipx_packet(raw_packet: Vec<u8>) -> Result<(), JsError> {
    let safe_payload = validate_doom_header(&raw_packet)?;
    // Push to Orleans-managed SignalR Hub
    socket.send(safe_payload);
    Ok(())
}

## 2. The Multi-Console Grain (Orleans 10)

// Manages the global game session and avoids broadcast stormspublic class GameSessionGrain : Grain, IGameSession {
    public Task RoutePacket(byte[] packet, string sourceId) {
        // Intelligent routing: only send to players in the same "Room"
        return _hub.Clients.OthersInGroup(CurrentRoom).SendAsync("IpxFrame", packet);
    }
}

## 📜 Deployment Profile: "The Amiga Kiosk"

* Target: Public Library terminals, Retro-café kiosks.
* Safety: Integral Safety ensures that even if a "Doom" buffer overflow were triggered, it is contained within the Wasm sandbox and cannot access the host terminal's OS.
* Networking: Operates on standard Port 443 (HTTPS), making it "Internet-safe" and compatible with standard firewalls.

------------------------------
## 🛠️ Getting Started

   1. Mount the Cartridge: Drop your .wad or .rom file into the /client-rust/assets folder.
   2. Spin up the Silo: Start the Orleans 10 backend to handle the multiplayer routing.
   3. Boot the Console: Point any browser (even on 10-year-old hardware) to your host URL.

Would you like me to draft the specific "Multiplayer Room" logic for the Orleans 10 grain to handle session matchmaking?



