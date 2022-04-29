# RF385-00 Command Set

## Index of Contents

1. [History](#1-history)
2. [Symbols](#2-symbols)
3. [GNetPlus Protocol](GNetPlus%20Protocol.md)
   1. [Basic Package](GNetPlus%20Protocol.md#basic-package)
   2. [CRC16 Calculation](GNetPlus%20Protocol.md#crc16-calculation)
   3. [GNetPlus Implement](GNetPlus%20Protocol.md#gnetplus-implement)
4. [Commands](#4-commands)
   1. [Get Version](#4-1-get-version-command)
   2. [Set Working Mode](#4-2-set-working-mode)
   3. [Read EEPROM](#4-3-read-eeprom)
   4. [Write EEPROM](#4-4-write-eeprom)
5. [Events](#5-events)
   1. [Tag Events Example](#5-1-tag-events-example)
6. [Error Code](#6-error-code)

## 1. History

### 2022/04/29

* Remove Tag Event Type 6 (EEPROM Table, Address 0Eh)
* Add Tag Event Example

### 2021/08/03

* Initial Version

## 2\. Symbols

Symbols to this command set are as follows

### Data Types

The RF385-00 commands digital data order uses **Big Endian**

| Type | Bytes | Value Range     | Description              |
|:----:|:-----:|:--------------- | ------------------------ |
| s8   | 1     | -128 \~ 127     | 8 bit signed integer     |
| s16  | 2     | -32768 \~ 32767 | 16 bit signed integer    |
| u8   | 1     | 0 \~ 255        | 8 bit unsigned integer   |
| u16  | 2     | 0 \~ 65535      | 16 bit  unsigned integer |

## 4\. Commands

RF385-00 commands is base on [GNetPlus Protocol](GNetPlus%20Protocol.md)

## *Request Code List*

| Name                                      | Code  | Description                     |
|:-----------------------------------------:|:-----:| ------------------------------- |
| [Get Version](#4-1-get-version-command)   | `10h` | Get Firmware / Hardware version |
| [Set Working Mode](#4-2-set-working-mode) | `12h` | Set Working Mode                |
| [Read EEPROM](#4-3-read-eeprom)           | 24h   | Read data from EEPROM           |
| [Write EEPROM](#4-4-write-eeprom)         | 22h   | Write data to EEPROM            |

## 4-1\. Get Version Command

Request Code is 10h

### Get Firmware/Hardware Version

* Parameter

| Offset | Bytes | Type | Name  | Description                                                 |
|:------:|:-----:|:----:| ----- | ----------------------------------------------------------- |
| 0      | 1     | u8   | Index | Version Index<br>0: Firmware Version<br>1: Hardware Version |

* Response
  * NAK: Response an [error code](#4\.-error-code)
  * ACK: Success with response data
    * Response Data

| Offset | Bytes | Type | Name    | Description                  |
|:------:|:-----:|:----:| ------- | ---------------------------- |
| 0      | N     | u8   | Version | Firmware or Hardware Version |

#### Example

For the NAK response of the example command, please refer to [GNetPlus's example](GNetPlus%20Protocol.md#response-a-nak-package)

* Get Firmware Version

<br />`[Send 7 Bytes] Get Version (10h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `00` | `10` | `01` | `00` | `71` | `00` |      |      |      |      |      |      |      |      |      | `.....q.`                                               |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` 00`                                                   | `00h: Device Address: Broadcast (Any Device)`                  |
| `02`     | `1`     | ` 10`                                                   | `10h: Code: Get Version`                                       |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` 00`                                                   | `0: Version Index: Firmware Version`                           |
| `05`     | `2`     | ` 71 00`                                                | `CRC16`                                                        |

<br />`[Receive 43 Bytes] Get Version Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `06` | `25` | `52` | `46` | `33` | `38` | `35` | `2D` | `30` | `30` | `20` | `52` | `4F` | `4D` | `...%RF385-00 ROM`                                      |
| `10`     | `2D` | `54` | `31` | `37` | `38` | `34` | `20` | `56` | `31` | `2E` | `30` | `30` | `52` | `33` | `20` | `28` | `-T1784 V1.00R3 (`                                      |
| `20`     | `32` | `30` | `30` | `33` | `31` | `31` | `30` | `29` | `00` | `27` | `C0` |      |      |      |      |      | `2003110).'.`                                           |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div>                                                                                                   | <div style='min-width:32em' align='center'>`Description`</div>  |
|:--------:|:-------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------- |:--------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                                                                                                                     | `SOH (Start of Heading)`                                        |
| `01`     | `1`     | ` FF`                                                                                                                                                     | `Device Address`                                                |
| `02`     | `1`     | ` 06`                                                                                                                                                     | `06h: Code: ACK`                                                |
| `03`     | `1`     | ` 25`                                                                                                                                                     | `Data Length`                                                   |
| `04`     | `37`    | ` 52 46 33 38 35 2D 30 30` <br /> ` 20 52 4F 4D 2D 54 31 37` <br /> ` 38 34 20 56 31 2E 30 30` <br /> ` 52 33 20 28 32 30 30 33` <br /> ` 31 31 30 29 00` | `Firmware Version:`<br />`RF385-00 ROM-T1784 V1.00R3 (2003110)` |
| `29`     | `2`     | ` 27 C0`                                                                                                                                                  | `CRC16`                                                         |

## 4-2\. Set Working Mode

Request Code is 12h

### Enter Auto / Command Mode

* Parameter

| Offset | Bytes | Type | Name | Description                                                    |
|:------:|:-----:|:----:| ---- | -------------------------------------------------------------- |
| 0      | 1     | u8   | Mode | New [Working Mode](#working-mode)<br />0: Auto<br />1: Command |

* Response
  * NAK: Response an [error code](#4\.-error-code). 
  * ACK: Success with response data
    * Response Data

| Offset | Bytes | Type | Name | Description                                                        |
|:------:|:-----:|:----:| ---- | ------------------------------------------------------------------ |
| 0      | 1     | u8   | Mode | Current [Working Mode](#working-mode)<br />0: Auto<br />1: Command |

#### Working Mode

| Value | Description                                                      |
| ----- | ---------------------------------------------------------------- |
| 0     | Auto Mode:<br />Automatic inventory tag and output tag data      |
| 1     | Command Mode:<br />Waiting for the command without inventory Tag |

#### Example

For the NAK response of the example command, please refer to [GNetPlus's example](GNetPlus%20Protocol.md#response-a-nak-package)

* Enter Auto Mode
  <br />`[Send 7 Bytes] Enter Auto Mode (10h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `00` | `10` | `01` | `00` | `71` | `00` |      |      |      |      |      |      |      |      |      | `.....q.`                                               |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` 00`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 10`                                                   | `00h: Code: Set Working Mode`                                  |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` 00`                                                   | `00h: [Working Mode](#working-mode): Auto Mode`                |
| `05`     | `2`     | ` 71 00`                                                | `CRC16`                                                        |

<br />`[Receive 7 Bytes] Enter Auto Mode Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `06` | `01` | `00` | `A1` | `D1` |      |      |      |      |      |      |      |      |      | `.......`                                               |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` FF`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 06`                                                   | `06h: Code: ACK`                                               |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` 00`                                                   | `00h: [Working Mode](#working-mode): Auto Mode`                |
| `05`     | `2`     | ` A1 D1`                                                | `CRC16`                                                        |

* Enter Command Mode
  <br />`[Send 7 Bytes] Enter Command Mode (12h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `00` | `12` | `01` | `01` | `71` | `60` |      |      |      |      |      |      |      |      |      | <code>.....q\`</code>                                   |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` 00`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 12`                                                   | `00h: Code: Change Working Mode`                               |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` 01`                                                   | `01h: Working Mode: Command Mode`                              |
| `05`     | `2`     | ` 71 60`                                                | `CRC16`                                                        |

<br />`[Receive 7 Bytes] Enter Command Mode Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `06` | `01` | `00` | `A1` | `D1` |      |      |      |      |      |      |      |      |      | `.......`                                               |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` FF`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 06`                                                   | `06h: Code: ACK`                                               |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` 00`                                                   | `01h: Working Mode: Command Mode`                              |
| `05`     | `2`     | ` A1 D1`                                                | `CRC16`                                                        |

## 4-3\. Read EEPROM

Request Code is 24h

### Read Device Settings from EEPROM  (Registers)

* Parameter

| Offset | Bytes | Type | Name    | Description                                                                                                                                             |
|:------:|:-----:|:----:| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0      | 2     | u16  | Address | Read [EEPROM Address](EEPROM%20Table.md#eeprom-table) (Big Endian)<br />Bit 0~15: Address<br /><br />Bit 15: Memory Option<br />0: EEPROM<br />1: Register |
| 1      | 1     | u8   | Length  | Number of bytes read from EEPROM                                                                                                                        |

* Response
  * NAK: Response an [error code](#4\.-error-code). 
  * ACK: Success with response data
    * Response Data

| Offset | Bytes | Type | Name | Description |
|:------:|:-----:|:----:| ---- | ----------- |
| 0      | N     | u8   | Data | EEPROM Data |

#### Example

For the NAK response of the example command, please refer to [GNetPlus's example](GNetPlus%20Protocol.md#response-a-nak-package)

* Read RF Power from EEPROM
  `[Send 9 Bytes] Read RF Power from EEPROM (24h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `00` | `24` | `03` | `00` | `00` | `01` | `98` | `B1` |      |      |      |      |      |      |      | `..$......`                                             |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` 00`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 24`                                                   | `24h: Code: Read EEPROM Command`                               |
| `03`     | `1`     | ` 03`                                                   | `Data Length`                                                  |
| `04`     | `2`     | ` 00 00`                                                | `0000h: EEPROM Address: RF Power`                              |
| `06`     | `1`     | ` 01`                                                   | `01h: Read EEPROM Length`                                      |
| `07`     | `2`     | ` 98 B1`                                                | `CRC16`                                                        |

<br />`[Receive 7 Bytes] Read RF Power from EEPROM Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `06` | `01` | `0A` | `A6` | `51` |      |      |      |      |      |      |      |      |      | `......Q`                                               |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` FF`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 06`                                                   | `06h: Code: ACK`                                               |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` 0A`                                                   | `0Ah: Data: RF Power 10 dBm`                                   |
| `05`     | `2`     | ` A6 51`                                                | `CRC16`                                                        |

* Read Baudrate from EEPROM
  <br />`[Send 9 Bytes] Read Baudrate from EEPROM (24h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `00` | `24` | `03` | `00` | `10` | `01` | `58` | `BC` |      |      |      |      |      |      |      | `..$....X.`                                             |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` 00`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 24`                                                   | `24h: Code: Read EEPROM Command`                               |
| `03`     | `1`     | ` 03`                                                   | `Data Length`                                                  |
| `04`     | `2`     | ` 00 10`                                                | `0010h: EEPROM Address: Baudrate`                              |
| `06`     | `1`     | ` 01`                                                   | `01h: Read EEPROM Length`                                      |
| `07`     | `2`     | ` 58 BC`                                                | `CRC16`                                                        |

<br />`[Receive 7 Bytes] Read Baudrate from EEPROM Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `06` | `01` | `FF` | `E1` | `91` |      |      |      |      |      |      |      |      |      | `.......`                                               |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` FF`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 06`                                                   | `06h: Code: ACK`                                               |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` FF`                                                   | `FFh: Data: Baudrate 115200 bps`                               |
| `05`     | `2`     | ` E1 91`                                                | `CRC16`                                                        |

## 4-4\. Write EEPROM

Request Code is 22h

### Write Device Settings to EEPROM (Registers)

* Parameter

| Offset | Bytes | Type | Name    | Description                                                                                                                                                                                                                                                                                                                  |
|:------:|:-----:|:----:| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0      | 2     | u16  | Address | Write [EEPROM Address](EEPROM%20Table.md#eeprom-table) (Big Endian)<br />Bit 0~14: EEPROM/Register Address please refer to [EEPROM Table](EEPROM%20Table.md#eeprom-table) <br /><br />Bit 15: Memory Option<br />0: EEPROM<br />1: Registers (Directly update the set value to Registers, and will force to enter the command mode) |
| 1      | N     | u8   | Data    | Write data.                                                                                                                                                                                                                                                                                                                  |

* Response
  * NAK: Response an [error code](#4\.-error-code).
  * ACK: Success with response data
    * Response Data

| Offset | Bytes | Type | Name | Description |
|:------:|:-----:|:----:| ---- | ----------- |
| 0      | N     | u8   | Data | EEPROM Data |

#### Example

For the NAK response of the example command, please refer to [GNetPlus's example](GNetPlus%20Protocol.md#response-a-nak-package)

* Write RF Power 10 dBm to EEPROM
  <br />`[Send 9 Bytes] Write RF Power 10 dBm to EEPROM (22h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `00` | `22` | `03` | `00` | `00` | `0A` | `5F` | `78` |      |      |      |      |      |      |      | `.."...._x`                                             |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` 00`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 22`                                                   | `24h: Code: Read EEPROM Command`                               |
| `03`     | `1`     | ` 03`                                                   | `Data Length`                                                  |
| `04`     | `2`     | ` 00 00`                                                | `0000h: EEPROM Address: RF Power`                              |
| `06`     | `1`     | ` 0A`                                                   | `0Ah: Data: RF Power 10 dBm`                                   |
| `07`     | `2`     | ` 5F 78`                                                | `CRC16`                                                        |

<br />`[Receive 7 Bytes] Write RF Power 10 dBm to EEPROM Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `06` | `01` | `0A` | `A6` | `51` |      |      |      |      |      |      |      |      |      | `......Q`                                               |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` FF`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 06`                                                   | `06h: Code: ACK`                                               |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` 0A`                                                   | `0Ah: Data: RF Power 10 dBm`                                   |
| `05`     | `2`     | ` A6 51`                                                | `CRC16`                                                        |

* Write Baudrate 115200 bps to EEPROM
  <br />`[Send 9 Bytes] Write Baudrate 115200 bps to EEPROM (22h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `00` | `22` | `03` | `00` | `10` | `FF` | `D8` | `B5` |      |      |      |      |      |      |      | `.."......`                                             |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` 00`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 22`                                                   | `24h: Code: Read EEPROM Command`                               |
| `03`     | `1`     | ` 03`                                                   | `Data Length`                                                  |
| `04`     | `2`     | ` 00 10`                                                | `0010h: EEPROM Address: Baudrate`                              |
| `06`     | `1`     | ` FF`                                                   | `FFh: Data: Baudrate 115200 bps`                               |
| `07`     | `2`     | ` D8 B5`                                                | `CRC16`                                                        |

<br />`[Receive 7 Bytes] Write Baudrate 115200 bps to EEPROM Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `06` | `01` | `FF` | `E1` | `91` |      |      |      |      |      |      |      |      |      | `.......`                                               |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` FF`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 06`                                                   | `06h: Code: ACK`                                               |
| `03`     | `1`     | ` 01`                                                   | `Data Length`                                                  |
| `04`     | `1`     | ` FF`                                                   | `FFh: Data: Baudrate 115200 bps`                               |
| `05`     | `2`     | ` E1 91`                                                | `CRC16`                                                        |

### Update EEPROM to Device Registers

Update EEPROM settings to device registers and EEPROM settings will take effect

* Parameter

| Offset | Bytes | Type | Name          | Description                                                                                                                      |
|:------:|:-----:|:----:| ------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 0      | 2     | u16  | Address       | Special update Address (Big Endian)<br />Bit 0~15: Address<br />FFFFh: Update EEPROM to Device Registers                         |
| 1      | 1     | u8   | Update Option | Update EEPROM to Reigsetrs option<br />0: Update All EEPROM to Registers<br />1: Update EEPROM without working Mode to Registers |

* Response
  * NAK: Response an [error code](#4\.-error-code).
  * ACK: Success

#### Example

For the response of the example command, please refer to [GNetPlus's example](GNetPlus%20Protocol.md#response-an-ack-package)

* Update EEPROM to Device Registers
  <br />`[Send 9 Bytes] Update EEPROM To Registers (22h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `00` | `22` | `03` | `FF` | `FF` | `01` | `58` | `48` |      |      |      |      |      |      |      | `.."....XH`                                             |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div>        |
|:--------:|:-------:|:------------------------------------------------------- |:--------------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                              |
| `01`     | `1`     | ` 00`                                                   | `Device Address`                                                      |
| `02`     | `1`     | ` 22`                                                   | `22h: Code: Command`                                                  |
| `03`     | `1`     | ` 03`                                                   | `Data Length`                                                         |
| `04`     | `2`     | ` FF FF`                                                | `FFFFh: EEPROM Address: Update to Registers`                          |
| `06`     | `1`     | ` 01`                                                   | `01h: Update Option: Update EEPROM without working Mode to Registers` |
| `07`     | `2`     | ` 58 48`                                                | `CRC16`                                                               |

<br />`[Receive 6 Bytes] Update EEPROM To Registers Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `06` | `00` | `50` | `42` |      |      |      |      |      |      |      |      |      |      | `....PB`                                                |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `SOH (Start of Heading)`                                       |
| `01`     | `1`     | ` FF`                                                   | `Device Address`                                               |
| `02`     | `1`     | ` 06`                                                   | `06h: Code: ACK`                                               |
| `03`     | `1`     | ` 00`                                                   | `Data Length`                                                  |
| `04`     | `2`     | ` 50 42`                                                | `CRC16`                                                        |

## 5\. Events

An event means a package that the device actively responds to to the host

### 5-1\. Tag Events Example

For Tag Event Type setting, please refer to [EEPROM Table](EEPROM%20Table.md#eeprom-table) and [Write EEPROM](#4-4-write-eeprom) Command

#### Tag Event Type 0 Example

<br />`[Receive 36 Bytes] Tag Event Type 0`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `02` | `33` | `30` | `30` | `30` | `45` | `32` | `38` | `30` | `36` | `38` | `39` | `34` | `32` | `30` | `30` | `.3000E2806894200`                                      |
| `10`     | `30` | `35` | `30` | `30` | `30` | `37` | `45` | `41` | `30` | `36` | `34` | `44` | `30` | `38` | `46` | `42` | `050007EA064D08FB`                                      |
| `20`     | `30` | `0D` | `0A` | `03` |      |      |      |      |      |      |      |      |      |      |      |      | `0...`                                                  |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div>                                        | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:---------------------------------------------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 02`                                                                                          | `02h: STX (Start of Text)`                                     |
| `01`     | `4`     | ` 33 30 30 30`                                                                                 | `PC: "3000"`                                                   |
| `05`     | `24`    | ` 45 32 38 30 36 38 39 34` <br /> ` 32 30 30 30 35 30 30 30` <br /> ` 37 45 41 30 36 34 44 30` | `EPC: "E2806894200050007EA064D0"`                              |
| `1D`     | `4`     | ` 38 46 42 30`                                                                                 | `EPC CRC16: "8FB0"`                                            |
| `21`     | `1`     | ` 0D`                                                                                          | `0Dh: CR (Carriage Return)`                                    |
| `22`     | `1`     | ` 0A`                                                                                          | `0Ah: LF (Line Feed)`                                          |
| `23`     | `1`     | ` 03`                                                                                          | `03h: ETX (End of Text)`                                       |

#### Tag Event Type 1 Example

<br />`[Receive 32 Bytes] Tag Event Type 1`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `02` | `33` | `30` | `30` | `30` | `45` | `32` | `38` | `30` | `36` | `38` | `39` | `34` | `32` | `30` | `30` | `.3000E2806894200`                                      |
| `10`     | `30` | `35` | `30` | `30` | `30` | `37` | `45` | `41` | `30` | `36` | `34` | `44` | `30` | `0D` | `0A` | `03` | `050007EA064D0...`                                      |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div>                                        | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:---------------------------------------------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 02`                                                                                          | `02h: STX (Start of Text)`                                     |
| `01`     | `4`     | ` 33 30 30 30`                                                                                 | `PC: "3000E2806894200050007EA064D0"`                           |
| `05`     | `24`    | ` 45 32 38 30 36 38 39 34` <br /> ` 32 30 30 30 35 30 30 30` <br /> ` 37 45 41 30 36 34 44 30` | `EPC: "3000E2806894200050007EA064D0"`                          |
| `1D`     | `1`     | ` 0D`                                                                                          | `0Dh: CR (Carriage Return)`                                    |
| `1E`     | `1`     | ` 0A`                                                                                          | `0Ah: LF (Line Feed)`                                          |
| `1F`     | `1`     | ` 03`                                                                                          | `03h: ETX (End of Text)`                                       |

#### Tag Event Type 2 Example

<br />`[Receive 29 Bytes] Tag Event Type 2`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `33` | `30` | `30` | `30` | `45` | `32` | `38` | `30` | `36` | `38` | `39` | `34` | `32` | `30` | `30` | `30` | `3000E28068942000`                                      |
| `10`     | `35` | `30` | `30` | `30` | `37` | `45` | `41` | `30` | `36` | `34` | `44` | `30` | `0D` |      |      |      | `50007EA064D0.`                                         |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div>                                        | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:---------------------------------------------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `4`     | ` 33 30 30 30`                                                                                 | `PC: "3000"`                                                   |
| `04`     | `24`    | ` 45 32 38 30 36 38 39 34` <br /> ` 32 30 30 30 35 30 30 30` <br /> ` 37 45 41 30 36 34 44 30` | `EPC: "E2806894200050007EA064D0"`                              |
| `1C`     | `1`     | ` 0D`                                                                                          | `0Dh: CR (Carriage Return)`                                    |

#### Tag Event Type 3 (New Tag) Example

<br />`[Receive 33 Bytes] Tag Event Type 3 (New Tag)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `02` | `49` | `33` | `30` | `30` | `30` | `45` | `32` | `38` | `30` | `36` | `38` | `39` | `34` | `32` | `30` | `.I3000E280689420`                                      |
| `10`     | `30` | `30` | `35` | `30` | `30` | `30` | `37` | `45` | `41` | `30` | `36` | `34` | `44` | `30` | `0D` | `0A` | `0050007EA064D0..`                                      |
| `20`     | `03` |      |      |      |      |      |      |      |      |      |      |      |      |      |      |      | `.`                                                     |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div>                                        | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:---------------------------------------------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 02`                                                                                          | `02h: STX (Start of Text)`                                     |
| `01`     | `1`     | ` 49`                                                                                          | `49h('I'): New Tag`                                            |
| `02`     | `4`     | ` 33 30 30 30`                                                                                 | `PC: "3000"`                                                   |
| `06`     | `24`    | ` 45 32 38 30 36 38 39 34` <br /> ` 32 30 30 30 35 30 30 30` <br /> ` 37 45 41 30 36 34 44 30` | `EPC: "E2806894200050007EA064D0"`                              |
| `1E`     | `1`     | ` 0D`                                                                                          | `0Dh: CR (Carriage Return)`                                    |
| `1F`     | `1`     | ` 0A`                                                                                          | `0Ah: LF (Line Feed)`                                          |
| `20`     | `1`     | ` 03`                                                                                          | `03h: ETX (End of Text)`                                       |

#### Tag Event Type 3 (Removed Tag) Example

<br />`[Receive 32 Bytes] Tag Event Type 3 (Removed Tag)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `02` | `52` | `33` | `30` | `30` | `30` | `45` | `32` | `38` | `30` | `36` | `38` | `39` | `34` | `32` | `30` | `.R3000E280689420`                                      |
| `10`     | `30` | `30` | `35` | `30` | `30` | `30` | `37` | `45` | `41` | `30` | `36` | `34` | `44` | `30` | `0D` | `0A` | `0050007EA064D0..`                                      |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div>                                        | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:---------------------------------------------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 02`                                                                                          | `02h: STX (Start of Text)`                                     |
| `01`     | `1`     | ` 52`                                                                                          | `52h('R'): Removed Tag`                                        |
| `02`     | `4`     | ` 33 30 30 30`                                                                                 | `PC: "3000"`                                                   |
| `06`     | `24`    | ` 45 32 38 30 36 38 39 34` <br /> ` 32 30 30 30 35 30 30 30` <br /> ` 37 45 41 30 36 34 44 30` | `EPC: "E2806894200050007EA064D0"`                              |
| `1E`     | `1`     | ` 0D`                                                                                          | `0Dh: CR (Carriage Return)`                                    |
| `1F`     | `1`     | ` 0A`                                                                                          | `0Ah: LF (Line Feed)`                                          |

#### Tag Event Type 4 (New Tag) Example

<br />`[Receive 28 Bytes] Tag Event Type 4 (New Tag) (11h: Code: Event (ID: 49h))`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `11` | `16` | `49` | `00` | `B7` | `0D` | `DF` | `C2` | `0E` | `30` | `00` | `E2` | `80` | `68` | `....I......0...h`                                      |
| `10`     | `94` | `20` | `00` | `50` | `00` | `7E` | `A0` | `64` | `D0` | `00` | `68` | `23` |      |      |      |      | `. .P.~.d..h#`                                          |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `01h: SOH (Start of Heading)`                                  |
| `01`     | `1`     | ` FF`                                                   | `FFh: Device Address`                                          |
| `02`     | `1`     | ` 11`                                                   | `11h: Code: Event (ID: 49h)`                                   |
| `03`     | `1`     | ` 16`                                                   | `16h: Data Length`                                             |
| `04`     | `1`     | ` 49`                                                   | `49h: Event: New Tag Event`                                    |
| `05`     | `1`     | ` 00`                                                   | `00h: Find the tag's number in one round`                      |
| `06`     | `1`     | ` B7`                                                   | `B7h: -73 dBm (RSSI: Tag Signal Strength)`                     |
| `07`     | `3`     | ` 0D DF C2`                                             | `0DDFC2h: 909250 KHz, Find tag frequency`                      |
| `0A`     | `1`     | ` 0E`                                                   | `0Eh: PC+EPC Length`                                           |
| `0B`     | `2`     | ` 30 00`                                                | `PC: 3000`                                                     |
| `0D`     | `12`    | ` E2 80 68 94 20 00 50 00` <br /> ` 7E A0 64 D0`        | `EPC: E2806894200050007EA064D0`                                |
| `19`     | `1`     | ` 00`                                                   | `00h: Antenna 0, Find the tag on the number of the antenna`    |
| `1A`     | `2`     | ` 68 23`                                                | `CRC16`                                                        |

#### Tag Event Type 4 (Removed Tag) Example

<br />`[Receive 28 Bytes] Tag Event Type 4 (Removed Tag) (11h: Code: Event (ID: 52h))`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `01` | `FF` | `11` | `16` | `52` | `00` | `C2` | `0D` | `D9` | `E6` | `0E` | `30` | `00` | `E2` | `80` | `68` | `....R......0...h`                                      |
| `10`     | `94` | `20` | `00` | `50` | `00` | `7E` | `A0` | `64` | `D0` | `00` | `3A` | `F4` |      |      |      |      | `. .P.~.d..:.`                                          |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `1`     | ` 01`                                                   | `01h: SOH (Start of Heading)`                                  |
| `01`     | `1`     | ` FF`                                                   | `FFh: Device Address`                                          |
| `02`     | `1`     | ` 11`                                                   | `11h: Code: Event (ID: 49h)`                                   |
| `03`     | `1`     | ` 16`                                                   | `16h: Data Length`                                             |
| `04`     | `1`     | ` 52`                                                   | `52h: Event: New Tag Event`                                    |
| `05`     | `1`     | ` 00`                                                   | `00h: Find the tag's number in one round`                      |
| `06`     | `1`     | ` C2`                                                   | `C2h: -62 dBm (RSSI: Find Tag Signal Strength)`                |
| `07`     | `3`     | ` 0D D9 E6`                                             | `0DD9E6h: 907750 KHz, Find tag frequency`                      |
| `0A`     | `1`     | ` 0E`                                                   | `0Eh: PC+EPC Length`                                           |
| `0B`     | `2`     | ` 30 00`                                                | `PC: 3000`                                                     |
| `0D`     | `12`    | ` E2 80 68 94 20 00 50 00` <br /> ` 7E A0 64 D0`        | `EPC: E2806894200050007EA064D0`                                |
| `19`     | `1`     | ` 00`                                                   | `00h: Antenna 0, Find the tag on the number of the antenna`    |
| `1A`     | `2`     | ` 3A F4`                                                | `CRC16`                                                        |

#### Tag Event Type 5 Example

<br />`[Receive 25 Bytes] Tag Event Type 5`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
|:--------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:------------------------------------------------------- |
| `00`     | `45` | `32` | `38` | `30` | `36` | `38` | `39` | `34` | `32` | `30` | `30` | `30` | `35` | `30` | `30` | `30` | `E280689420005000`                                      |
| `10`     | `37` | `45` | `41` | `30` | `36` | `34` | `44` | `30` | `0D` |      |      |      |      |      |      |      | `7EA064D0.`                                             |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div>                                        | <div style='min-width:32em' align='center'>`Description`</div> |
|:--------:|:-------:|:---------------------------------------------------------------------------------------------- |:-------------------------------------------------------------- |
| `00`     | `24`    | ` 45 32 38 30 36 38 39 34` <br /> ` 32 30 30 30 35 30 30 30` <br /> ` 37 45 41 30 36 34 44 30` | `EPC: "E2806894200050007EA064D0"`                              |
| `18`     | `1`     | ` 0D`                                                                                          | `0Dh: CR (Carriage Return)`                                    |

## 6\. Error Code

| Value | Name          | Description                                                           |
|:-----:| ------------- | --------------------------------------------------------------------- |
| 00h   | ERR NONE      | No error occured                                                      |
| FFh   | ERR UNKNOWN   | Unknown error occured                                                 |
| FEh   | ERR NOMEM     | Not enough memory to perform the requested operation                  |
| FDh   | ERR BUSY      | Device or resource busy                                               |
| FCh   | ERR IO        | Generic IO error                                                      |
| FBh   | ERR TIMEOUT   | Error due to timeout                                                  |
| FAh   | ERR REQUEST   | Invalid request or requested function can't be executed at the moment |
| F9h   | ERR NOMSG     | No message of desired type                                            |
| F8h   | ERR PARAM     | Parameter error                                                       |
| F7h   | ERR PROTO     | Protocol error                                                        |
| F6h   | ERR PROTO CRC | Protocol CRC16 check error                                            |
