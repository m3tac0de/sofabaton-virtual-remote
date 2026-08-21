![HEADER](screenshots/virtualremote-header.png)

# Sofabaton Virtual Remote for Home Assistant

[![HACS Badge](https://img.shields.io/badge/HACS-Default-green.svg)](https://github.com/hacs/integration)
![Version](https://img.shields.io/badge/version-0.2.1-blue)

A highly customizable virtual remote for your lovelace dashboard. It works with the **Sofabaton X1, X1S, and X2** remotes.

> [!NOTE]
> This is an unofficial community project and is not affiliated with or endorsed by Sofabaton.

## ⚠️ Before installing

**This card does not work standalone**, it is a frontend component only. It is dependent on an `integration` that communicates with the hub, via Home Assistant's backend.
You will need to have that integration installed and working, before you can use this card.

2 options:

- **[X1, X1S, X2]** Install and configure the **Sofabaton X** integration via [`HACS`](https://my.home-assistant.io/redirect/hacs_repository/?owner=m3tac0de&repository=home-assistant-sofabaton-x1s&category=integration) or [`Github`](https://github.com/m3tac0de/home-assistant-sofabaton-x1s).
- **[X2]** Install and configure the **Official Sofabaton Hub** integration via [`HACS`](https://my.home-assistant.io/redirect/hacs_repository/?owner=yomonpet&repository=ha-sofabaton-hub&category=integration) or [`Github`](https://github.com/yomonpet/ha-sofabaton-hub).

## ✨ Features

- **It's your remote, in Home Assistant**: Mirrors how you've set up your physical remote. Control Activities, including their macros and favorites.
- **Works with all Sofabaton hubs**: Compatible with the Sofabaton X1, X1S, and X2 hubs.
- **Theming friendly**: The virtual remote plays nice with your dashboard's theme, or override it for a different one.
- **Custom Layouts**: Show only the button groups you need (Direction Pad, Volume, etc.), and create layouts per Activity.
- **Device mode** _(Sofabaton X integration only)_: Control any device configured on the hub, using a device's button bindings and complete, searchable command list. Device mode works independently of Activities. See [`docs/device_mode.md`](docs/device_mode.md).
- **Hold-to-repeat**: Hold a selected Volume, Channel, or Direction Pad button to send its command repeatedly, as on the physical remote. This is off by default, and you can choose which button groups take part.
- **Automation Assist**: Record keypresses in the virtual remote and receive ready-to-use YAML for your own dashboard or automations. X2 users can also create MQTT device triggers.
- **Responsive Design**: The card scales to however much space it has. Tweak its behavior by setting a maximum width.
- **Configure via the UI**: No need for YAML.

## 📸 Screenshots

<img src="https://raw.githubusercontent.com/m3tac0de/sofabaton-virtual-remote/refs/heads/main/screenshots/virtual-remote-01.png" width="220"> <img src="https://raw.githubusercontent.com/m3tac0de/sofabaton-virtual-remote/refs/heads/main/screenshots/virtual-remote-02.png" width="220"> <img src="https://raw.githubusercontent.com/m3tac0de/sofabaton-virtual-remote/refs/heads/main/screenshots/virtual-remote-03.png" width="220">

---

## 🚀 Installation

### Via HACS (Recommended)

1. Open **HACS** in Home Assistant.
2. Search for "Sofabaton Virtual Remote" and click **Download**.

### Manual Installation

1. Download the `sofabaton-virtual-remote.js` from the [latest release](https://github.com/m3tac0de/sofabaton-virtual-remote/releases).
2. Upload it to your `<config>/www/` directory.
3. Add the resource to your Dashboard configuration:
   - **URL:** `/local/sofabaton-virtual-remote.js`
   - **Type:** `JavaScript Module`

---

## 🛠 Configuration

The card is best configured using the Visual Editor. Just add a new card to your dashboard and search for **Sofabaton Virtual Remote**.

Once in the card configuration panel, select your remote/hub from the dropdown. The dropdown will only contain remote entities that are compatible with the card, so you can't go wrong here.
After that, just play around with the settings.

If you prefer YAML, this is the minimal implementation:

```yaml
type: custom:sofabaton-virtual-remote
entity: remote.x2_hub # the remote entity added by the Sofabaton integration.
```

Here is the full list of options:

| Key                      | Type         | Description                                                                                                                                                                                                                                                                                                                                                       | Default                                                                                    |
| :----------------------- | :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------- |
| `entity`                 | string       | The `remote.` entity of your Sofabaton device.                                                                                                                                                                                                                                                                                                                    | **Required**                                                                               |
| `max_width`              | number       | Limits how wide the remote grows.                                                                                                                                                                                                                                                                                                                                 | `360`                                                                                      |
| `show_automation_assist` | boolean      | Enable or disable **Key capture** in General Options.                                                                                                                                                                                                                                                                                                             | `false`                                                                                    |
| `hold_repeat`            | map / object | Hold-to-repeat settings: `enabled` turns the feature on. `volume`, `channel`, and `dpad` each default to `true` when enabled; set one to `false` to exclude that group. Holding a selected button sends its command after 400 ms and repeats it every 250 ms until released. Only these three button groups support hold-to-repeat. Works with both integrations. | `{}`                                                                                       |
| `show_activity`          | boolean      | Show/hide the activity selector.                                                                                                                                                                                                                                                                                                                                  | `true`                                                                                     |
| `show_dpad`              | boolean      | Show/hide the Direction Pad.                                                                                                                                                                                                                                                                                                                                      | `true`                                                                                     |
| `show_nav`               | boolean      | Show/hide the Back, Home, and Menu buttons.                                                                                                                                                                                                                                                                                                                       | `true`                                                                                     |
| `show_volume`            | boolean      | Show/hide Volume controls.                                                                                                                                                                                                                                                                                                                                        | `true`                                                                                     |
| `show_channel`           | boolean      | Show/hide Channel controls.                                                                                                                                                                                                                                                                                                                                       | `true`                                                                                     |
| `show_mid`               | boolean      | Legacy combined switch for the Volume and Channel controls, used only when `show_volume` / `show_channel` are not set. Prefer those instead.                                                                                                                                                                                                                      | `true`                                                                                     |
| `show_media`             | boolean      | Show/hide Play/Pause, Rew, Fwd buttons.                                                                                                                                                                                                                                                                                                                           | `true`                                                                                     |
| `show_dvr`               | boolean      | Show/hide the X2 DVR, Pause, Exit buttons.                                                                                                                                                                                                                                                                                                                        | `true`                                                                                     |
| `show_colors`            | boolean      | Show/hide Red, Green, Yellow, Blue buttons.                                                                                                                                                                                                                                                                                                                       | `true`                                                                                     |
| `show_abc`               | boolean      | Show/hide the X2 A/B/C buttons.                                                                                                                                                                                                                                                                                                                                   | `true`                                                                                     |
| `show_macros_button`     | boolean      | Toggle the Macros drawer button.                                                                                                                                                                                                                                                                                                                                  | `true`                                                                                     |
| `show_favorites_button`  | boolean      | Toggle the Favorites drawer button.                                                                                                                                                                                                                                                                                                                               | `true`                                                                                     |
| `mf_as_rows`             | boolean      | When `true`, Macros and Favorites render as their own scrollable rows in the card instead of drawer buttons. Each becomes an independently-orderable row (`macros_row / favorites_row`) and the combined `macro_favorites` row is hidden.                                                                                                                         | `false`                                                                                    |
| `mf_row_visible_rows`    | number       | Number of button rows visible in each inline Macros / Favorites row before the row becomes scrollable. Shared by both rows. Range 1–6. Only effective when `mf_as_rows`: `true`.                                                                                                                                                                                  | `2`                                                                                        |
| `custom_favorites`       | list         | List of custom buttons for the drawer.                                                                                                                                                                                                                                                                                                                            | `[]`                                                                                       |
| `theme`                  | string       | Set a specific theme for this card.                                                                                                                                                                                                                                                                                                                               | `""`                                                                                       |
| `background_override`    | list/object  | Override the card background (e.g., [33, 33, 33]).                                                                                                                                                                                                                                                                                                                | `null`                                                                                     |
| `group_order`            | list         | Change the order of the button groups. Valid entries: `activity, macro_favorites, macros_row, favorites_row, dpad, nav, mid, media, colors, abc`. `macros_row / favorites_row` are only rendered when `mf_as_rows`: `true`; `macro_favorites` is only rendered when `mf_as_rows`: `false`.                                                                        | `activity, macro_favorites, macros_row, favorites_row, dpad, nav, mid, media, colors, abc` |
| `layouts`                | map / object | Layout Options per Activity. Use key `default` for the layout shared by all Activities, and an Activity id for a single Activity's override. All layout keys above (including `mf_as_rows, mf_row_visible_rows, show_macros_button, show_favorites_button, group_order`, etc.) can be set per entry.                                                              | `{}`                                                                                       |
| `device_mode`            | map / object | Device mode settings _(Sofabaton X integration only)_: `enabled`, `open_device`, and per-device `layouts`. See [`docs/device_mode.md`](docs/device_mode.md).                                                                                                                                                                                                      | `{}`                                                                                       |

Hold-to-repeat example: enable it for the Volume and Direction Pad buttons, but not for Channel:

```yaml
type: custom:sofabaton-virtual-remote
entity: remote.x2_hub
hold_repeat:
  enabled: true
  channel: false
```

Per-activity layout example: hide the color buttons everywhere, but in Activity 101 also hide the activity selector and reorder the groups:

```yaml
type: custom:sofabaton-virtual-remote
entity: remote.x2_hub
layouts:
  default:
    show_colors: false # color buttons hidden in every Activity
  "101":
    show_activity: false # Activity select hidden in Activity 101
    group_order: # Custom group order for Activity 101
      - activity
      - dpad
      - nav
      - mid
      - media
      - colors
      - abc
      - macro_favorites
```

Macros/Favorites as inline scrollable rows: both sections become their own rows positioned where you want them. Each row shows 3 button rows at a time before scrolling:

```yaml
type: custom:sofabaton-virtual-remote
entity: remote.x2_hub
layouts:
  default:
    mf_as_rows: true
    mf_row_visible_rows: 3
    group_order:
      - activity
      - dpad
      - nav
      - mid
      - macros_row # inline scrollable macros row
      - favorites_row # inline scrollable favorites row
      - media
      - colors
      - abc
  "101":
    mf_as_rows: false # Activity 101 falls back to drawer buttons
```

### Device mode

_(Sofabaton X integration only)_

Device mode lets the remote control one device configured on the hub. The card uses that device's button bindings and adds a searchable drawer containing its complete command list. It works independently of Activities, so you can reach any command on any device even when no Activity is running. This is useful for occasional commands that are not part of an Activity or for building a dedicated per-device remote.

It requires the Sofabaton X integration with persistent caching enabled; the card hides all Device mode functionality when it is not available. Full documentation, including the `device_mode` configuration block and per-device layouts: [`docs/device_mode.md`](docs/device_mode.md).

### Automation Assist

<img src="https://raw.githubusercontent.com/m3tac0de/sofabaton-virtual-remote/refs/heads/main/screenshots/virtual-remote-04.png" height="300"> <img src="https://raw.githubusercontent.com/m3tac0de/sofabaton-virtual-remote/refs/heads/main/screenshots/virtual-remote-05.png" height="300"> <img src="https://raw.githubusercontent.com/m3tac0de/sofabaton-virtual-remote/refs/heads/main/screenshots/virtual-remote-06.png" height="300">

Automation Assist provides two ways to help build your own dashboards and automations. Enable **General Options → Key capture** to use them.

- **Key capture → copy/paste code (X1 / X1S / X2)**

  When enabled, the card captures button presses and Activity changes on your virtual remote and sends a Notification, available in your Home Assistant sidebar, containing YAML to reproduce that button press in:
  - your dashboard (a Lovelace button that triggers the same command)
  - a script / automation action (a ready-to-use service call)

  For more details, see here [`docs/keycapture.md`](docs/keycapture.md).

- **MQTT device triggers (X2 only)**

  This feature creates descriptive Home Assistant triggers for MQTT commands and Activity changes — without having to copy/paste MQTT topics and JSON payloads by hand.

  Instead of:
  - **Topic**: `F19879827938423/up`
  - **Payload**: `{"device_id":1,"key_id":1}`

  You get:
  - **Device**: `X2 → [YOUR DEVICE NAME]`
  - **Trigger**: `Dim the lights`

  For more details, see here [`docs/automation_triggers.md`](docs/automation_triggers.md).

---

## License

MIT © 2026 m3tac0de.
