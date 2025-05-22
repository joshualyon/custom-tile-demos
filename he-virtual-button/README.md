# Hubitat Virtual Button Custom Tile

A simple, customizable SharpTools Custom Tile for triggering Hubitat devices with the `PushableButton` capability.

## Features

- **Clean, minimal design** that inherits styling from your tile configuration
- **Flexible button handling** - works with single or multiple button devices
- **Dynamic button selection** - automatically shows button list for multi-button devices
- **Customizable display** - show custom text, icons, or default button symbol
- **Visual click feedback** - subtle overlay confirms button presses
- **Full tile clickable** - tap anywhere on the tile to activate

## Installation

1. Open your [SharpTools Custom Tile Developer Tools](https://sharptools.io/developer/custom-tiles/)
2. Create a new Custom Tile and select "HTML" type
3. Copy and paste the HTML code from this repository
4. Configure the tile settings as described below
5. Save and add the tile to your dashboard

## Configuration

### Required Settings

* **Device**: Select a Hubitat device with PushableButton capability

### Optional Settings

- **Button Number**: Specify which button to push (leave empty to select from list)
- **Display Type**: Choose between "Icon" or "Text" display
- **Icon Library**: Select icon library (Font Awesome or Material Design)
- **Icon Name**: Enter the icon name (without prefix) to display (eg. `power-off`, `play`, `lightbulb`)
- **Display Text**: Custom text to display instead of icon 

## Usage Scenarios

### Single Button Device
- Configure device and optionally set Button Number to `1`
- Clicking the tile immediately pushes the button
- Perfect for simple on/off or trigger actions

### Multi-Button Device (Specific Button)
- Configure device and set Button Number to desired button (e.g., `2`)
- Clicking the tile immediately pushes that specific button
- Good for dedicated function tiles

### Multi-Button Device (Dynamic Selection)
- Configure device but leave Button Number empty
- Clicking the tile shows a list of available buttons to choose from
- Ideal for devices with multiple functions

## Display Options

### Default Icon
- Leave both Icon Name and Display Text empty
- Shows a simple circular button icon
- Works with any tile color scheme

### Custom Icon
- Set Display Type to "Icon"
- Enter icon name (without prefix) in Icon Name field
- Choose icon library (Font Awesome or Material Design)
- **Font Awesome examples**: `power-off`, `play`, `pause`, `stop`, `home`
- **Material Design examples**: `power`, `play-arrow`, `pause`, `stop`, `home`

### Custom Text
- Set Display Type to "Text"
- Enter desired text in Display Text field
- Text inherits tile's font styling

## Compatible Devices

This tile works with any Hubitat device that implements the `PushableButton` capability, including:

- **Virtual Button devices**
- **Button Controller devices**
- **Physical button devices** (Pico remotes, button fobs, etc.)
- **Multi-button devices** with multiple button endpoints

## Troubleshooting

### "No device configured" error
- Ensure you've selected a device in the tile settings
- Verify the device has the `PushableButton` capability

### Icon not showing
- Check that the icon name is correct (without `fa-` or `mdi-` prefix)
- Try switching between Font Awesome and Material Design libraries
- Verify icon exists in the selected library

### Button not responding
- Check that the device is online in Hubitat
- Verify the button number exists on the device
- Check Hubitat logs for any error messages

## Styling Notes

- The tile inherits all colors, fonts, and styling from your SharpTools tile configuration
- Visual feedback (white overlay on press) works best on colored or dark backgrounds
- Content is automatically centered within the tile space
- Tile scales appropriately for different tile sizes

## License

This Custom Tile is provided as-is for use with SharpTools and Hubitat. Feel free to modify and customize for your needs.