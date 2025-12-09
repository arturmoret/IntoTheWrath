# INTO THE WRATH

INTO THE WRATH is a first-person 3D escape room set on a Victorian whaling ship, built with a custom C++ / OpenGL 4.6 engine as a university project.

The game focuses on exploration, atmosphere, and puzzle-solving, with a stylized monochrome visual look inspired by Return of the Obra Dinn.

Status: Work-in-progress preview.  
The final version (full puzzle flow and ending scenes) is planned for completion by Friday.

---

## Overview

You wake up alone on a ship trapped in a storm, surrounded by mysterious symbols, locked doors, and strange machinery.  
Your goal is to explore the different decks, solve interconnected puzzles, and ultimately escape the wrath that haunts the vessel.

The project combines:

- A custom rendering pipeline in modern OpenGL 4.6  
- A monochrome, dithered visual style with outline-based post-processing  
- A first-person interaction system with multiple puzzles and a minigame

---

## Screenshots

(These are example paths. Place your image files in the images/ folder as described below and update names if needed.)

![Main Cabin](images/01_main_cabin.png)  
![Deck – Obra Dinn Style](images/02_deck_obradinn.png)  
![Matapatos Minigame](images/03_matapatos_minigame.png)  
![Lever Puzzle Map](images/04_lever_map.png)

---

## Key Features

### Visual Style and Rendering

- Custom monochrome shading inspired by Return of the Obra Dinn  
- Ordered dithering using a Bayer matrix to simulate 1-bit style shading  
- Optional gamma mapping and adjustable luminance thresholds  
- Sobel edge detection pass applied on a mask to draw clean black outlines only on objects (not on the skybox)  
- Separated passes for:
  - Main scene rendering  
  - Mask rendering (for Sobel)  
  - Fullscreen quad post-processing

This results in a clear, stylized look where interactive objects read well even in a heavily stylized scene.

---

### Gameplay and Puzzles

- First-person exploration across multiple ship decks  
- Interactive objects: keys, levers, maps, candles, and more  
- Puzzle chain involving:
  - A symbolic painting/chest puzzle (quadres simbolics)  
  - A lever-map puzzle where you must interpret hints to pull levers in the correct order  
  - Key-based progression and item gating  

- Minigame – Matapatos:
  - Duck-shooting style minigame embedded in the escape room flow  
  - Shooting mechanics with cooldown/delay between shots  
  - Success/failure feedback through animations and hand gestures

- Multiple endings planned (win/lose), including:
  - Escaping successfully  
  - Failing and being devoured by the sea in a Moby Dick–style outcome

---

### Technical Features

- Custom C++ engine using:
  - OpenGL 4.6  
  - GLFW (window and input)  
  - GLM (math)  
  - ImGui (debug UI and HUD overlay)

- Game systems:
  - First-person controller (movement and camera)
  - Object interaction and ray-based use logic
  - HUD and on-screen prompts
  - Game state management (exploration, minigames, cutscenes, endings)

- Post-processing architecture:
  - Scene rendered to the backbuffer
  - Mask rendered to a framebuffer object (FBO)
  - Fullscreen quad with Sobel filter sampling the mask texture
  - Compositing of outlines over the base scene

- Debug and tuning UI via ImGui:
  - Toggles for Obra Dinn-style rendering
  - Dithering strength and luminance threshold sliders
  - Gamma mapping toggle
  - Sobel on/off and strength control
  - Teleport/debug keys for quickly testing different ship areas

---

## Tech Stack

- Language: C++  
- Graphics API: OpenGL 4.6  
- Libraries and tools:
  - GLFW – windowing and input
  - GLM – math (vectors, matrices)
  - ImGui – in-game debug interface
  - STL and standard C++ features

- IDE and platform:
  - Visual Studio 2022
  - Windows (x64)

---

## Controls (Preview)

These are the default controls in the current preview build.

- W / A / S / D – Move  
- Mouse – Look around  
- E – Interact / Use object  
- F – Toggle lantern / flashlight  
- Esc – Pause or exit to menu (if implemented)  

Some controls may be extended or adjusted as the project is finalized.

---

## Project Structure (Simplified)

```text
into-the-wrath/
├─ src/
│  ├─ main.cpp
│  ├─ ... (game logic, rendering, interaction, puzzles)
├─ shaders/
│  ├─ obra_dinn.fs
│  ├─ sobel.fs
│  ├─ ...
├─ images/
│  ├─ 01_main_cabin.png
│  ├─ 02_deck_obradinn.png
│  ├─ 03_matapatos_minigame.png
│  ├─ 04_lever_map.png
├─ assets/        (optional: models, textures, etc.)
├─ README.md
└─ ...
```

Note: The exact structure may vary; this is a simplified overview for readers.

---

## Building and Running

1. Clone the repository  
   ```bash
   git clone https://github.com/<your-username>/into-the-wrath.git
   ```

2. Open the solution  
   - Open the .sln file in Visual Studio 2022.

3. Select configuration  
   - x64  
   - Debug or Release

4. Build and run  
   - Press F5 or use Build → Build Solution, then Start Debugging.

External libraries (GLFW, GLM, ImGui, etc.) are already integrated in the project structure used for the university course.  
If any library paths are missing, they can be adjusted in the Visual Studio project properties.

---

## Status and Future Work

This is currently a preview build created primarily to showcase:

- Custom rendering work (dithering and Sobel)
- First-person interaction
- Core puzzle and minigame systems

Planned improvements:

- Final puzzle sequence and polished flow from start to ending(s)
- More feedback and polish on interactions and UI
- Additional visual tweaks and performance refinements

---

## Contact

Author: Artur Moret  
Role: Computer Engineering student (AI and Programming specialization)  
Location: Barcelona, Spain

Feel free to reach out for more details about the project, technical implementation, or collaboration opportunities.
