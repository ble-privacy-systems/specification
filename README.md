# specification
Documentation detailing the BLE protocol structure, UUIDs, data bytes, and file metadata tagging rules

short intro: TODO

# BLE Privacy Signal (BLEPS) 📵

A recording LED on smart glasses is a guess. The device already knows if the camera is live, but a bystander across the table has no radio way to say *"do not record me."* 

**BLE Privacy Signal (BLEPS)** is an open, preference-based Bluetooth protocol that bridges this gap. Bystanders can broadcast a privacy request, and recording devices (smart glasses, AI badges, doorbells, robots, and cars) can transparently declare their operational state.

This is a **preference protocol**, not a hardware kill-switch. It acts as a digital social convention.

---

## 🚀 Core Marks & States

The protocol maps hardware states using standard symbols inside local device names:

* 📵 **Bystander:** *"Please do not record me"* (Do-Not-Record / DNR request).
* 📷 **Camera Device:** Camera on board, but currently idle.
* 📸 **Recording:** Photo or video capture is actively live.
* 🎤 **Microphone Live:** Audio stream is actively recording.

---

## 🛠️ Technical Specification

### 1. Workshop UUIDs
Until official 16-bit assignments are granted by the Bluetooth SIG, the protocol uses:
* **Recording Presence Service:** `0000f1a0-0000-1000-8000-00805f9b34fb`
* **Do-Not-Record Request:** `0000f1a3-0000-1000-8000-00805f9b34fb`

### 2. Service Data Byte (1-Byte Bit Field)
A single byte handles state transitions locally without needing internet or cloud lookup tables:
* **`0x20`** = 📵 Bystander DNR Beacon.
* **`0x11`** = Idle camera that honors DNR.
* **`0x13`** = Camera recording live.

---

## 📱 App Roadmap: Making Recognition Easy

To make setup simple for everyday users, mobile bystander applications will use multiple zero-configuration layers:

* **Layer 1: BLE & Bluetooth Names (Current)** — Apps instantly scan for raw Bluetooth friendly names containing the emojis (e.g., `📵 DNR` or `PMSG_📸`) without needing pairing handshakes.
* **Layer 2: Wi-Fi SSIDs (Added Later)** — To bypass mobile background BLE restrictions, fixed hardware like smart doorbells, home robots, or vehicles can append states directly to local Wi-Fi network names (e.g., `MyHome_📷`). Apps can detect these signals passively over the air.

---

## 💾 Media Metadata Stamping

When a compliant device captures video while a local DNR signal is active, it stamps the file manifest:

```html
<meta name="pmsg:dnr-present" content="true">
<meta name="pmsg:protocol" content="ble-privacy-signal/0.1">
<meta name="pmsg:capture_state" content="video">
```
This allows social media platforms and publishing sites to recognize user preferences downstream.
