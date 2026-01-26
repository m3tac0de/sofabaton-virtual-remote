# Automation Assist (MQTT Device Triggers)

Automation Assist helps you create **descriptive Home Assistant triggers** for your Sofabaton MQTT remote — without having to copy/paste MQTT topics and JSON payloads by hand.

## Why this exists

Automation Assist enhances the current MQTT related workflow with **MQTT Discovery Device Triggers**:

- No manual topic/payload copy-pasting
- Triggers appear as **real device triggers** in Home Assistant
- Triggers are **human-readable and descriptive**, so your automation UI shows meaningful button names instead of opaque JSON

This uses Home Assistant’s MQTT Device Trigger mechanism:  
- https://www.home-assistant.io/integrations/device_trigger.mqtt/

## What you get after creating triggers

After you click **Create Triggers** they are instantly available in Home Assistant:

- Go to **Settings → Automations & Scenes → Automations → Create Automation**
- Choose **Add Trigger > Device**
- Select your MQTT device, it's named  **`X2 -> [Your Device Name]`**
- Pick a button/command from a descriptive list

  *   **Device:** `X2 -> [Your Device Name]`
  *   **Trigger:** `Dim Lights`

## What Automation Assist does (in plain terms)

When **Automation Assist** is enabled:

1. The card subscribes to the MQTT keypress topic.
2. As soon as you press a button that belongs to an MQTT/Home Assistant device, we detect the keypress.
3. That keypress contains a **device_id**.
4. Using that device_id, the card looks up **all commands for that same device** (this lookup is invisible to you).
5. The card then offers a button to **Create MQTT Discovery Triggers** for that device.
6. When clicked, a new MQTT topic is created for each command, and retained under **homeassistant/device_automation** in your broker. By merely creating these, we've added the triggers to Home Assistant.

### Important: the triggers work without the card

These triggers are an **MQTT + Home Assistant feature**, they are NOT a feature of this card.

- Your automations will keep working even if the Lovelace card is removed, broken, or unavailable.
- The card is only used to **set up / update** the triggers.



---
# Keeping triggers in sync

Automation Assist **does not automatically sync** triggers when your remote configuration changes.

If you make changes to your remote config (rename commands, change mappings, etc.):

✅ You should return to Automation Assist and click **Create Triggers** again.

### What “Create Triggers” does when you run it again

It **updates / publishes the MQTT discovery topics** again so Home Assistant sees the latest intended values.

Think of it as “refresh/update discovery”.

---

# Ghost triggers (when buttons were removed)

There’s one important edge case:

- If you **remove buttons/commands** from your config
- And then you click **Create Triggers** again

⚠️ The removed ones will **not** be automatically deleted from Home Assistant.

That means you may end up with **stale / “ghost” triggers** that still show up as options in the UI, even though they no longer exist on the remote.

## How to remove ghost triggers (recommended cleanup)

If you want to fully clean out stale triggers:

1. Go to **Settings > Devices & Services > MQTT**.
2. Find the device named **`X2 -> [Your Device Name]`**.
3. **Delete** this device entirely. This only removes the triggers. It has no other impact.
4. Go back to your Virtual Remote Card and click the **Create** button again.

After that, Home Assistant will rediscover only the current, valid buttons.

Ultimately, creating and then maintaining these triggers would be an excellent firmware feature. Only the hub can do this properly and fully automatically.
