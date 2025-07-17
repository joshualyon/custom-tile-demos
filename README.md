# SharpTools Custom Tile Demos

A collection of Custom Tile demonstrations and reference implementations for [SharpTools.io](https://sharptools.io) - a smart home dashboard platform.

## For Users

Looking to use these custom tiles? Each tile has detailed setup instructions in the SharpTools Community:

- 🏠 **[Community Projects](https://community.sharptools.io/c/community-projects/14)** - Find the official release thread for each custom tile with:
  - Step-by-step import instructions
  - Configuration guides
  - Screenshots and examples
  - Community support

To use a custom tile:
1. Find the tile's release thread in [Community Projects](https://community.sharptools.io/c/community-projects/14)
2. Click the one-click import link in the thread
3. Follow the configuration guide provided

## For Developers

Building your own custom tiles? Start here:

### 📚 Official Documentation
- **[Custom Tiles Overview](https://docs.sharptools.io/developer-tools/custom-tiles/html)** - Core concepts and getting started
- **[stio Library Reference](https://docs.sharptools.io/developer-tools/custom-tiles/stio-lib)** - API documentation

### 🔍 Reference Implementations

Learn from these examples in the repository:

#### Modern Approach (Recommended)
- **`/html-attribute/`** - THING and THING:ATTRIBUTE:NAME settings
- **`/gauges/thingGaugeJS.html`** - VARIABLE integration with gauge visualization
- **`/vertical-range-input/`** - Advanced features including:
  - Capability filters for device selection
  - *Undocumented*: Conditional settings using `showIf` (use at your own risk)

#### UI Components
- **`/gauges/`** - Complex visualizations with ApexCharts and GaugeJS
- **`/analog-clock/`** - Animated SVG elements
- **`/sunrise-sunset/`** - External API integration

#### Platform Integrations (Fallback Approach)
- **`/he-maker-api-html/`** - Hubitat Maker API (when THING access isn't sufficient)
- **`/smartthings-scenes/`** - SmartThings API with Personal Access Tokens
- **`/homey/`** - Multiple Homey integration methods

### 🏗️ Repository Structure

```
/he-*                    # Hubitat platform integrations
/homey/                  # Homey platform tiles
/smartthings-scenes/     # SmartThings integrations
/gauges/                 # Various gauge visualizations
/vertical-range-input/   # Vertical slider controls
/html-attribute/         # Display device attributes
/analog-clock/          # Clock display
/sunrise-sunset/        # Sunrise/sunset times
```

### 🛠️ Development Tips

1. **Use Modern Approach**: Prefer THING/VARIABLE settings over direct API integration
2. **Test Broadly**: Verify functionality across different browsers and devices
3. **Handle Errors**: Implement user-friendly error messages (see examples)
4. **Follow Patterns**: Review existing tiles for coding conventions
5. **Check CLAUDE.md**: Internal development guidelines and patterns

### ⚠️ Important Notes

- Custom tiles run in a sandboxed environment
- Always validate user inputs and handle missing configurations
- Some features shown in examples (like `showIf`) are undocumented and may change
- Place settings schema at the end of your HTML file

## Contributing

This repository primarily serves as a reference collection. For questions or to share your own custom tiles:

- 💬 **[SharpTools Community](https://community.sharptools.io)** - Get help and share ideas

**Note**: Custom Tiles are community-supported. For help with a specific tile, please find its release thread in the [Community Projects](https://community.sharptools.io/c/community-projects/14) section.

## License

Individual custom tiles may have their own licenses. Check each file for specific terms.