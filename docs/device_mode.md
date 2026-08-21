# Device mode

Device mode controls one hub device directly, using that device's own button bindings and full command list instead of an Activity.

> [!IMPORTANT]
> Device mode requires card version 0.2.1 or newer, the [Sofabaton X integration](https://github.com/m3tac0de/home-assistant-sofabaton-x1s), and Persistent Cache. It is not available with the Official Sofabaton Hub integration.

## Set up in the visual editor

1. In the **Sofabaton Control Panel**, open **Settings** and enable **Persistent Cache**.
2. Add or edit the **Sofabaton Virtual Remote** card and select its remote entity.
3. Turn on **Enable device mode**. Optionally use **Initial view** to start on a specific device.
4. Open **Layout Options** and select **Default device layout** or a specific device. Configure its visible controls, order, Commands presentation, and mode switch just like an Activity layout.

The default Device layout applies to every device. A layout selected for a specific device contains only that device's overrides.

**Mode switch** hides the Activity/Device switch button for the selected layout; it does not disable Device mode.

## Use Device mode

- Use the button beside the selector to switch between Activities and Devices.
- Select a device to load its button bindings. Unbound buttons are disabled.
- Open **Commands** to search and send any command stored for that device.

If devices or commands are missing, open the Sofabaton Control Panel's **Hub** tab and refresh the cache.

## YAML reference

Everything is optional and lives under `device_mode`:

```yaml
device_mode:
  open_device: 12
  layouts:
    default:
      c_as_rows: true
      c_row_visible_rows: 3
    "12":
      show_colors: false
```

| Key           | Type         | Purpose                                      | Default          |
| :------------ | :----------- | :------------------------------------------- | :--------------- |
| `enabled`     | boolean      | Set to `false` to disable Device mode.       | `true`           |
| `open_device` | number       | Device ID to open when the card loads.       | Current Activity |
| `layouts`     | map / object | `default` plus optional Device-ID overrides. | `{}`             |

Device layouts are independent from Activity layouts. Resolution order is built-in defaults → `device_mode.layouts.default` → `device_mode.layouts["<device id>"]`.

Supported Device-layout keys:

- `group_order`
- `show_activity`, `show_dpad`, `show_nav`
- `show_volume`, `show_channel`, `show_media`, `show_dvr`, `show_colors`, `show_abc`
- `show_commands_button`, `show_device_toggle`
- `c_as_rows`, `c_row_visible_rows`

`c_as_rows` places Commands in the `macros_row` position instead of a drawer. `c_row_visible_rows` accepts 1–6 and defaults to 2.
