# Device mode

Device mode turns the Virtual Remote into a remote for one device configured on your hub. Instead of showing an Activity's controls, it uses the selected device's own button assignments and gives you access to every command stored for that device.

> [!IMPORTANT]
> This page describes Device mode in card version 0.2.2. It requires the [Sofabaton X integration](https://github.com/m3tac0de/home-assistant-sofabaton-x1s) version 0.6.7 or newer with **Persistent Cache** enabled. Device mode is not available with the Official Sofabaton Hub integration.

## What Device mode provides

- **Your device's button assignments:** the card automatically uses the commands assigned to the physical remote's buttons for the selected device. Buttons without an assignment are disabled.
- **Every device command:** open **Commands** to search and send any command stored for the device, including commands that are not assigned to a physical button.
- **Per-device shortcuts:** place up to three frequently used commands in a dedicated Shortcuts row and give each one its own icon.
- **Device power control:** when power behavior is configured on the hub, the card can show a Power button that sends the appropriate Power On or Power Off command.
- **Per-device layouts:** start with one default Device layout, then adjust the visible controls and their order for individual devices.
- **Configured long-press assignments:** holding a supported button for 500 ms follows the long-press command configured on the hub.

## Configure Device mode in the visual editor

The visual editor is the recommended way to configure the Virtual Remote. It exposes Device mode's layout, command, shortcut, and Power options without requiring device IDs or command IDs.

### Enable Device mode

1. In the **Sofabaton Control Panel**, open **Settings** and enable **Persistent Cache**.
2. Add or edit the **Sofabaton Virtual Remote** card and select its remote entity.
3. Open **General Options** and turn on **Enable device mode**. Optionally use **Initial view** to start on a specific device.

The Activity/Device button beside the selector is now available on the card. Use it to switch between Activity mode and Device mode.

### Build the default Device layout

1. Open **Layout Options**.
2. Select **Default device layout**.
3. Show, hide, and reorder the control groups you want every device to start with.
4. Choose whether Commands opens in a drawer or appears as an inline, scrollable row.
5. Choose whether to show the Power button, Shortcuts row, and Activity/Device mode switch.

The default Device layout is the shared starting point for every device. It is independent from the default Activity layout, so changing one does not change the other.

### Customize an individual device

1. In **Layout Options**, select the device you want to customize instead of **Default device layout**.
2. Change only the controls or ordering that should differ for that device.
3. Leave the other settings untouched to continue using the defaults.

Each device layout contains only that device's overrides. This makes it practical to create a shared layout once, then make small adjustments—for example, hiding Channel controls on a receiver or placing Commands above the Direction Pad on a media player.

The **Mode switch** option hides the Activity/Device button for the selected layout; it does not disable Device mode.

### Add shortcuts

Shortcuts are configured for one device at a time:

1. Select the specific device in **Layout Options**.
2. Find the **Shortcuts** row.
3. Select its left, middle, or right slot.
4. Choose an icon and a command. Both are required.
5. Repeat for the other slots as needed, or use **Reset** to clear a slot.

The three positions are fixed, so you can configure only the left, middle, or right button and it will remain in that position. Shortcut assignments do not carry over to other devices. The Default device layout can position or hide the Shortcuts row, but shortcuts themselves must be assigned on a specific device.

Outside the editor, the Shortcuts row appears only when at least one shortcut is configured for the selected device. If a saved command is later removed from the hub, its shortcut remains visible but disabled so that you can identify and replace the stale assignment.

### Configure Power

Power is available only for devices that have power behavior configured on the hub. The card automatically hides it for other devices, even if **Power** is enabled in the layout.

The Power button shares the Commands row. Reordering that row also moves Power. You can hide Commands and keep Power visible; in that case, Power appears on its own row.

When Power is pressed, the card reads the device's current power state from the hub and sends the configured Power On or Power Off command. If the state cannot be read, the card sends nothing rather than guessing.

## Use Device mode

1. Use the Activity/Device button beside the selector to enter Device mode.
2. Select a device. Its button assignments and layout load automatically.
3. Tap an assigned button, open **Commands** to find another command, or use one of your shortcuts.
4. Hold a button with a configured long-press assignment for 500 ms to send that assignment once. Releasing it does not also send the short-press command.

Configured hub long-press is separate from the card's optional hold-to-repeat feature. If both apply to the same button, hold-to-repeat takes precedence.

## Troubleshooting

- **Device mode is not available:** confirm that the card uses an entity from the Sofabaton X integration, that the integration is version 0.6.7 or newer, and that Persistent Cache is enabled.
- **A device or its commands are missing:** open the Sofabaton Control Panel's **Hub** tab and refresh the cache, then reload the dashboard.
- **Power is missing:** confirm that power behavior is configured for the device on the hub, then refresh the cache.
- **A shortcut is disabled:** its saved command is no longer available. Edit that device's Shortcuts row and select a current command or reset the slot.
- **A button is disabled:** the selected device has no command assigned to that physical button. Use **Commands** to access unassigned commands.

## Advanced: YAML configuration

Most users should use the visual editor. It creates and maintains the configuration below without requiring you to find device or command IDs manually.

<details>
<summary>Show the Device mode YAML structure</summary>

```yaml
device_mode:
  enabled: true
  open_device: 12
  shortcuts:
    "12":
      left:
        icon: mdi:netflix
        command_id: 7
      right:
        icon: mdi:cog
        command_id: 15
  layouts:
    default:
      c_as_rows: true
      c_row_visible_rows: 3
      show_power_button: true
      show_shortcuts: true
    "12":
      show_colors: false
```

- `enabled` controls Device mode and defaults to `true`.
- `open_device` is the device ID to open when the card loads.
- `layouts.default` contains the shared Device layout. A layout under a device ID contains only that device's overrides.
- Device layouts support `group_order`, all applicable `show_*` controls, `c_as_rows`, and `c_row_visible_rows`.
- `shortcuts` contains strictly per-device `left`, `middle`, and `right` slots. Each slot requires an `icon` and `command_id`.
- `shortcuts` is also the Device-mode-only group name used to position the Shortcuts row in `group_order`.

</details>
