# Device mode

Device mode turns the Virtual Remote into a remote for one device configured on your hub. Instead of showing an Activity's controls, it uses the selected device's own button assignments and gives you access to every command stored for that device.

> [!IMPORTANT]
> This page describes Device mode in card version 0.2.4, bundled with Sofabaton X 0.6.9. Device mode requires the [Sofabaton X integration](https://github.com/m3tac0de/home-assistant-sofabaton-x1s) version 0.6.7 or newer with **Persistent Cache** enabled. Device mode is not available with the Official Sofabaton Hub integration.

## What Device mode provides

- **Your device's button assignments:** the card automatically uses the commands assigned to the physical remote's buttons for the selected device. Buttons without an assignment are disabled.
- **Every device command:** open **Commands** to search and send any command stored for the device, including commands that are not assigned to a physical button.
- **Per-device shortcuts:** place up to three frequently used commands in a dedicated Shortcuts row and give each one its own icon.
- **Device power control:** when power behavior is configured on the hub, the card can show a Power button that sends the appropriate Power On or Power Off command.
- **Per-device layouts:** start with one default Device layout, then adjust the visible controls and their order for individual devices.

## Configure Device mode in the visual editor

The visual editor is the recommended way to configure the Virtual Remote. It exposes Device mode's layout, command, shortcut, and Power options without requiring device IDs or command IDs.

### Enable Device mode

1. In the **Sofabaton Control Panel**, open **Settings** and enable **Persistent Cache**.
2. Add or edit the **Sofabaton Virtual Remote** card and select its remote entity.
3. Open **General Options** and check that **Enable device mode** is on (the default). Optionally use **Initial view** to start on a specific device.

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
- **Number pad is missing:** use card 0.2.4 or newer on an X2, enable **Number pad** in the selected Device layout, and check that the device has at least one numeric key assigned. Refresh the cache after changing assignments, then reload the dashboard.

## Configuration reference

The following fields belong inside `device_mode`:

| Field | Type | Description | Default |
| --- | --- | --- | --- |
| `enabled` | boolean | Allow Device mode when the integration and cache support it. `false` removes all Device mode controls. | `true` |
| `open_device` | number / null | Device ID to select when the card loads. Ignored if Device mode is unavailable. | `null` (current Activity) |
| `layouts` | object | `default` holds shared Device settings; a device ID such as `"12"` holds that device's overrides. | `{}` |
| `shortcuts` | object | Per-device shortcut slots; see below. | `{}` |

Each entry in `device_mode.layouts` supports these fields. The resolution order is built-in defaults, `device_mode.layouts.default`, then the selected device's overrides. Activity layouts and top-level layout switches do not participate.

| Field | Type | Description | Default |
| --- | --- | --- | --- |
| `show_activity` | boolean | Show the Device selector row, including its mode switch. The stored key keeps its Activity-mode name. | `true` |
| `show_device_toggle` | boolean | Show the Activity/Device switch in the selector row. Does not disable Device mode. | `true` |
| `show_dpad` | boolean | Show the Direction Pad. | `true` |
| `show_numpad` | boolean | Allow the X2 keypad when at least one numeric key is assigned. Independent of `show_dpad`. | `true` |
| `show_nav` | boolean | Show Back, Home, and Menu. | `true` |
| `show_volume` | boolean | Show Volume Up/Down and Mute. | `true` |
| `show_channel` | boolean | Show Channel controls. | `true` |
| `show_media` | boolean | Show playback controls. | `true` |
| `show_dvr` | boolean | Show X2 DVR, Pause, and Exit controls. | `true` |
| `show_colors` | boolean | Show Red, Green, Yellow, and Blue. | `true` |
| `show_abc` | boolean | Show X2 A/B/C. | `true` |
| `show_commands_button` | boolean | Show Commands, as a drawer button or inline row. | `true` |
| `show_power_button` | boolean | Show Power when the device has power behavior configured. Shares the Commands group's position. | `true` |
| `show_shortcuts` | boolean | Show the Shortcuts row when the selected device has at least one configured slot. | `true` |
| `c_as_rows` | boolean | Show Commands inline instead of in a drawer. | `false` |
| `c_row_visible_rows` | number | Visible command-button rows before scrolling, from 1 to 6; applies when `c_as_rows` is `true`. | `2` |
| `group_order` | list | Uses the [shared group names and default order](../README.md#activity-layout-options). `macro_favorites` positions the Commands/Power row in drawer mode; `macros_row` positions inline Commands with Power. `shortcuts` positions the Shortcuts row. Omitted groups are appended. | Shared default order |

Use `c_as_rows`, `c_row_visible_rows`, and `show_commands_button` for Device layouts, rather than the Activity-only `mf_*` and Macros/Favorites switches. `show_mid` and `show_favorite_device_names` are also Activity-only. Card-wide theme and sizing settings still apply in both modes.

For `device_mode.shortcuts`, use a device ID as the key and any of `left`, `middle`, and `right` as slots. Each slot requires a string `icon` (for example `mdi:netflix`) and a numeric `command_id` from that device. There is no `default` shortcut assignment; slots are strictly per device.

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
      show_device_toggle: true
      show_numpad: true
    "12":
      show_colors: false
      show_dpad: false # On X2, show the assigned number pad on its own
```

- `enabled` controls Device mode and defaults to `true`.
- `open_device` is the device ID to open when the card loads.
- `layouts.default` contains the shared Device layout. A layout under a device ID contains only that device's overrides.
- Device layouts support `group_order`, all applicable `show_*` controls, `c_as_rows`, and `c_row_visible_rows`.
- `shortcuts` contains strictly per-device `left`, `middle`, and `right` slots. Each slot requires an `icon` and `command_id`.
- `shortcuts` is also the Device-mode-only group name used to position the Shortcuts row in `group_order`.

</details>
