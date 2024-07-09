# BMS Intel Device DECODE Documentation

## The reason for this section in BMSIntel is to provide a standardized way to decode packets sent from devices. Typically a device reports on a continuous basis on a specific interval and most devices transmit in base64 format to save on transmission space. The idea here is to identify how to break down a data transmission into specific data elements

Within a typical bas64 transmission, specific data points - one or more are encoded one of two ways: As a Binary or as Hexadecimal number/string.

Binary numbers are used for error flags and other types of data and thus sending a single Hexadecimal byte can provide 8 different boolean flags.

Hexadecimal numbers/strings can consist of a single or multiple bytes.

Example Smartico Meter:  
 Raw base64 received packet -> AACAAwCAAADeg==
Decoded packet in Hex -> 0c 00 02 00 0c 02 00 00 03 7a

In this example :  
 Status - 2 bytes, Model - 1 Byte, Serial - 3 bytes, volume = 4 bytes

The Status and Model are both bitmaps - binary representations, the rest unsigned integers

My thoughts are that all we need to do is identify the above and then we can automate the data stream processing engine programming as well by using a JSON.

Uplinks (and downlinks) could thus be preset as a JSON.

THUS a DECODE for uplinks would look (Something) like the following

```
  "meta": {
    "device": "Sensor",
    "manufacturer": "Smartico",
    "make": "Ultrasonic Gas Meter",
    "model": "GG-1.6, G-2.5, G-4.0, G-6.0",
    "sku": "GMU3.1",
    "type": "Gas Meter",
    "communication_protocol": "Direct",
    "packetType": "Base64",
    "decode": [
      "0x04", "0x41"
    ]
  },

  "0x04": {
      "message_Name": "MSG_UP_GAS_METER",
      "decodedLen" : 10,
      "payload" : [
      {
        "payload_1_Len": 1,
          "payload1Name": "status1",
          "payloadType": "bitmap",
          "payloadDesc": "Register of gas meter status #1",
          "value0" : "FLG_LOW_BAT",
          "value1" : "FLG_MOTION_DETECT",
          "value2" : "FLG_MAGNET_DETECT",
          "value3" : "FLG_TAMPER_DETECT",
          "value4" : "STS_VALVE",
          "value5" : "STS_VALVE",
          "value6" : "STS_VALVE",
          "value7" : "FLG_ERR_OVERFLOW"
        },
        {
          "payload_2_Len": 1,
          "payload1Name": "status2",
          "payloadType": "bitmap",
          "payloadDesc": "Register of gas meter status #2",
          "value0" : "FLG_ERR_REVERSE",
          "value1" : "FLG_ERR_SENSOR",
          "value2" : "FLG_ERR_GAS",
          "value3" : "FLG_ERR_TIME",
          "value4" : "RESERVED",
          "value5" : "FLG_POWER_ON",
          "value6" : "FLG_LOCK",
          "value7" : "FLG_CFG_DONE"
        },
        {
          "payload_3_Len": 1,
          "payload1Name": "Model",
          "payloadType": "bitmap",
          "payloadDesc": "Gas meter model",
          "value0" : "METER_CLASS",
          "value1" : "METER_CLASS",
          "value2" : "METER_CLASS",
          "value3" : "METER_UNITS",
          "value4" : "Reserved",
          "value5" : "Reserved",
          "value6" : "Reserved",
          "value7" : "Reserved"
        },
        {
          "payload_4_Len": 3,
          "payload1Name": "Serial",
          "payloadType": "integer",
          "payloadDesc": "Serial number of gas meter. Unsigned integer",
          "value0" : "Serial"
        },
        {
          "payload_5_Len": 4,
          "payload1Name": "Volume",
          "payloadType": "integer",
          "payloadDesc": "Total volume, unit of measure: liter or ft3. Unsigned integer",
          "value0" : "volume"
        }
      ]
    },

    "0x41": {
      "message_Name": "MSG_UP_TIME",
      "decodedLen" : 6,
      "payload" : [
        {
          "payload1" :
            {
              "payload_1_Len": 4,
              "payload1Name": "CurrentTime",
              "payloadType": "Integer",
              "payloadDesc": "Current DeviceTime",
              "value0" : "currentDeviceTime"
            }
        },
        {
          "payload2" :
            {
              "payload_1_Len": 2,
              "payload1Name": "Debug",
              "payloadType": "Integer",
              "payloadDesc": "Debugging information for developers",
              "value0" : "Debug"
            }
        }
      ]
    }

```
