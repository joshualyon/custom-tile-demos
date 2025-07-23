# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains Custom Tile demos for SharpTools.io - a smart home dashboard platform. Each tile is a self-contained HTML file with embedded JavaScript and CSS that extends SharpTools functionality through the Custom Tiles API.

## Architecture

- **Standalone HTML Files**: Each custom tile is a single HTML file with all code embedded
- **No Build Process**: Direct HTML editing without compilation or bundling
- **SharpTools API Integration**: All tiles use `https://cdn.sharptools.io/js/custom-tiles.js` (generic URL that forwards to latest version)
- **JSON Configuration**: Each tile starts with a JSON settings schema
- **Sandboxed Execution**: Tiles run in isolated contexts for security

## Essential Documentation

**MUST READ** before developing Custom Tiles:
- https://raw.githubusercontent.com/sharptools-io/developer-docs/main/content/developer-tools/custom-tiles/html.md
- https://raw.githubusercontent.com/sharptools-io/developer-docs/main/content/developer-tools/custom-tiles/stio-lib.md

## Common Development Tasks

### Creating/Editing Tiles
1. Edit HTML files directly in their respective directories
2. Upload complete HTML file to SharpTools Custom Tile editor
3. Test in SharpTools dashboard

### Transpilation for Browser Compatibility
- Modern JavaScript (ES2022) can be used
- SharpTools editor has built-in transpiler (Code Editor > Cog > Preview Transpile)
- Converts modern syntax to ES5 for older browser support

## Project Structure

### Platform-Specific Integrations
- `/he-*` - Hubitat integrations (Maker API, virtual devices)
- `/homey/` - Homey smart home platform tiles
- `/smartthings-scenes/` - SmartThings scene control

### UI Components
- `/gauges/` - Various gauge implementations (ApexCharts, GaugeJS)
- `/analog-clock/` - Animated clock display
- `/vertical-range-input/` - Vertical slider controls (recommended over audio-range-vertical)
- `/audio-range-vertical/` - Legacy audio level controls (use vertical-range-input instead)

### Utility Tiles
- `/html-attribute/` - Display device attributes as HTML
- `/refreshable-url-embed/` - Embed external URLs with refresh
- `/sunrise-sunset/` - Sunrise/sunset time display

## Modern Integration Approach (Recommended)

### Using THING and VARIABLE Settings
- **Preferred Method**: Use `THING`, `THING:ATTRIBUTE:NAME`, and `VARIABLE` setting types
- Leverages `stio` JS library for direct platform integration
- Examples:
  - `/html-attribute/` - Demonstrates THING:ATTRIBUTE:NAME usage
  - `/gauges/thingGaugeJS.html` - Shows VARIABLE integration
- Benefits: Simpler configuration, automatic authentication, real-time updates

### Direct API Integration (Fallback Only)
Use platform APIs directly only when THING integration doesn't provide needed data:

#### Hubitat Maker API
- Uses CORS-enabled Maker API endpoints
- Requires API token and device ID configuration
- See `/he-maker-api-html/` for implementation example

#### SmartThings
- Uses Personal Access Tokens (PAT)
- Direct REST API calls to SmartThings cloud
- See `/smartthings-scenes/` for pattern

#### Homey
- Multiple approaches documented in `/homey/src/README.md`:
  - API Key + Direct HTTP (Homey 2023 only)
  - API Key + Homey Library
  - Homey.ink Token (unofficial workaround for older models)

## Security Considerations

- **HTML Rendering**: Several tiles render raw HTML from device attributes
- **API Tokens**: Stored in tile settings, accessible only to tile instance
- **CORS Requirements**: External APIs must support CORS for browser access
- **Trusted Code Only**: Users warned to only use tiles from trusted sources

## Testing

No automated tests exist. Manual testing process:
1. Upload tile to SharpTools
2. Configure tile settings
3. Add to dashboard
4. Verify functionality across different browsers/devices

## Common Patterns

### Library Installation
```html
<script src="https://cdn.sharptools.io/js/custom-tiles.js"></script>
```

