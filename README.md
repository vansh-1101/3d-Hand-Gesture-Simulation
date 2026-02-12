# 3D Hand Gesture Simulation

✋ An interactive 3D particle visualization system controlled by real-time hand gestures using AI-powered hand tracking.

### 🔗 Live Demo
🚀 [Try it here](https://vansh-1101.github.io/3d-Hand-Gesture-Simulation/)

## 🎯 Features

- **Real-time Hand Tracking**: Uses MediaPipe and TensorFlow to detect hand position and gestures from webcam
- **Interactive 3D Particles**: 15,000 particles rendered with Three.js with dynamic physics
- **Gesture Controls**:
  - **Hand Position**: Move your hand to rotate the 3D shape
  - **Pinch Gesture**: Pinch your thumb and index finger to expand/contract particles
- **Multiple Shapes**:
  - Sphere - Classic particle sphere
  - Cube - 3D cube formation
  - Saturn - Ring formation with central sphere
  - Galaxy - Spiral galaxy pattern
- **Visual Effects**: Additive blending, dynamic colors based on distance, smooth morphing animations
- **Performance**: Optimized with 60 FPS+ rendering on modern browsers

## 🚀 Getting Started

### Prerequisites
- Modern web browser with WebGL support (Chrome, Firefox, Edge, Safari)
- Webcam access (HTTPS connection recommended for production)

### Running Locally

1. **Clone the repository**:
   ```bash
   git clone https://github.com/vansh-1101/3d-Hand-Gesture-Simulation.git
   cd 3d-Hand-Gesture-Simulation
   ```

2. **Open in browser**:
   - Double-click `index.html` to open locally, OR
   - Use a local server (recommended for security):
     ```bash
     python -m http.server 8000
     # Then visit http://localhost:8000
     ```

3. **Allow Camera Access**: Grant webcam permission when the browser prompts

4. **Interact**: Move your hand in front of the camera to control the particles!

## 🎮 Usage

### Controls

| Action | Effect |
|--------|--------|
| **Move Hand** | Rotates the particle structure in X and Y axis |
| **Pinch (Thumb + Index)** | Expands or contracts all particles |
| **Shape Buttons** | Switch between Sphere, Cube, Saturn, and Galaxy patterns |

### Loading

The application shows a loading screen with progress indicator:
- Graphics initialization
- Camera device request
- AI model loading (MediaPipe Hands)

Once ready, the UI shows "ACTIVE" with pinch distance tracking.

## 🛠️ Technical Architecture

### Core Technologies

1. **Three.js** - 3D rendering engine
   - WebGL-based rendering
   - Additive blending for glow effect
   - BufferGeometry for efficient particle rendering

2. **TensorFlow.js** - Machine learning framework
   - Handles model inference
   - WebGL backend for GPU acceleration

3. **MediaPipe Hands** - Hand detection & tracking
   - Real-time 21-point hand landmark detection
   - ~100ms latency
   - Supports single-hand tracking

4. **Vanilla JavaScript** - No frameworks, pure DOM manipulation

### Algorithm Overview

```
┌─────────────────┐
│  Camera Frame   │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────┐
│  MediaPipe Hand Detection   │
│  (21 hand landmarks)        │
└────────┬────────────────────┘
         │
         ▼
┌──────────────────────────┐
│  Extract Hand Data       │
│  - Position (x, y)       │
│  - Pinch Distance        │
└────────┬─────────────────┘
         │
         ▼
┌──────────────────────────────────┐
│  Particle Physics Engine         │
│  1. Base Positions (morph)       │
│  2. Target Positions (shape)     │
│  3. Render Positions (visual)    │
└────────┬─────────────────────────┘
         │
         ▼
┌──────────────────────────┐
│  Three.js Rendering      │
│  - Apply rotation        │
│  - Apply colors (HSL)    │
│  - Apply camera view     │
└──────────────────────────┘
```

### Memory Management

Three separate Float32 buffers for 15,000 particles:
- **basePositions**: Current particle location (morphing target)
- **targetPositions**: Goal position for current shape
- **renderPositions**: Final screen coordinates (with expansion + noise)
- **colors**: HSL color values updated per frame

This separation prevents visual glitches during morphing and expansion.

## 📊 Configuration

Edit these constants in the `<script>` section to customize:

```javascript
const COUNT = 15000;        // Number of particles
const SIZE = 0.15;          // Individual particle size
```

### Shape Parameters

Modify the `setShape()` function:
- **Sphere**: Radius (r)
- **Cube**: Size (s)
- **Ring**: Inner radius, outer radius, disk height
- **Galaxy**: Spiral density and spread

## 🐛 Troubleshooting

### Black Screen
- Check browser console for WebGL errors
- Verify browser supports WebGL (visit webglreport.com)
- Try disabling browser extensions

### No Hand Detected
- Ensure good lighting conditions
- Position hand clearly in frame
- Check camera permissions
- Try refreshing the page

### Low Performance
- Reduce `COUNT` variable for fewer particles
- Disable browser hardware acceleration for consistent FPS
- Close other browser tabs

### Camera Not Working
- Check browser permissions for camera access
- On HTTPS sites, the browser should prompt for camera
- On HTTP (localhost), you may need to allow manually in browser settings

## 📦 Dependencies

All dependencies are loaded via CDN (no npm installation needed):

- **Three.js**: `https://unpkg.com/three@0.160.0`
- **TensorFlow.js Core**: CDN script
- **TensorFlow.js WebGL Backend**: CDN script
- **MediaPipe Hands**: `@mediapipe/hands@0.4.1675469240`
- **Hand Pose Detection**: `@tensorflow-models/hand-pose-detection@2.0.1`

## 🎨 Customization

### Change Particle Colors

Edit the HSL color calculation in the animation loop:
```javascript
const hue = 0.6 + (handData.x * 0.2);  // Hue shifts with hand position
const saturation = 0.8;                 // Color intensity
const lightness = 0.6 - (dist * 0.015); // Darker at center
```

### Adjust Physics

Modify the morphing speed:
```javascript
basePositions[idx] += (targetPositions[idx] - basePositions[idx]) * 0.05; // 0.05 = morphing speed
```

Lower values = smoother, slower transitions
Higher values = snappier, faster transitions

## 📱 Browser Compatibility

| Browser | Support | Notes |
|---------|---------|-------|
| Chrome | ✅ | Recommended, best performance |
| Firefox | ✅ | Full support |
| Edge | ✅ | Full support |
| Safari | ⚠️ | Works but WebGL may be slower |
| Mobile | ⚠️ | Some devices lack camera support |

## 📚 Learning Resources

- [Three.js Documentation](https://threejs.org/docs/)
- [MediaPipe Hands Guide](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)
- [TensorFlow.js Handbook](https://www.tensorflow.org/js)
- [WebGL Fundamentals](https://webglfundamentals.org/)

## 🚀 Future Enhancements

- [ ] Two-hand support
- [ ] Gesture recognition (peace sign, thumbs up, etc.)
- [ ] Sound visualization integration
- [ ] Recording/export particle animations
- [ ] Mobile optimization
- [ ] Custom shape uploader
- [ ] Performance metrics dashboard

## 📄 License

MIT License - Feel free to use this project for personal or commercial purposes.

## 👤 Author

**Vansh Darji**
- GitHub: [@vansh-1101](https://github.com/vansh-1101)

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs via issues
- Suggest features
- Submit pull requests with improvements

## ⭐ Support

If you find this project interesting, please consider:
- Starring this repository
- Sharing with friends
- Using it in your own projects

---

**Last Updated**: February 2026

**Note**: This project requires a modern browser with WebGL and camera support. For best experience, use Chrome or Firefox on desktop.
