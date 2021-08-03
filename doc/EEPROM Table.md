# EEPROM Table

| Address | Name | Description |
| :-----: | :--- | :---------- |
| 00h | RF Tx Power | RF output power (Signed Byte)<br />EDh\~00h: -19\~0 dBm (F0h: -16 dBm(Default))<br />01h~1Bh: 1~27 dBm |
| 01h | RF Rx Sensitivity | CAh: -54 dBm<br />C7h: -57 dBm<br />C4h: -60 dBm<br />C1h: -63 dBm<br />BFh: -65 dBm<br />BCh: -68 dBm (Default)<br />B9h: -71 dBm<br />B6h: -74 dBm<br />B0h: -80 dBm<br />AFh: -81 dBm<br />ACh: -84 dBm<br />A9h: -87 dBm<br />A6h: -90 dBm |
| 02h | Rx Decode | 00h: FM0<br />01h: Miller 2<br />02h: Miller 4 (Default)<br />03h: Miller 8<br />Other: Miller 2 |
| 03h | Session And Target | Bit 0\~1 : Session<br />0: S0 (Default)<br />1: S1<br />2: S2<br />3: S3<br /><br />Bit 2: RFU (Must be 0)<br /><br />Bit 3\~4 :<br />Session :<br />0: Target A (Default)<br />1: Target B<br />2: Target A<->B |
| 04h | Link Frequency | 00h: 40kHz<br />03h: 80kHz<br />06h: 160kHz<br />08h: 213.3kHz<br />09h: 256kHz (Default)<br />0Ch: 320kHz<br />0Fh: 640kHz |
| 05h |  | RFU |
| 06h |  | RFU |
| 07h | Trext | 0: Don't use<br />1: Use long pilot tone (Default) |
| 08h | Q Begin | Bit 0\~3: Q Value<br />0\~15: Q=0\~15 (4: Default)<br /><br />Bit 4~6: RFU must be 0<br />Other: Set Q Value And Q Option to Default<br /><br />Bit 7: Q Option<br />0: Q Begin (Default)<br />1: Max Q |
| 09h |  | RFU |
| 0Ah | Buzzer | 00h: Disabled<br />Other: Enabled (Default) |
| 0Bh |  | RFU |
| 0Ch |  | RFU |
| 0Dh |                    | RFU                                                          |
| 0Eh | Tag Event Type | Bit 7: Tag Format Type<br />0: Text Event<br />1: Binary Event (GNetPlus Event Pakcage)<br /><br />When Binary Event (GNetPlus Event Pakcage):<br />Bit 0: Tag Event with TID option<br />0: Enabled<br />1: Disabled<br />Bit 1: Tag Event with Tag Data option<br />0: Enabled<br />1: Disabled<br />Bit 5: Remove Tag Event option<br />0: Enabled<br />1: Disabled<br />Bit 6: Tag Count Info Event option<br />0: Enabled<br />1: Disabled<br /><br />When Text Event:<br />Bit 0~6: Text format tag event<br />0: STX+PC+EPC+CRC+CR+LF+ETX<br />1: STX+PC+EPC+CR+LF+ETX<br />2: PC+EPC+CR<br />3: STX+[IR]+PC+EPC+CR+LF+ETX<br />4: Same as GNetPlus Event (PC+EPC Only)<br />5: EPC+CR<br />6: EPC+CR+LF<br />Note: Output PC, EPC, CRC using Hex string format |
| 0Fh |  | RFU |
| 10h | Baudrate | 04h: 2400bps<br />05h: 4800bps<br />06h: 9600bps<br />07h: 19200bps<br />08h: 38400bps<br />09h: 57600bps<br />FFh or Other :115200bps (Default) |
| 11h | Address | 01h\~FFh: Device Address<br />00h : Any Device (For Broadcast) |
| 12h | Reader Mode | Reader working mode<br />0: Command Mode<br />1: Auto Mode |
| 13h | Time To Remove | Remove tag when number of inventory round not found tag<br />00h: Remove tag immediately<br />01h\~FEh: Number of inventory round<br />FF: Same as 05h |
| 14h | Repeat Time | Repeat Tag Event<br />00h: Fast<br />01h\~FDh: Repeat time unit is 100 ms<br />FEh: Never repeat<br />FFh: Default is fast |
| 15h |                    | RFU |
| 16h | Output Interface | Tag Event Output Interface<br />02h: RS232 Interface<br />FFh: Default Interface |
| 17h~FFh |  | RFU |