### Tile Initialization
```javascript
stio.ready(function(data) {
    // Access tile settings via data.settings
    const thing = data.settings.thing;          // Enriched Thing object
    const attrName = data.settings.attribute;   // String: attribute name
    const variable = data.settings.variable;     // Enriched Variable object
    
    // For Things: 2-step process
    if (thing && attrName) {
        // Step 1: Setup attribute value listener
        thing.attributes[attrName].onValue(function(value) {
            console.log(`${attrName} updated:`, value);
            // Update your UI here
        });
        
        // Step 2: Subscribe to receive updates (REQUIRED!)
        thing.subscribe(attrName);
    }
    
    // For Variables: simpler 1-step process
    if (variable) {
        variable.onValue(function(value) {
            console.log('Variable updated:', value);
            // Update your UI here
        });
    }
});
```

### Working with Things
```javascript
// 2-step process for attribute updates:
// 1. Setup event listener for the attribute
thing.attributes[attrName].onValue(function(value) {
    console.log(`Attribute ${attrName} updated:`, value);
    updateUI(value);
});

// 2. Subscribe to request updates from the hub (REQUIRED!)
thing.subscribe(attrName);  // Single attribute
// OR
thing.subscribe(['switch', 'level']);  // Multiple attributes

// Send commands to things
thing.sendCommand("on");
thing.sendCommand("setLevel", 50);

// Access current attribute values
const currentValue = thing.attributes.switch.value;
```

### Working with Variables
```javascript
// Set variable value
myVariable.setValue("new value");

// Subscribe to value changes
myVariable.onValue(function(value) {
    console.log('Variable updated:', value);
    console.log('Timestamp:', myVariable.timestamp);
});

// Access current value
const currentValue = myVariable.value;
```

**Variable Setting Requirements**: When using VARIABLE in tile settings, you MUST specify the variable type filter:
```json
{"type": "VARIABLE", "name": "variable", "label": "Variable", "filters": {"type": "String"}}
```
Available types: "String", "Number", "Boolean"

### User Interaction Methods
```javascript
// Show toast notification
stio.showToast("Success!", "green");  // Colors: green, red, blue

// Show selection list
stio.showList([
    {label: "Option 1", value: "opt1"},
    {label: "Option 2", value: "opt2"}
]).then(function(selected) {
    console.log("User selected:", selected);
});

// Show input form
stio.showForm({
    title: "Configure Settings",
    items: [
        {name: "text", label: "Enter Text", type: "STRING"},
        {name: "number", label: "Enter Number", type: "NUMBER"},
        {name: "toggle", label: "Enable Feature", type: "BOOLEAN"},
        {name: "choice", label: "Select Option", type: "ENUM", 
         options: [{label: "A", value: "a"}, {label: "B", value: "b"}]}
    ]
}).then(function(data) {
    console.log("Form data:", data);
});
```

### Settings Schema Examples

**Important**: Place the settings schema at the END of your HTML document to prevent users from accidentally modifying it.

Modern approach using THING:
```html
<!-- Do not edit below -->
<script type="application/json" id="tile-settings">
{
  "schema": "0.2.0",
  "settings": [
    {"type": "THING", "name": "thing", "label": "Thing"},
    {"type": "THING:ATTRIBUTE:NAME", "name": "attribute", "label": "Attribute", "filters": {"source": "thing"}}
  ],
  "name": "Custom Tile Name"
}
</script>
<!-- Do not edit above -->
```

Variable integration:
```html
<!-- Do not edit below -->
<script type="application/json" id="tile-settings">
{
  "schema": "0.2.0",
  "settings": [
    {
      "type": "VARIABLE", 
      "name": "variable", 
      "label": "Variable",
      "filters": {"type": "String"}
    }
  ],
  "name": "Variable Tile"
}
</script>
<!-- Do not edit above -->
```

**Important**: VARIABLE settings MUST specify `filters.type` with one of: "String", "Number", or "Boolean".

