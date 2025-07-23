# Media Image Display Tile

This Custom Tile for SharpTools displays images from URLs stored in Thing attributes or SharpTools Variables. It's perfect for displaying dynamic images like game artwork, camera snapshots, or any other media that changes based on your smart home state.

## Features

- Display images from URLs stored in SharpTools Variables
- Display images from URLs stored in Thing attributes (e.g., HomeAssistant sensors)
- Automatic image updates when the source URL changes
- Error handling for invalid URLs or failed image loads
- Responsive image display that scales to fit the tile

## Use Cases

- Display currently playing game artwork from gaming consoles (PS5, Xbox, etc.)
- Show album artwork from media players
- Display camera snapshots or weather maps
- Show any dynamic image content from your smart home devices

## Configuration

1. **Source Type**: Choose between Variable or Thing Attribute
2. **Variable Mode**: Select a SharpTools Variable that contains an image URL
3. **Thing Mode**: Select a Thing and then choose an attribute that contains an image URL

## Example Setup

### For HomeAssistant PS5 Game Artwork:
1. Set Source Type to "Thing Attribute"
2. Select your PS5 sensor Thing
3. Select the attribute containing the game artwork URL

### For SharpTools Variable:
1. Set Source Type to "Variable"
2. Select a Variable containing an image URL
3. The tile will automatically update when the Variable value changes

## Technical Details

- Images are displayed using `object-fit: contain` to maintain aspect ratio
- Supports all standard web image formats (PNG, JPG, GIF, WebP, etc.)
- Includes URL validation and error handling
- Real-time updates via SharpTools event system