# BMS Intel Device Documentation

## Configuration files

This JSON configuration file is used to define the properties and registers of a device. The file is divided into two main sections: meta and registers.

#### Meta

The meta section contains metadata about the device:

- `device`: The type of device.
- `manufacturer`: The manufacturer of the device.
- `make`: The series or line of the device.
- `model`: The specific model of the device.
- `sku`: The Stock Keeping Unit, if applicable.
- `type`: The type of device.
- `communicationProtocol`: The communication protocol used by the device.
- 'document_version': 1.1: New Apr 15-2024, Clay added this to track version compatibility
- 'document_timestamp': 1713219896: New Apr 15-2024 Clay added this to track versions
- 'comment': " Penteon indoor air quality sensor": New Apr 15-2024 Clay addes this for additional detail on the device itself. May be useful for the customer if displayed on the web interface for explaination of what the device is.

##### Supported device types

| Device Type            |
| ---------------------- | ---------------------------------- |
| Thermostat             |
| Power Meter            |
| HVAC Controls          | // modified by Clay April 15, 2024 |
| Fire Alarm             |
| Elevator               |
| EV Charger             |
| Water Meter            |
| Gas Meter              |
| Power Meter            |
| Air Quality Sensor     | // Added by Clay April 15, 2024    |
| Humidity Sensor        |
| Solar Panel            |
| Heat Pump              |
| Water Heater           |
| Smoke Detector         |
| CO Detector            |
| Door Sensor            |
| Battery Bank           |
| AC Power Panel         |
| Generic Modbus Gateway |

#### Registers

The registers section is an array of objects, each representing a register of the device. Each register object has the following properties:

- `name`: The human-readable name of the register.
- `influxName`: The name used for this register in InfluxDB.
- `addressType`: The type of address, typically "Register".
- `address`: The specific address of the register.
- `minValue`: The minimum value that this register can hold.
- `maxValue`: The maximum value that this register can hold.
- `units`: The units of the value.
- `dataType`: The data type of the value.
- `multiplier`: A multiplier applied to the raw value to get the actual value.
- `rw`: Indicates if the register is readable ("r"), writable ("w"), or both ("rw").
- `reportInterval`: The interval, in seconds, at which the device reports the value of this register.

This structure allows for a flexible configuration that can be adapted to different devices and their specific registers. The meta section provides a high-level overview of the device, while the registers section provides detailed information about each register of the device.

```
{
  "meta": {
    "device": "Sensor",
    "manufacturer": "Manufacturer Name",
    "make": "Device Series",
    "model": "Device Model",
    "sku": "SKU",
    "type": "Device Type",
    "communicationProtocol": "Communication Protocol"
  },
  "registers": [
    {
      "name": "Register Name 1",
      "influxName": "influxName_1",
      "addressType": "Address Type",
      "address": "Address 1",
      "minValue": "Min Value",
      "maxValue": "Max Value",
      "units": "Units",
      "dataType": "Data Type",
      "multiplier": "Multiplier",
      "rw": "Read/Write",
      "reportInterval": "Report Interval"
    },
    ...
  ]
}
```
