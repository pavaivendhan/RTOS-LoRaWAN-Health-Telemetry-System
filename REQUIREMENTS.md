# Project Requirements & Non-Requirements

This document defines the strict scope of the **RTOS-Driven LoRaWAN Health Telemetry System**. It is broken down into what the system *must* do (Requirements) and what the system is explicitly designed *not* to do (Non-Requirements).

---

## ✅ Functional Requirements (What the system MUST do)
1. **Vitals Acquisition:** The system must continuously poll the MAX30100 (HR & SpO2), DS18B20 (Temperature), and AD8232 (ECG) sensors.
2. **Leads-Off Detection:** The system must hardware-detect if the ECG pads have fallen off the patient and report an error state.
3. **Local Visualization:** The system must render real-time vitals locally on both a 20x4 LCD and a 128x64 OLED display via the I2C bus.
4. **Data Compression:** The system must compress the float and integer sensor readings into a highly efficient, 6-byte hexadecimal array to comply with LoRaWAN payload limits.
5. **Wireless Telemetry:** The system must transmit the compressed payload over unlicensed radio bands (868MHz/915MHz) to a LoRa Gateway via The Things Network (TTN).
6. **Concurrent Execution:** The system must use FreeRTOS to isolate sensor polling, display updating, and radio transmission into separate threads, ensuring the UI never freezes while waiting for the network.

## ⚡ Non-Functional Requirements (How the system MUST behave)
1. **Power Efficiency:** The system must consume significantly less power than a traditional Wi-Fi or Cellular edge device.
2. **Cost-Effectiveness:** Telemetry must be transmitted with **zero recurring subscription costs** (no SIM card fees).
3. **Reliability:** Network failures (e.g., failing to join TTN) must not halt the local monitoring loops. The device must continue to function as a local bedside monitor even if radio transmission fails.
4. **Range:** The radio telemetry must be capable of penetrating building walls and reaching a gateway up to several miles away.
5. **Security:** Payload data must be secured over the air using LoRaWAN's standard AES-128 end-to-end encryption.

---

## ❌ Non-Requirements / Out of Scope (What the system will NOT do)
These features are explicitly excluded from the current design scope to maintain system efficiency, reduce hardware costs, and adhere to the core problem statement.

1. **No Cellular / Wi-Fi / Bluetooth:** The system will *not* use heavy, power-hungry protocols. All telemetry is strictly confined to LoRaWAN.
2. **No Local Data Logging:** The edge device will *not* write historical patient data to a local SD card. All historical archiving must be handled by the cloud backend after TTN routes the MQTT payload.
3. **No Automated Diagnosis:** The system is purely a telemetry relay. It will *not* use AI or algorithms to diagnose diseases, prescribe medication, or act as a certified medical device.
4. **No Smartphone App Pairing:** The device will *not* require a Bluetooth pairing process with a smartphone to function. It is a standalone edge device that connects directly to TTN gateways.
5. **No Actuation:** The system will *not* trigger physical alarms, IV drips, or defibrillators. It is strictly a read-only monitoring architecture.