Legacy API approach (deprecated - use schema 0.2.0 for new tiles):
```html
<!-- Do not edit below -->
<script type="application/json" id="tile-settings">
{
  "schema": "0.1.0",
  "settings": [
    {"name": "apiToken", "label": "API Token", "type": "STRING"},
    {"name": "deviceId", "label": "Device ID", "type": "STRING"}
  ],
  "name": "Legacy Tile"
}
</script>
<!-- Do not edit above -->
```

### Advanced Settings Features

#### Capability Filters and Conditional Display
Settings can be filtered by device capabilities and conditionally displayed:

```json
{
  "type": "THING",
  "name": "audioThing",
  "label": "Audio Device",
  "filters": {"capabilities": ["audioVolume"]},
  "showIf": ["thingType", "==", "audioVolume"]
}
```

- **filters**: Restricts Thing selection by capabilities (e.g., `switchLevel`, `audioVolume`)
- **showIf**: Conditionally shows setting based on other setting values
  - Format: `["settingName", "operator", "value"]`
  - Operators: `==`, `!=`, `<`, `<=`, `>`, `>=`
- **default**: Shows default value in UI but remains `undefined` in code until user saves

#### Complete Example with Conditional Settings
```javascript
{
  "schema": "0.2.0",
  "settings": [
    {
      "type": "ENUM",
      "name": "thingType",
      "label": "Thing Type",
      "values": [
        {"label": "Audio Volume", "value": "audioVolume"},
        {"label": "Switch Level", "value": "switchLevel"},
        {"label": "Custom", "value": "custom"}
      ],
      "default": "switchLevel"
    },
    {
      "type": "THING",
      "name": "audioThing",
      "label": "Thing",
      "filters": {"capabilities": ["audioVolume"]},
      "showIf": ["thingType", "==", "audioVolume"]
    },
    {
      "type": "THING",
      "name": "levelThing",
      "label": "Thing",
      "filters": {"capabilities": ["switchLevel"]},
      "showIf": ["thingType", "==", "switchLevel"]
    },
    {
      "type": "THING",
      "name": "customThing",
      "label": "Thing",
      "filters": {"capabilities": []},
      "showIf": ["thingType", "==", "custom"]
    },
    {
      "type": "THING:ATTRIBUTE:NAME",
      "name": "customAttribute",
      "label": "Custom Attribute",
      "filters": {"source": "customThing"},
      "showIf": ["thingType", "==", "custom"]
    }
  ],
  "name": "Advanced Tile Example"
}
```

### Error Handling Best Practices

Always use defensive coding since settings may be undefined:

```javascript
stio.ready(function(data) {
    // Check settings exist before using
    const settings = data.settings || {};
    const defaultLevel = settings.defaultLevel || 50;
    
    // Check Thing and attributes exist
    const thing = settings.thing;
    const attrName = settings.attribute;
    
    if (thing && attrName && thing.attributes && thing.attributes[attrName]) {
        // Safe to use the attribute
        thing.attributes[attrName].onValue(function(value) {
            // Handle value updates
        });
        thing.subscribe(attrName);
    } else {
        // Handle missing configuration gracefully
        console.warn('Thing or attribute not configured');
        showError('Please configure Thing and Attribute in tile settings');
    }
});
```

#### Error Display Pattern

Create a dedicated error display method and DOM element for user-friendly error messages:

```javascript
// Add error container to your HTML
const errorContainer = document.createElement('div');
errorContainer.id = 'error-message';
errorContainer.style.cssText = 'color: red; padding: 10px; display: none;';
document.body.appendChild(errorContainer);

// Error display function
function showError(message) {
    console.error('Tile Error:', message);  // Always log to console
    const errorElement = document.getElementById('error-message');
    if (errorElement) {
        errorElement.textContent = message;
        errorElement.style.display = 'block';
    }
}

// Usage examples
showError('API key is invalid. Please check your settings.');
showError('Failed to connect to device. Please verify configuration.');
```

**Important**: Do NOT use `stio.showToast()` for error messages - this is an anti-pattern. Toast messages should only be used for brief success notifications.