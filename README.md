# Govee to MQTT bridge for Home Assistant

> **⚠️ Fork notice — Mosquitto 7.x fix**
>
> This is a temporary fork of [wez/govee2mqtt](https://github.com/wez/govee2mqtt) that replaces
> the `/` in the MQTT client ID with `-`. Mosquitto 7.0+ rejects the upstream client ID
> (`govee2mqtt/<uuid>`) as a "dangerous client ID" due to stricter ACL rules, causing the
> upstream addon to fail to connect. Use this fork as a drop-in replacement until
> [the upstream repo](https://github.com/wez/govee2mqtt) ships a fix.
>
> ### How to switch (Home Assistant OS / Supervised)
>
> 1. **Uninstall** the upstream "Govee to MQTT Bridge" addon. Copy your config somewhere first
>    (API key, email, password, MQTT settings) — you'll paste it back in step 4.
> 2. **Add this fork as an addon repository:** Settings → Add-ons → Add-on Store →
>    ⋮ (top-right) → Repositories → paste `https://github.com/radaiko/govee2mqtt` → Add.
> 3. **Install** "Govee to MQTT Bridge" from the newly added *radaiko fork* source in the store.
> 4. **Restore** your configuration and start the addon.
> 5. Check the addon log — the MQTT client ID should now look like `govee2mqtt-<uuid>` (with
>    a dash, not a slash) and Mosquitto should accept it.
>
> Once upstream merges the fix, remove this repository and reinstall the official addon.
> See commit [`2520244`](https://github.com/radaiko/govee2mqtt/commit/2520244) for the
> one-line change.

This repo provides a `govee` executable whose primary purpose is to act
as a bridge between [Govee](https://govee.com) devices and Home Assistant,
via the [Home Assistant MQTT Integration](https://www.home-assistant.io/integrations/mqtt/).

## Features

* Robust LAN-first design. Not all of Govee's devices support LAN control,
  but for those that do, you'll have the lowest latency and ability to
  control them even when your primary internet connection is offline.
* Support for per-device modes and scenes.
* Support for the undocumented AWS IoT interface to your devices, provides
  low latency status updates.
* Support for the new [Platform
  API](https://developer.govee.com/reference/get-you-devices) in case the AWS
  IoT or LAN control is unavailable.

|Feature|Requires|Notes|
|-------|--------|-------------|
|DIY Scenes|API Key|Find in the list of Effects for the light in Home Assistant|
|Music Modes|API Key|Find in the list of Effects for the light in Home Assistant|
|Tap-to-Run / One Click Scene|IoT|Find in the overall list of Scenes in Home Assistant, as well as under the `Govee to MQTT` device|
|Live Device Status Updates|LAN and/or IoT|Devices typically report most changes within a couple of seconds.|
|Segment Color|API Key|Find the `Segment 00X` light entities associated with your main light device in Home Assistant|

* `API Key` means that you have [applied for a key from Govee](https://developer.govee.com/reference/apply-you-govee-api-key)
  and have configured it for use in govee2mqtt
* `IoT` means that you have configured your Govee account email and password for
  use in govee2mqtt, which will then attempt to use the
  *undocumented and likely unsupported* AWS MQTT-based IoT service
* `LAN` means that you have enabled the [Govee LAN API](https://app-h5.govee.com/user-manual/wlan-guide)
  on supported devices and that the LAN API protocol is functional on your network

## Usage

* [Installing the HASS Add-On](docs/ADDON.md) - for HAOS and Supervised HASS users
* [Running it in Docker](docs/DOCKER.md)
* [Configuration](docs/CONFIG.md)

## Have a question?

* [Is my device supported?](docs/SKUS.md)
* [Check out the FAQ](docs/FAQ.md)

## Want to show your support or gratitude?

It takes significant effort to build, maintain and support users of software
like this. If you can spare something to say thanks, it is appreciated!

* [Sponsor me on Github](https://github.com/sponsors/wez)
* [Sponsor me on Patreon](https://patreon.com/WezFurlong)
* [Sponsor me on Ko-Fi](https://ko-fi.com/wezfurlong)
* [Sponsor me via liberapay](https://liberapay.com/wez)

## Credits

This work is based on my earlier work with [Govee LAN
Control](https://github.com/wez/govee-lan-hass/).

The AWS IoT support was made possible by the work of @bwp91 in
[homebridge-govee](https://github.com/bwp91/homebridge-govee/).

