# Sofabaton Virtual Remote for Home Assistant

[![HACS Badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/hacs/integration)
![Version](https://img.shields.io/badge/version-0.0.1-blue)

A highly customizable virtual remote for your lovelace dashboard. It works with the **Sofabaton X1, X1S, and X2** remotes.

## ⚠️ Before installing

This card does not work standalone, it is a frontend component only. It is dependent on an integration that actually communicates with the hub, via Home Assistant's backend.
You will need to have this integration installed and working, before you can use this card.

  - If you have an **X1 or X1S** remote, install and configure the [`Sofabaton X1S integration`](https://github.com/m3tac0de/home-assistant-sofabaton-x1s).
  - If you have an **X2** remote, install and configure the [`Sofabaton Hub integration`](https://github.com/yomonpet/ha-sofabaton-hub).


## ✨ Features
* **It's your remote, in Home Assistant**: Control activities, navigation, volume, and media playback.
* **Works with all Sofabaton hubs**: Compatible with the Sofabaton X1, X1S, and X2 hubs.
* **Theming friendly**: The virtual remote plays nice with your dashboard's theme, or override it for different one.
* **Custom Layouts**: Show only the button groups you need (D-pad, Volume, etc.).
* **Macros & Favorites**: Your macros and favorites are right there in the virtual remote.
* **Responsive Design**: Adjustable card width and background customization to fit any dashboard.
* **Configure via the UI**: No need for YAML.



## 📸 Screenshots

<img src="screenshots/virtual-remote-01.png" width="220"> <img src="screenshots/virtual-remote-02.png" width="220"> <img src="screenshots/virtual-remote-03.png" width="220">


---

## 🛠 Configuration
The card features a robust visual editor, allowing you to customize your remote experience without touching YAML.

### Styling Options
* **Apply a Theme**: Quickly switch between visual styles like Light, Dark, or Glass.
* **Maximum Card Width**: Fine-tune the size of your remote card in pixels (e.g., 360px).
* **Customize Background**: Enable custom background colors to perfectly match your Home Assistant dashboard.

### Button Visibility
Tailor the remote to your specific needs by toggling the following sections:
* **Activity Selector**: Quickly switch between your configured hub activities.
* **Direction Pad**: Show or hide the main navigation cluster.
* **Back/Home/Menu Keys**: Toggle the essential navigation row.
* **Volume/Channel Rockers**: Dedicated controls for audio and channel surfing.
* **Media Playback**: Play, pause, and skip controls.
* **Red/Green/Yellow/Blue**: Classic colored buttons for specific device functions.
* **A/B/C Buttons (X2 only)**: Specialized support for the additional hardware buttons on the X2 remote.
* **Macros/Favorites**: Access your most-used commands and channels.

---

## 🚀 Installation

### Via HACS (Recommended)
1. Open **HACS** in Home Assistant.
2. Click the three dots in the top right and select **Custom repositories**.
3. Add `https://github.com/m3tac0de/sofabaton-virtual-remote` with the type **Dashboard**.
4. Search for "Sofabaton Virtual Remote" and click **Download**.

### Manual Installation
1. Download the `sofabaton-virtual-remote.js` from the [latest release](https://github.com/m3tac0de/sofabaton-virtual-remote/releases).
2. Upload it to your `<config>/www/` directory.
3. Add the resource to your Dashboard configuration:
   - **URL:** `/local/sofabaton-virtual-remote.js`
   - **Type:** `JavaScript Module`

---
