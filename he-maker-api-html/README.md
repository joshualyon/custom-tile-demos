## Hubitat Maker API - With HTML Support
This is a proof-of-concept for reading data from a device using the Hubitat Maker API and displaying it in a tile. This particular 
variant supports a 'hack' which is common in some Hubitat community devices wherein the attribute is stuffed with HTML data.

While normal tiles on a SharpTools dashboard won't render the HTML as a security precaution, this proof-of-concept DOES render the HTML.
This rendering of HTML is allowed in SharpTools Custom Tiles since they are running in a sandboxed context.

> **WARNING**: You should still only run Custom Tile code (and thus injected HTML) from _trusted_ sources! The injected HTML still has access to other 
items within the context of the Custom Tile.

## Setup
This tile requires some basic setup on your Hubitat Hub as it uses the Maker API to reading data.

1. Open the Apps page on your Hubitat hub and add a built-in app called Maker API
2. Configure the Maker API with your desired devices and add `https://run.sharptools.app` to the CORS field  
  **NOTE**: We plan on changing this base domain before production release of Custom Tiles to prevent domain escalation, so it will need to get updated at some point
3. Copy and paste any of the example URLs into the `apiSample` setting
4. Update the `deviceId` and `attribute` settings with your desired configuration


**Note**: To simplify development and sharing purposes, the `tileSettings` variable within the code has been stubbed with these values. The proper
approach to this would be to add Tile Settings in your Custom Tile for each of these settings and uncomment line 23 in `stio.ready()` which would copy 
your per-tile user configured settings into the variable. By using the Tile Settings, it allows you to create multiple copies of the tile with various different 
configurations. 
