# Open Weather Custom Tile

Display current conditions and a multi-day forecast on a SharpTools dashboard. The tile includes several layouts, optional air-quality information, configurable units and language, and multiple weather-data providers.

## Install

Visit the [Weather Tile community release thread](https://community.sharptools.io/t/weather-tile-open-weather-current-and-forecast/9237) for the latest screenshots, setup guidance, release notes, and import link.

After importing the tile:

1. Add it to a SharpTools dashboard from **Other → Custom Tiles**.
2. Edit the tile on the dashboard.
3. Select a weather provider and configure the location as latitude and longitude.
4. Choose the preferred units, language, layout, and optional features.

## Weather providers

### Open-Meteo

Open-Meteo is the default provider and does not require an API key.

### OpenWeather 2.5 Multi

Uses OpenWeather's current-weather and five-day forecast endpoints. An OpenWeather API key is required.

### OpenWeather One Call 3.0

Uses the combined One Call 3.0 current-and-forecast endpoint. An OpenWeather API key with a separate **One Call by Call** subscription for One Call 3.0 is required.

### OpenWeather One Call 4.0

Uses the separate One Call 4.0 current-weather and daily-timeline endpoints. An OpenWeather API key with a separate **One Call by Call** subscription for One Call 4.0 is required.

One Call 4.0 makes two weather requests per tile refresh. At the default three-hour interval, a continuously displayed tile makes approximately 16 One Call 4.0 requests per day. Enabling AQI adds one additional provider request per refresh. See the [API Preference section](https://community.sharptools.io/t/weather-tile-open-weather-current-and-forecast/9237#api-preference) for current usage and subscription details.

## Configuration notes

- **Location:** Enter decimal latitude and longitude separated by a comma or space, such as `33,-96`.
- **Units:** Select imperial or metric units.
- **Language:** Enter a supported provider language code when localization is desired.
- **Layout:** Choose from current conditions, forecast, compact, wide, horizontal, and weekly-trend layouts.
- **Refresh interval:** The default is three hours. Shorter custom intervals consume API allowances more quickly, especially across multiple displayed tile instances.
- **AQI:** Air-quality data requires an additional request when enabled.

## Updating an installed tile

Imported Custom Tiles are not updated automatically. To pull a newer release:

1. Open the [SharpTools Custom Tile developer tools](https://sharptools.io/developer/custom-tiles/).
2. Select the Open Weather tile.
3. Open the code editor's gear menu and choose **Update from Source**.
4. Review the update, then save it.

Older installations may still use the original GitHub Gist as their update source. That mirror continues to be maintained for compatibility.

## Support and source

- Questions and release notes: [SharpTools Community](https://community.sharptools.io/t/weather-tile-open-weather-current-and-forecast/9237)
- Canonical source: [open-weather.html](open-weather.html)

This is a community-supported Custom Tile. Review third-party API terms, pricing, and usage limits before selecting a provider or shortening the refresh interval.
