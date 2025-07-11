# BMS Intel Device Documentation

## Configuration files

This JSON configuration file defines the properties, registers, decode logic, and commands for a device. The file is divided into several main sections: `meta`, `json`, `registers` and `commands`.

---

### Meta

The `meta` section contains metadata about the device:

- `manufacturer`: The manufacturer of the device.
- `make`: The series or line of the device.
- `model`: The specific model of the device.
- `sku`: The Stock Keeping Unit, if applicable.
- `type`: The type of device.
- `communicationProtocol`: The communication protocol used by the device.
  - Modbus
  - MQTT
  - API
  - Direct
- `description`: (Optional) A description of the device.
- `version`, `document_version`, `document_timestamp`, `comment`: (Optional) Versioning and documentation fields.

---

### JSON

The `json` section describes how to interpret incoming data:

- `topic`: The MQTT topic for the device.
- `deviceIdField`: The field containing the device ID.
- `payloadField`: The field containing the payload data.
- `timestampField`: The field containing the timestamp.
- `keys`: An array of objects mapping additional fields (e.g., gatewayId, rssi, fCnt).
- `payloadEncoding`: The encoding of the payload (e.g., Base64).
- `payloadFormat`: The format of the payload (e.g., tagValue, positionBased).

---

### Registers

The `registers` section is an array of objects, each representing a register of the device. Each register object can have the following properties:

- `name`: The human-readable name of the register.
- `category`: The category of the register (e.g., Power, Measurement, Status).
- `description`: A description of the register.
- `dataKey`: The key used for this register in InfluxDB and payloads.
- `addressType`: The type of address, typically "Register".
- `address` or `startAddress`: The specific address of the register.
- `minValue` / `maxValue`: The minimum and maximum values for the register.
- `defaultValue`: The default value for the register.
- `units`: The units of the value.
- `dataType`: The data type of the value (e.g., Word, DoubleWord).
- `multiplier`: A multiplier applied to the raw value to get the actual value.
- `rw`: Indicates if the register is readable ("r"), writable ("w"), or both ("rw").
- `reportInterval`: The interval, in seconds, at which the device reports the value.
- `length`: The length of the value in bytes or words.
- `states`: (Optional) A mapping of possible values to human-readable states.
- `display`: (Optional) If false, the register is hidden from UI.

---

### Commands

The `commands` section (optional) is an array of objects, each representing a command that can be sent to the device. Each command object can have:

- `name`: The name of the command.
- `description`: A description of what the command does.
- `topic`: The MQTT topic to send the command to.
- `payloadTemplate`: A template for the command payload, with placeholders for dynamic values.
- `encoding`: The encoding format for the command payload (e.g., Hex).

---

### Example

```json
{
  "meta": {
    "device": "Sensor",
    "manufacturer": "Manufacturer Name",
    "make": "Device Series",
    "model": "Device Model",
    "sku": "SKU",
    "type": "Device Type",
    "communicationProtocol": "Communication Protocol",
    "description": "Device description"
  },
  "json": {
    "topic": "/devices/device-topic",
    "deviceIdField": "devEUI",
    "payloadField": "data",
    "timestampField": "time",
    "keys": [
      { "name": "gatewayId", "path": "rxInfo.gatewayID" },
      { "name": "rssi", "path": "rxInfo.rssi" },
      { "name": "fCnt", "path": "fCnt" }
    ],
    "payloadEncoding": "Base64",
    "payloadFormat": "tagValue"
  },
  "registers": [
    {
      "name": "Battery",
      "category": "Power",
      "description": "Battery Level in %",
      "dataKey": "battery",
      "addressType": "Register",
      "address": "0175",
      "minValue": 0,
      "maxValue": 100,
      "defaultValue": 0,
      "units": "%",
      "dataType": "Word",
      "multiplier": 1,
      "rw": "r",
      "reportInterval": 21600,
      "length": 1
    }
    // ... more registers ...
  ],
  "commands": [
    {
      "name": "Set Reporting Interval",
      "description": "Set the reporting interval for the sensor",
      "topic": "/devices/device-topic/commands",
      "payloadTemplate": {
        "devEui": "{{devEui}}",
        "registerAddress": "FF03",
        "value": "{{interval}}"
      },
      "encoding": "Hex"
    }
    // ... more commands ...
  ]
}
```

---

This structure allows for a flexible configuration that can be adapted to different devices, their registers, decoding logic, and supported commands. The `meta` section provides a high-level overview, while `registers`, `decode`, and `commands` provide detailed information for integration and control.
