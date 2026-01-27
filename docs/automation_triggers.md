# Automation Assist / MQTT Device Triggers

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

---

Ultimately, creating and then maintaining these triggers would be an excellent firmware feature. Only the hub can do this fully automatically.

For anyone interested, looking at code or playing with the MQTT goodness of the X2, I'll share the following insights.

Through playing with the X1S and the X2, and exploring both the APIs intended for the app and the MQTT implementation of the X2, it's fairly clear that app API and MQTT implementation derive from the same hub-internal API. 

Of course. That makes sense. 

But this introduces some interesting challenges the X2 is having to deal with. It's very clear that this hub-internal API is designed for a single user at a time. 
Actually that is not an entirely accurate statement. There is a single method in that hub-internal API that appears optimized for a multiuser environment and that is the one that lets you send a keypress to the hub. This particular command is also the only one (that I know of) that does not provide a response, ever. You can fairly safely (semi) spam the hub with fresh commands to emit, and it will be mostly fine. 

But here comes the problem: all other methods that do have a response (list of activities, commands etc) are slow and brittle. The hub-internal API appears to lacks reentrancy; initiating a second request before the first completes creates a race condition that leads to state corruption.. one but likely both requests will fail. Do this often enough and the entire method starts to fail. Do it even more frequently and the hub-internal API will fully crash. I probably own the most rebooted X2 hub outside of a Sofabaton office. 

* The Sofabaton app solves this problem on the client side. The app just locks the UI completely while awaiting a response from the hub, thereby preventing double requests. At the same time, the hub stops advertising its presence and rejects additional connections, so a second app cannot connect to the same hub, at the same time.
* The integration for the X1S (also for the X1) solves this by 1. It mimics app behavior in that it will allow only a single client at a time to connect to the proxy. 2. blocks user input when the app is connected to the proxy 3. has an internal queueing system for requests that expect a response, ensuring commands await their turn and 4. implements caching to minimize roundtrips to the hub.
* The official integration for the X2 solves this with an internal queuing system so that requests for lists await their turn and implements caching to minimize roundtrips to the hub.

But this is imperfect:

As can be concluded from the previous bullet (and as I've proven to myself with my own hub a hundred times): the hub's MQTT implementation does NOT shield it from double commands. It's only the integration that prevents that from happening. Go spam some stuff directly to the MQTT topics to see your hub crash (it's gradual degradation... first MQTT requests for lists of a particular type will start to fail, then all lists fail, then the app API fails). What's interesting is that throughout this, even when app API is fully down and there's no way to get any response from app or MQTT/integration: 1. the physical remote continues to work without issue, 2. when a button for an MQTT/Home Assistant device is pressed, this is successfully published to the broker and 3. sending a command for a key press over MQTT continues to work. So basically, everything to do with direct control seems solid, everything to do with retrieving lists of things is brittle and can make the response layer fail completely.

This is compounded by another weakness of the hub's MQTT implementation. It can timeout on a response (or perhaps this is buffer overflow). A big difference between the app API and the MQTT implementation is that when the app requests a list, the data starts flowing nearly instantly. The hub is just flushing raw data straight into the app. This is a slow but continuous feed of data, until the end of the response is reached. So when you have a long list, say the 100+ commands of your AVR, that takes forever to load in the app, but the data is incoming continuously. In the MQTT world, the hub cannot do this. There is no paging or streaming of data. Instead, the hub's MQTT implementation requests the data from the hub-internal API, which takes just as long as it takes the app to load that list, the hub now fully buffers that response, to then serialize it to JSON, to then publish it to the MQTT broker. This chain of events can easily fail. It's no doubt for this reason that Sofabaton have disabled support for retrieving lists of devices and their commands, in the official X2 integration. It's still enabled on the hub, you can get there if you bypass the integration and go directly to MQTT, but now more likely than not, you're going to make the hub fail. I also do not doubt that Activities, if sufficiently populated with lots of favorites and especially macros, can suffer the same behavior.

Because you and I are always tinkering, Sofabaton have taken the approach in the app and integration that we must always be ensured of the most up-to-date configuration. Maybe you've added a button, so let's keep requesting those lists so that when you see a UI somewhere, it's going to have that button automatically. The most infuriating moments in the app are when you're mapping buttons for your AVR, and every single time you press Select Command, it is retrieving a whole fresh list of commands from the hub. It's an acceptable and proper approach for a management/configuration application, but it's not for our home automation needs.

From my perspective, directed at Sofabaton:

- MQTT must be considered the real endpoint. In the Home Automation world you cannot have something like a remote. entity be the gatekeeper over the proper usage of your request/response model. It's Home Automation. We tinker.
- If it is, for legacy reasons, impossible to make a stable and open request/response MQTT endpoint, if gatekeeping is required to keep the hub stable, then just ditch the request/response model for a way to let us retrieve a static dataset containing all our devices, activities and their commands/keys. The hub is perfectly capable of producing this, we know this from the backup feature. If we could just somehow fetch a JSON file with all our commands, maybe from the cloud, and then put that somewhere in a Home Assistant path, we can then retain our MQTT based direct device control, while relying on a static and local dataset for our UIs, automations, triggers etc. Yes, we'll have to refetch the file when we change configuration, but this stabilizes over time.
