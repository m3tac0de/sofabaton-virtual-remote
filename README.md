![HEADER](screenshots/virtualremote-header.png)

# Sofabaton Virtual Remote for Home Assistant

[![HACS Badge](https://img.shields.io/badge/HACS-Default-green.svg)](https://github.com/hacs/integration)
![Version](https://img.shields.io/badge/version-0.0.6-blue)

A highly customizable virtual remote for your lovelace dashboard. It works with the **Sofabaton X1, X1S, and X2** remotes.

## ⚠️ Before installing
**This card does not work standalone**, it is a frontend component only. It is dependent on an `integration` that communicates with the hub, via Home Assistant's backend.
You will need to have that integration installed and working, before you can use this card.

  - If you have an **X1 or X1S** remote, install and configure the **Sofabaton X1S** integration via [`HACS`](https://my.home-assistant.io/redirect/hacs_repository/?owner=m3tac0de&repository=home-assistant-sofabaton-x1s&category=integration) or [`Github`](https://github.com/m3tac0de/home-assistant-sofabaton-x1s).
  - If you have an **X2** remote, install and configure the **Sofabaton Hub** integration via [`HACS`](https://my.home-assistant.io/redirect/hacs_repository/?owner=yomonpet&repository=ha-sofabaton-hub&category=integration) or [`Github`](https://github.com/yomonpet/ha-sofabaton-hub).

## ✨ Features
* **It's your remote, in Home Assistant**: Mirrors how you've set up your physical remote, including macros and favorites.
* **Works with all Sofabaton hubs**: Compatible with the Sofabaton X1, X1S, and X2 hubs.
* **Theming friendly**: The virtual remote plays nice with your dashboard's theme, or override it for different one.
* **Custom Layouts**: Show only the button groups you need (D-pad, Volume, etc.), and change their order too.
* **Custom Favorites**: For the virtual remote only: make your own favorites that send any command or call any Home Assistant action.
* **Responsive Design**: The card scales to however much space it has. Tweak its behavior by setting a maximum width.
* **Configure via the UI**: No need for YAML*. (*except for custom favorites)

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
entity: remote.x2_hub    # the remote entity added by the Sofabaton integration.
```

Here is the full list of options:

| Key | Type | Description | Default |
| :--- | :--- | :--- | :--- |
| `entity` | string | The `remote.` entity of your Sofabaton device. | **Required** |
| `max_width` | number | Limits how wide the remote grows. | `360` |
| `show_activity` | boolean | Show/hide the activity selector. | `true` |
| `show_dpad` | boolean | Show/hide the directional pad. | `true` |
| `show_nav` | boolean | Show/hide Volume and Channel controls. | `true` |
| `show_mid` | boolean | Show/hide Home, Menu, and Back buttons. | `true` |
| `show_media` | boolean | Show/hide Play, Pause, Rew, Fwd. | `true` |
| `show_colors` | boolean | Show/hide Red, Green, Yellow, Blue buttons. | `true` |
| `show_abc` | boolean | Show/hide the X2 A/B/C buttons. | `true` |
| `show_macros_button`| boolean | Toggle the Macros drawer button. | `true` |
| `show_favorites_button`| boolean | Toggle the Favorites drawer button. | `true` |
| `custom_favorites` | list | List of custom buttons for the drawer. | `[]` |
| `theme` | string | Set a specific theme for this card. | `""` |
| `background_override` | list/object | Override the card background (e.g., [33, 33, 33]). | `null` |
| `group_order` | list | Change the order of the button groups. | `activities, macro_favorites, dpad, nav, mid, media, colors, abc` |

### Custom Favorites Example
You can add buttons to the favorites drawer that trigger specific hub commands or standard Home Assistant actions.
Note that these buttons will only exist on your virtual remote, unfortunately not on your real one.
Also, you can only configure these with YAML.

```YAML
custom_favorites:
  - name: "Netflix"
    icon: "mdi:netflix"
    command_id: 148                      # Or use key_id, either works
    device_id: 3                         # Or use activity_id, either works
  - name: "Lights Off"
    icon: "mdi:lightbulb-off"
    tap_action:
      action: call-service
      service: light.turn_off
      target:
        entity_id: light.living_room
```


---
## License
MIT © 2026 m3tac0de.
