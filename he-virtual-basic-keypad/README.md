This Custom Tile is designed to work with a device similar to the [Virtual Basic Keypad](https://community.hubitat.com/t/release-virtual-keypad/51825/2?u=josh) by `mbarone` in the Hubitat Community. 

It expects a device to have a `InputDisplay` attribute which reports the status. For example:
* `Enter Code`
* `*` (one PIN digit entered)
* `Success. Executing {action}` (where the action could be `armHome`, `armAway`, `disarm`)
* `Input Denied`

The device must implement the `PushableButton` and `LockCodes` capabilities for it to show up as a selectable device. 

Note that the device sends each button immediately, but it also tracks a local copy of what it expects the `InputDisplay` state to be. If your hub is slow, you may find that if you press multiple buttons in a row too quickly that the hub / device driver might not be able to register them quickly enough so you might need to take a very slight pause between tapping each. 
