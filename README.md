# electron-liquid-glass

<div align="center">

<img width="387" alt="image" src="https://github.com/user-attachments/assets/3c3c9ea6-2663-4292-b812-a630c2c3f65b" />

![npm](https://img.shields.io/npm/v/electron-liquid-glass)
![npm downloads](https://img.shields.io/npm/dm/electron-liquid-glass)
![GitHub](https://img.shields.io/github/license/meridius-labs/electron-liquid-glass)
![Platform](https://img.shields.io/badge/platform-macOS-blue)
![Node](https://img.shields.io/node/v/electron-liquid-glass)

**Modern macOS glass effects for Electron applications**

_🪄 NATIVE `NSGlassEffectView` integration with ZERO CSS hacks_

[Installation](#-installation) • [Quick Start](#-quick-start) • [API](#-api-reference) • [Examples](examples/) • [Contributing](#-contributing)

</div>

---

## ✨ Features

- 🪟 **Native Glass Effects** - Real `NSGlassEffectView` integration, not CSS approximations
- ⚡ **Zero Configuration** - Works out of the box with any Electron app
- 🎨 **Fully Customizable** - Corner radius, tint colors, and glass variants
- 📦 **Modern Package** - Dual ESM/CommonJS support with TypeScript declarations
- 🔧 **Pre-built Binaries** - No compilation required for standard setups
- 🌙 **Auto Dark Mode** - Automatically adapts to system appearance changes

## 🚀 Installation

```bash
# npm
npm install electron-liquid-glass

# yarn
yarn add electron-liquid-glass

# pnpm
pnpm add electron-liquid-glass

# bun
bun add electron-liquid-glass
```

### Requirements

- **macOS 26+** (Tahoe or later)
- **Electron 30+**
- **Node.js 22+**

> **Note**: This package only works on macOS. On other platforms, it provides safe no-op fallbacks.

## 🎯 Quick Start

### Basic Usage

```javascript
import { app, BrowserWindow } from "electron";
import liquidGlass from "electron-liquid-glass";

app.whenReady().then(() => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,

    vibrancy: false, // <-- ❌❌❌ do NOT set vibrancy alongside with liquid glass, it will override and look blurry

    transparent: true, // <-- This MUST be true
  });

  win.setWindowButtonVisibility(true); // <-- ✅ This is required to show the window buttons

  win.loadFile("index.html");

  /**
   * 🪄 Apply glass effect after content loads 🪄
   */
  win.webContents.once("did-finish-load", () => {
    // 🪄 Apply effect, get handle
    const glassId = liquidGlass.addView(win.getNativeWindowHandle(), {
      /* options */
    });

    // Experimental, undocumented private APIs
    liquidGlass.unstable_setVariant(glassId, 2);
  });
});
```

### TypeScript Usage

```typescript
import { BrowserWindow } from "electron";
import liquidGlass, { GlassOptions } from "electron-liquid-glass";

const options: GlassOptions = {
  cornerRadius: 16, // (optional)
  tintColor: "#44000010", // black tint (optional)
  opaque: true, // add opaque background behind glass (optional)
};

liquidGlass.addView(window.getNativeWindowHandle(), options);
```

## 📚 API Reference

### `liquidGlass.addView(handle, options?)`

Applies a glass effect to an Electron window.

**Parameters:**

- `handle: Buffer` - The native window handle from `BrowserWindow.getNativeWindowHandle()`
- `options?: GlassOptions` - Configuration options

**Returns:** `number` - A unique view ID for future operations

### `GlassOptions`

```typescript
interface GlassOptions {
  cornerRadius?: number; // Corner radius in pixels (default: 0)
  tintColor?: string; // Hex color with optional alpha (#RRGGBB or #RRGGBBAA)
  opaque?: boolean; // Add opaque background behind glass (default: false)
}
```

---

### UNDOCUMENTED EXPERIMENTAL METHODS

> ⚠️ **Warning**: DO NOT USE IN PROD. These methods use private macOS APIs and may change in future versions.

```typescript
// Glass variants (number) (0-15, 19 are functional)
liquidGlass.unstable_setVariant(glassId, 2);

// Scrim overlay (0 = off, 1 = on)
liquidGlass.unstable_setScrim(glassId, 1);

// Subdued state (0 = normal, 1 = subdued)
liquidGlass.unstable_setSubdued(glassId, 1);
```

## 🔧 Development

### Building from Source

```bash
# Clone the repository
git clone https://github.com/meridius-labs/electron-liquid-glass.git
cd electron-liquid-glass

# Install dependencies
bun install

# Build native module
bun run build:native

# Build TypeScript library
bun run build

# Build everything
bun run build:all
```

### Rebuilding for Custom Electron

If you're using a custom Electron version:

```bash
npx electron-rebuild -f -w electron-liquid-glass
```

### Project Structure

```
electron-liquid-glass/
├── src/                 # Native C++ source code
│   ├── glass_effect.mm  # Objective-C++ implementation
│   └── liquidglass.cc   # Node.js addon bindings
├── js/                  # TypeScript source
│   ├── index.ts         # Main library code
│   └── native-loader.ts # Native module loader
├── dist/                # Built library (generated)
├── examples/            # Example applications
└── prebuilds/          # Pre-built binaries
```

## 🏗️ How It Works

1. **Native Integration**: Uses Objective-C++ to create `NSGlassEffectView` instances
2. **View Hierarchy**: Inserts glass views behind your web content, not over it
3. **Automatic Updates**: Listens for system appearance changes to keep effects in sync
4. **Memory Management**: Properly manages native view lifecycle

### Technical Details

- **Primary**: Uses `NSGlassEffectView` API when available
- **Fallback**: Falls back to public `NSVisualEffectView` on older systems
- **Performance**: Minimal overhead, native rendering performance
- **Compatibility**: Works with all Electron window configurations

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Setup

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes and test thoroughly
4. Commit with conventional commits: `git commit -m "feat: add amazing feature"`
5. Push and create a Pull Request

### Reporting Issues

- Use the [issue tracker](https://github.com/meridius-labs/electron-liquid-glass/issues)
- Include your macOS version, Electron version, and Node.js version
- Provide a minimal reproduction case when possible

## 📋 Roadmap

- [ ] **View Management** - Remove and update existing glass views

## 🙏 Acknowledgments

- Apple's private `NSGlassEffectView` API documentation (reverse-engineered)
- The Electron team for excellent native integration capabilities
- Contributors and users who help improve this library

## 📄 License

MIT © [Meridius Labs](https://github.com/meridius-labs) 2025

---

<div align="center">

**Made with ❤️ for the Electron community**

[⭐ Star on GitHub](https://github.com/meridius-labs/electron-liquid-glass) • [🐛 Report Bug](https://github.com/meridius-labs/electron-liquid-glass/issues) • [💡 Request Feature](https://github.com/meridius-labs/electron-liquid-glass/issues)

</div>
