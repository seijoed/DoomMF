# README.md: Project DJ Llama
## 🦙 Project: DJ Mini-Llama## The Open-Source Distributed Intelligence & Music Visualizer
Welcome to Project DJ Llama, an open-source hardware/software experiment that turns a physical USB "Llama" device into a sentient, music-mixing powerhouse. This project is designed to be a "Breadcrumb Project"—meaning the architecture is modular enough that even small LLMs can help you build and extend it.
------------------------------
## 🌟 Vision
DJ Llama isn't just a toy; it’s a Distributed Actor System. It combines the reasoning of Qwen-35B, the safety of Rust, and the scalability of Microsoft Orleans to create a real-time interaction loop between hardware physics and musical intent.
------------------------------
## 🏗️ System Architecture
The project follows a "Brain-Heart-Eyes" design pattern:

   1. The Brain (Cloud/Local LLM): Qwen-35B handles high-level intent. It decides the "vibe" and maps user requests to physical parameters.
   2. The Nervous System (Backend): Microsoft Orleans manages the Llama’s state as a "Virtual Actor" (Grain). Even if the USB is unplugged, the Llama's "soul" stays alive in the actor system.
   3. The Heart (Firmware/Logic): Rust handles the high-speed physics engine and raw USB communication.
   4. The Eyes (Frontend): An HTML5 Canvas renders a 2D physics-based visualizer directly on a Chromebook or any web-enabled device.

------------------------------
## 💻 Tech Stack

| Component | Technology | Role |
|---|---|---|
| LLM | Qwen-35B | Intent extraction, music selection, and "personality" generation. |
| Backend | Microsoft Orleans | State management via distributed virtual actors. |
| Logic/Systems | Rust | High-performance physics engine and memory-safe hardware logic. |
| Frontend | HTML5 Canvas | Real-time 2D visualization of "bouncing data" and waveforms. |
| Interface | USB/Web Serial | Seamless plug-and-play connectivity for Chromebooks. |

------------------------------
## 🎛️ Functionality## 1. AI-Driven Music Mixing
Instead of traditional sliders, the Llama uses "Vibe Parameters."

* Qwen-35B interprets prompts like "Make it feel like a rainy Tuesday in Stockholm" and translates them into BPM, EQ, and transition styles.
* Orleans Grains ensure that your mix progress is saved across sessions.

## 2. Physics-Based Visualizer
The Rust-powered physics engine calculates the trajectory of "Pure Light" particles.

* Gravity corresponds to bass intensity.
* Bounciness corresponds to treble/high-frequency energy.
* Canvas renders these calculations at 60FPS to provide a zero-latency visual representation of the music.

------------------------------
## 🍞 The "Breadcrumbs" (LLM Integration Guide)
This project is designed to be built collaboratively with AI. You can use the following "breadcrumbs" to prompt any LLM to help you write the code:

* For the Rust Logic: "Generate a 2D physics engine in Rust that calculates elastic collisions for 50 circles within a 800x600 coordinate space."
* For the Orleans Actor: "Write a Microsoft Orleans Grain interface for a 'DJ State' that tracks current BPM, track ID, and a 'happiness' float value."
* For the Canvas UI: "Create a Javascript function to draw circles on an HTML5 Canvas based on a stream of X/Y coordinates received via Web Serial."

------------------------------
## 🚀 Getting Started

   1. Clone the Repo: git clone https://github.com/your-username/dj-mini-llama
   2. Install Rust: Ensure you have cargo installed for the physics module.
   3. Spin up Orleans: Use the provided Docker Compose file to start the Silo and Gateway.
   4. Plug in the Llama: Connect your USB device and open index.html on your Chromebook.

------------------------------
## 🤝 Contributing
This is a School-Friendly Open Source Project. Whether you're fixing a bug in the physics engine or teaching the Llama new slang via Qwen, your PRs are welcome!
“In the stream of pure light, everyone is an engineer.”
------------------------------
License: MIT

