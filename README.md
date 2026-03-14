# Particles

A professional 3D particle swarm simulation platform built with Three.js, featuring audio reactivity, hand gesture control, and AI-powered particle behavior generation.

![GitHub stars](https://img.shields.io/github/stars/manojxshrestha/particles)
![GitHub license](https://img.shields.io/github/license/manojxshrestha/particles)
![Three.js](https://img.shields.io/badge/Three.js-0.160.0-blue)
![Firebase](https://img.shields.io/badge/Firebase-10.7.1-orange)

## Features

### Core Simulation
- **50,000+ Particles** - High-performance GPU-accelerated particle rendering
- **8 Visual Styles** - Spark, Plasma, Ink, Paint, Steel, Glass, Vector, Cyber
- **4 Built-in Shapes** - Sphere, Cube, Helix, Donut (Torus)
- **Real-time Controls** - Particle count, speed, glow intensity

### Media Integration
- **Image to Particles** - Convert images to particle formations
- **Video to Particles** - Real-time video particle mapping
- **Audio to Particles** - Audio reactive particle system with folder playlist support
- **3D Model Import** - GLB, OBJ, PDB formats with surface sampling
- **Blueprint/CAD** - Edge detection for architectural particle designs

### Audio Reactivity
- **Folder Support** - Upload entire music folders for playlist playback
- **Frequency Analysis** - Bass, mid, treble analysis
- **Sensitivity Control** - Adjustable reactivity (0.5x - 5x)
- **Visual Sync** - Particles pulse and scale with music beats

### Controls
- **Hand Gesture Control** - MediaPipe-powered hand tracking
  - One finger: Rotate view
  - Open palm: Zoom
  - Peace sign: Speed control
- **Orbit Controls** - Mouse drag to rotate, scroll to zoom
- **Auto-rotate** - Toggle automatic scene rotation
- **Fullscreen** - Immersive viewing mode
- **Keyboard Shortcuts** - Space, F, R, 1-4

## Quick Start

### Running Locally

```bash
# Clone the repository
git clone https://github.com/manojxshrestha/particles.git
cd particles

# Start local server (Python)
python3 -m http.server 8080

# Or using Node.js
npx serve .
```

Open **http://localhost:8080** in your browser.

### Requirements
- Modern browser with WebGL support
- Internet connection (for CDN dependencies)
- Optional: Firebase account for community features

## Project Structure

```
particles/
├── index.html          # Main HTML file with UI
├── main.js             # Core Three.js application
├── style.css           # Main styles
├── favicon.svg         # App icon
├── README.md           # Documentation
├── css/
│   ├── drawing-pad.css # Drawing pad UI
│   └── export-ui.css   # Export modal styles
└── js/
    ├── drawing-pad.js  # Canvas drawing tool
    └── export-manager.js # Code export functionality
```

## Controls Reference

### Sidebar Controls

| Control | Function |
|---------|----------|
| Particle Count | 5,000 - 50,000 particles |
| Sim Speed | 0.1x - 2.0x playback speed |
| Glow Intensity | Bloom effect strength |
| Visual Style | 8 rendering modes |
| Sensitivity | Audio reactivity strength |
| Upload Audio | Single audio file |
| Audio Folder | Entire music folder playlist |
| Upload Image | Convert to particles |
| Upload Video | Map video to particles |
| Upload 3D Model | Sample mesh surface |

### Toolbar

| Button | Action |
|--------|--------|
| ▶/⏸ | Play/Pause audio |
| ⏮/⏭ | Previous/Next track |
| 🔄 | Toggle auto-rotate |
| ✋ | Toggle hand gesture control |
| ⛶ | Fullscreen mode |
| ℹ️ | Toggle info panel |
| 💡 | Open AI prompt guide |
| ⬇️ | Export code |

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| Space | Play/Pause |
| F | Fullscreen |
| R | Reset view |
| 1-4 | Switch shapes |

## Audio Reactivity

The particle system responds to audio frequencies:

- **Bass (20-250Hz)** → Particle scale pulse + bloom intensity
- **Mid (250-2000Hz)** → Color brightness boost
- **Treble (2000-20000Hz)** → Particle sparkle

### Enabling Audio Reactivity

1. Upload audio file or folder (MP3/WAV/OGG)
2. Click anywhere on page (required for AudioContext)
3. Audio plays automatically
4. Adjust "Sensitivity" slider if needed
5. Toggle "Audio React" checkbox to enable/disable

## Custom Particle Code

Write custom particle behaviors using JavaScript:

```javascript
// Available variables (read-only)
i          // Current particle index (0 to count-1)
count      // Total particle count
target     // THREE.Vector3 (WRITE-ONLY) - set position
color      // THREE.Color (WRITE-ONLY) - set color
time       // Simulation time in seconds
THREE      // Full Three.js library

// Helper functions
addControl(id, label, min, max, initialValue) // Create UI slider
setInfo(title, description)                    // Update HUD (i===0 only)
annotate(id, position, label)                 // Add 3D label (i===0 only)

// Example: Spiral
const angle = i * 0.1 + time;
target.set(Math.cos(angle) * 50, Math.sin(angle) * 50, i * 0.05);
color.setHSL(i / count, 1.0, 0.5);
```

### Security Restrictions

Forbidden patterns (code will be rejected):
- `document`, `window`, `fetch`, `XMLHttpRequest`
- `eval`, `Function`, `import`, `require`
- `setTimeout`, `setInterval`, `alert`
- `localStorage`, `sessionStorage`

## Architecture

### Rendering Pipeline

```
1. initThree()        → Setup scene, camera, renderer
2. createParticles()  → Create BufferGeometry with 50k points
3. setShape()         → Define target positions for each particle
4. animate()          → Per-frame loop
   ├── updateAudioData()    → Extract bass/mid/treble
   ├── updatePositions()    → Lerp current to target
   ├── applyAudioReactive() → Scale/color with audio
   └── composer.render()    → Bloom post-processing
```

### Key Classes

| Class | Purpose |
|-------|---------|
| `STATE` | Global state (mode, speed, render style) |
| `CONFIG` | Particle configuration (count, size) |
| `AUDIO_STATE` | Audio reactivity state |
| `positions` | Current/target particle positions |

### Performance Optimizations

- **BufferGeometry** - Single draw call for all particles
- **Object Pooling** - Reuse Vector3 objects
- **Lerp Smoothing** - Smooth position transitions
- **InstancedMesh** - Efficient matrix updates
- **FPS Target** - 60fps with 50k particles

## Dependencies

- **Three.js** (0.160.0) - 3D rendering
- **Firebase** (10.7.1) - Community cloud storage
- **MediaPipe** - Hand gesture recognition
- **Post-processing** - Bloom effects

## Browser Support

| Browser | Support |
|---------|---------|
| Chrome 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 14+ | ✅ Full |
| Edge 90+ | ✅ Full |

## Troubleshooting

### Particles not showing
- Check browser console for errors
- Ensure WebGL is enabled
- Try reducing particle count

### Audio not reactive
- Click anywhere on page first (required for AudioContext)
- Check Audio React checkbox is enabled
- Verify audio file format is supported
- Adjust Sensitivity slider higher

### Hand gestures not working
- Allow camera permissions
- Ensure good lighting
- Keep hand visible in frame

## License

MIT License

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Credits

- **Original Project**: [particles.casberry.in](https://particles.casberry.in/) - The inspiration for this project
- [Three.js](https://threejs.org/) Community
- [MediaPipe](https://google.github.io/mediapipe/) by Google
- [Firebase](https://firebase.google.com/) by Google
- [MediaPipe](https://google.github.io/mediapipe/) by Google
- [Firebase](https://firebase.google.com/) by Google