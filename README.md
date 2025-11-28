# SAJ Portal Scraper Add-on for Home Assistant

This Home Assistant add-on enables automated retrieval of live data from the **SAJ Solar Portal** using **Playwright**, and publishes the filtered values via **MQTT**.

---

## 🚀 Features

**Tested Portal:** This add-on has only been tested with the SAJ portal URL:

```
https://eop.saj-electric.com
```

Other SAJ portal instances or regional servers may behave differently. Compatibility with those servers cannot be guaranteed.

**Note:** SAJ does *not* provide an official API. This add-on exclusively uses publicly visible network requests of the web portal that occur during a normal user login in a browser. No security mechanisms are bypassed, and no private API endpoints are accessed.

* Automatic login to the SAJ web portal via Playwright
* Captures the HTTP response `getDeviceEneryFlowData`
* Filters the returned JSON data based on `filter_fields.json`
* Publishes the filtered data via MQTT
* Error detection with automatic screenshots (keeps only the 5 newest screenshots)
* Configurable scan time, MQTT settings, and SAJ portal URL

---

## 🔧 How It Works & Parameter Explanation

The add-on performs an automated login to the SAJ portal and waits for the network request `getDeviceEneryFlowData`, which contains the current energy and status data of the inverter.

Only when this request occurs inside the portal, the data is collected, filtered, and published via MQTT.

### Configuration Fields Explained

| Field             | Description                                            |
| ----------------- | ------------------------------------------------------ |
| `saj_username`    | Login name or email for the SAJ portal                 |
| `saj_password`    | Password for the SAJ portal                            |
| `mqtt_broker`     | Hostname/IP of the MQTT broker                         |
| `mqtt_port`       | MQTT port (default: 1883)                              |
| `mqtt_username`   | Optional MQTT username                                 |
| `mqtt_password`   | Optional MQTT password                                 |
| `mqtt_topic`      | Topic used to publish filtered data                    |
| `scan_time`       | Delay in seconds after login to wait for incoming data |
| `saj_url`         | SAJ portal URL (if using an alternative domain)        |
| `screenshot_path` | Directory path for error screenshots                   |

---

## 📦 Installation

1. Add the repository in Home Assistant under **Add-on Store → Repositories**
2. Install the add-on
3. Adjust the configuration
4. Start the add-on

---

## 🔍 How the Add-on Works Internally

### 1. Login via Playwright

* A headless Chromium browser is launched
* The login form is filled out and submitted automatically

### 2. Capturing the HTTP Request

The add-on listens for responses containing:

```
getDeviceEneryFlowData
```

The captured JSON data is then:

* filtered
* logged
* published via MQTT

### 3. Error Handling

If no login or no data is received:

* A screenshot is saved
* Only the **five most recent** error screenshots are kept

---
