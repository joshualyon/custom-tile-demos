# JSON Expression Custom Tile

Display a value from a JSON variable directly on a SharpTools dashboard. You can select a nested property, read an item from an array, calculate a result, or format a date without first unpacking the JSON into separate variables in a Rule.

## Quick start

1. Create or choose a SharpTools String variable containing valid JSON.
2. Import `json-expression.html` as an HTML Custom Tile.
3. Select the JSON variable.
4. Enter an expression for the value you want to display.
5. Optionally add a label, units, number formatting, or custom colors.
6. Preview the tile and add it to your dashboard.

For example, if the variable contains:

```json
[
  {
    "name": "Sink Sensor",
    "batteryLevel": 87
  }
]
```

Use this expression to display the device name:

```text
data[0].name
```

Or display the battery level with `data[0].batteryLevel` and set the tile's suffix to `%`.

## Understanding `data`

The tile automatically parses the selected String variable as JSON and makes the result available as `data`. You do not need to call a JSON parsing function.

For a JSON object, use dots to access nested properties:

```text
data.weather.temperature
data.weather.condition
```

For a JSON array, select an item by its position. Array positions start at zero, so `[0]` is the first item and `[1]` is the second:

```text
data.devices[0].name
data.devices[1].state
```

Use `data` by itself to display the complete JSON value.

## Common examples

| What you want to display | Expression |
| --- | --- |
| A nested value | `data.weather.temperature` |
| The first array item's name | `data[0].name` |
| The second room's temperature | `data.rooms[1].temperature` |
| The average of a numeric array | `mean(data.temperatures)` |
| The highest value in an array | `max(data.readings)` |
| A comma-separated list | `join(data.names, ", ")` |
| Uppercase status text | `upper(data.status)` |
| Whether a list contains `open` | `contains(data.states, "open")` |

For simple units, use the tile's Prefix or Suffix setting instead of adding them to the expression. For example, display `data.weather.temperature` with a suffix of `°F`.

## Dates and times

Date expressions use the local timezone of the browser displaying the dashboard.

| What you want to display | Expression |
| --- | --- |
| Current local date and time | `formatDate(now(), "L LT")` |
| A timestamp from the JSON | `formatDate(data.updatedAt, "L LT")` |
| Tomorrow at the current time | `formatDate(addDays(now(), 1), "YYYY-MM-DD h:mm A")` |
| Parse a formatted date | `date("07/11/2026 6:30 PM", "MM/DD/YYYY h:mm A")` |
| Beginning of today | `startOfDate(now(), "day")` |
| A two-hour duration in minutes | `durationAs(2 * 60 * 60 * 1000, "minutes")` |

Timestamps containing an explicit timezone offset are converted to the dashboard browser's local timezone. Localized formats such as `L` and `LT` use English formatting.

## Display settings

| Setting | What it controls |
| --- | --- |
| JSON Variable | The String variable containing the JSON source |
| Rule Expression | The value or calculation to display |
| Label | Small heading shown above the result; leave blank to hide it |
| Display Format | Automatic, Text, Number, or formatted JSON |
| Decimal Places | Number of digits after the decimal when Number format is selected |
| Value Prefix | Text shown before the result, such as `$` |
| Value Suffix | Text shown after the result, such as `%` or `°F` |
| Empty Result Text | What to show when the expression returns no value |
| Show Last Updated | Shows the variable's most recent update time |
| Text Color | Main tile text color |
| Accent Color | Color of the small accent line |

## Troubleshooting

### The tile shows an invalid JSON error

The selected variable must contain valid JSON. Property names and text values must use double quotes.

Valid:

```json
{"status": "open"}
```

Invalid:

```text
{status: 'open'}
```

### The tile shows `—`

The expression returned a missing, empty, or `null` value. Check the property name and array position. You can change `—` with the Empty Result Text setting.

### The expression selects the wrong array item

Arrays start at zero:

- `[0]` selects the first item.
- `[1]` selects the second item.
- `[2]` selects the third item.

### The date shows in a different timezone

Dates are displayed using the local timezone of the browser or device showing the dashboard. Different dashboard devices can therefore display the same timestamp in their respective local times.

### The tile shows an expression error

Check spelling, punctuation, and whether the selected property contains the expected type of value. The tile displays the error directly and also logs more detail to the browser console.

## Advanced expressions

More complex expressions can define a temporary function and then use it in a calculation. For example, given an array of room objects:

```text
getTemperature(room) = room.temperature; mean(map(data.rooms, getTemperature))
```

Useful function groups include:

- Numbers: `sum`, `mean`, `min`, `max`, `round`, and `toFixed`
- Text: `upper`, `lower`, `title`, `trim`, `contains`, `replace`, `split`, and `join`
- JSON objects: `objectKeys`, `objectValues`, `objectEntries`, and `stringify`
- Dates: `now`, `date`, `formatDate`, `addDays`, `addHours`, `startOfDate`, `endOfDate`, and `durationAs`

The original, unparsed String variable is also available as `value`. Most tiles should use the automatically parsed `data` value, but the equivalent explicit form is available when needed:

```text
parseJson(value).weather.temperature
```

## Limitations

- The source must be one SharpTools String variable containing valid JSON.
- Expressions cannot reference other SharpTools variables by names such as `$myVariable`.
- Rule context values are not available inside the tile.
- Dates use the dashboard browser's local timezone.
- Sunrise and sunset calculations are not included.
