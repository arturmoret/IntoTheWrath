# INTO THE WRATH - Custom C++ & OpenGL Engine

- **Project Type:** 3D First-Person Adventure / Custom Game Engine
- **Language:** C++17
- **Graphics API:** OpenGL 4.6
- **Dependencies:** GLFW, GLM, ImGui

## Project Overview

**Into the Wrath** is a first-person 3D escape room developed as a comprehensive graphics programming project. Unlike projects built on commercial engines, this game is built upon a **custom rendering engine** written from scratch in C++.

The project aims to simulate the high-contrast aesthetic of early Macintosh displays and the dithering style of *Return of the Obra Dinn* within a modern OpenGL pipeline. It features a complete gameplay loop including exploration, inventory management, environmental puzzles, and minigames, all set aboard a dark, atmospheric whaling ship.

<p align="center">
  <img src="https://github.com/user-attachments/assets/aa8fed7a-9457-4cc9-b8af-dd7b225b6203" width="100%" alt="Into the Wrath Main Deck">
  <br>
  <em>Figure 1: Exterior view of the ship demonstrating the 1-bit monochrome aesthetic and custom ocean rendering.</em>
</p>

## Rendering Architecture & Visual Pipeline

The visual identity of *Into the Wrath* relies on a strictly controlled post-processing pipeline. The engine uses a multi-pass approach to achieve a stylized 1-bit monochrome look.

### 1. High-Contrast Monochrome Shading
The scene is initially rendered focusing on luminance. A global thresholding algorithm processes the frame, compressing the color range into binary tones (black and white). This is controlled via adjustable uniforms for threshold and gamma mapping, allowing for dynamic lighting adjustments depending on the scene's mood.

### 2. Ordered Dithering (Bayer Matrix)
To emulate retro rendering, the engine applies an ordered dithering filter using a **4x4 Bayer Matrix**.
* The matrix pattern is mapped based on screen coordinates (`gl_FragCoord`).
* This replaces standard gradients with texture-based noise, simulating depth and shadow without greyscale.
* Dithering intensity is variable, allowing the engine to transition between a cleaner look and a heavy "lo-fi" aesthetic.

### 3. Sobel Edge Detection (Masked)
To ensure geometry remains readable against the noisy dithered background, a Sobel operator is applied to specific objects.
* **The Challenge:** Applying Sobel to the final image would outline the dithering noise itself.
* **The Solution:** The pipeline renders a simplified "mask" of the scene (white objects on black background) to a separate Framebuffer Object (FBO). The Sobel pass runs on this mask to generate clean edges.
* **Composition:** The resulting edge texture is blended back onto the main scene, creating crisp outlines around silhouettes while ignoring texture noise.

### 4. Atmospheric Effects
* **Vertex Animation:** The ocean is rendered as a high-density plane with vertex displacement shaders to simulate waves.
* **Lighting:** The engine uses localized point lights to create pockets of visibility in the dark environment, specifically tuned to interact with the threshold parameters.

## Engine Architecture & Systems

The codebase is structured to separate low-level OpenGL abstraction from high-level gameplay logic.

### Core Systems
* **Custom Render Loop:** Manages shader switching, FBO binding, and multipass rendering.
* **Entity System:** Object-oriented structure for handling 3D models, transforms, and collision triggers.
* **State Management:** A finite state machine (FSM) handles the transition between different gameplay modes (Exploration, Puzzle Interface, Minigame, Game Over).
* **Debug Tools:** Extensive integration of **ImGui** allows for runtime modification of shader parameters (Sobel strength, dithering matrix size, light intensity) and simplified level navigation during development.

### 2D/3D Hybrid Integration
A unique technical feature is the integration of 2D distinct elements within the 3D world, inspired by the surrealist style of *Hylics*.
* **Hand Animation System:** The first-person hands are not 3D meshes but 2D sprite sheets rendered on a quad in Normalized Device Coordinates (NDC). This ensures they always render on top of the geometry while maintaining correct aspect ratios. UV offsets are updated dynamically to play animations (clapping, holding keys, lighting candles).
* **UI Overlays:** Interfaces for maps and puzzles are rendered as overlays that pause the 3D update loop while keeping the rendering loop active.

<p align="center">
  <img src="https://github.com/user-attachments/assets/33366c8c-4aee-4911-b308-3769c714a67d" width="45%" alt="Axe Inspection">
  <img src="https://github.com/user-attachments/assets/aba22030-5129-4576-9123-c14040d374dd" width="45%" alt="Matapatos Minigame">
  <br>
  <em>Left: Object inspection system | Right: "Matapatos" shooting minigame</em>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/f3f0aadd-0525-4267-b555-0aa933ea205f" width="45%" alt="Candle Lighting">
  <br>
  <em>Dynamic lighting interaction with 2D sprite animations</em>
</p>


## Gameplay Implementation

The project implements several distinct gameplay mechanics, demonstrating the engine's flexibility:

* **Inventory & State:** An internal state manager tracks collected items (keys, tools) and validates interaction queries (e.g., checking if the player holds a key before playing the "Unlock Door" animation).
* **The Lever Puzzle:** An environmental puzzle where the player must correlate a 2D texture map overlay with 3D physical objects (levers) in the scene.
* **"Matapatos" Minigame:** A completely separate game mode implemented within the engine. It features custom logic for target movement, collision detection (raycasting from the center of the screen), and win/lose conditions that trigger specific hand animations.
* **Symbolic Logic:** Puzzles that require the player to observe textures (paintings) and input codes into chest actors.

## Content & Level Design

The game takes place on a multi-deck ship. The level design is vertical and interconnected:
* **Exterior:** Main deck with skybox and ocean rendering.
* **Interiors:** Corridors and rooms designed to maximize the claustrophobic effect of the dithering shader.
* **Narrative Flow:** The game features two distinct endings (Escape or "The Whale"), triggered by the player's efficiency and puzzle completion status.

## Build Instructions

**Prerequisites:**
* Visual Studio 2019 or newer (Windows)
* OpenGL 4.6 compatible Graphics Card

**Steps:**
1. Clone the repository.
2. Open `IntoTheWrath.sln`.
3. Ensure the `lib` folder contains the static libraries for GLFW and GLM (included in the repo).
4. Select **x64** and **Release** configuration.
5. Build and Run.

---

**Developed by Artur Moret González**
