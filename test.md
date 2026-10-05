---
title: test
---
| **Address** | BRAM addr (VHDL side) | **Description** |
|-------------|-----------------------|-----------------|
| 0x0080 | 0 | A0x |
| 0x0081 | 1 | A1x |
| 0x0082 | 2 | A2x |
| 0x0083 | 3 | NOTCH1_GAIN_X |
| 0x0084 | 4 | NOTCH1_DELAY_X (7-bit unsigned, 100 µs per LSB) |
| 0x0085 | 5 | A0z |
| 0x0086 | 6 | A1z |
| 0x0087 | 7 | A2z |
| 0x0088 | 8 | NOTCH1_GAIN_Z |
| 0x0089 | 9 | NOTCH1_DELAY_Z (7-bit unsigned, 100 µs per LSB) |
| 0x008A | 10 | [BPM_ISUM_X](#FOFBregisters-ISUM) |
| 0x008B | 11 | [BPM_ISUM_Z](#FOFBregisters-ISUM) |
| 0x008C | 12 | [ISUM_CNT](#FOFBregisters-ISUM) |
| 0x008D | 13 | [ISUM_MODE](#FOFBregisters-ISUM) |
| 0x008E | 14 | bpm_start_ind (default: 1). _added in v5.2.3_ |
| 0x008F | 15 | bpm_stop_ind (default: 192). _added in v5.2.3_ |
| 0x0090 - 0x0092 | 16 - 18 | Unused |
| 0x0093 | 19 | [WAVEFORMS_ENA](#FOFBregisters-waveforms) |
| 0x0094 – 0x009D | 20 - 29 | [Trigger Registers](#FOFBregisters-triggers) |
| 0x009E | 30 | RF_FREQ_CNT (_added in v5.4.8_)_<br>_4-bit counter, increment each time RF_FREQ is updated__ |
| 0x009F – 0x00AE | 31 - 46 | Unused |
| 0x00AF | 47 | [BLANKING_DURATION](#FOFBregisters-blanking) |
| 0x00B0 - 0x00B9 | 48 - 57 | Simple Sniffer (not documented) |
| 0x00BA | 58 | Unused |
| 0x00BB | 59 | [LIMITS_DISABLE](#FOFBregisters-LIMITS_DISABLE) |
| 0x00BC | 60 | [LIMITS_ENABLE](#FOFBregisters-LIMITS_ENABLE) |
| 0x00BD | 61 | BPM deviation limit value for X plane (16 bit integer expressed in µm)<br>If zero, set to default value: 2 mm. |
| 0x00BE | 62 | BPM deviation limit value for Y plane (16 bit integer expressed in µm)<br>If zero, set to default value: 500 µm. |
| 0x00BF | 63 | Steerer absolute limit value for X plane in mA (16 bit signed integer, max value is 205)<br>If set to 205, limit on steerer current disabled |
| 0x00C0 | 64 | Steerer absolute limit value for Y plane in mA (16 bit signed integer, max value is 205)<br>If set to 205, limit on steerer current disabled |
| 0x00C1 | 65 | SINGEN_AMPL_X<br>16-bit signed (-1 → 1) |
| 0x00C2 | 66 | SINGEN_FREQ_X<br>16-bit signed (65536 = FA_freq = 10.149 kHz) |
| 0x00C3 | 67 | SINGEN_CORR_N_X<br>select X steerers to output sin wave (bit 0 ↔ steerer #0, etc.) |
| 0x00C4 | 68 | SINGEN_AMPL_Y<br>16-bit signed (-1 → 1) |
| 0x00C5 | 69 | SINGEN_FREQ_Y<br>16-bit signed (65536 = FA_freq = 10.149 kHz) |
| 0x00C6 | 70 | SINGEN_CORR_N_Y<br>select Y steerers to output sin wave (bit 0 ↔ steerer #0, etc.) |
| 0x00C7 – 0x01FF | 71 - 127 | Unused |
| 0x0200 - 0x020D |  | WF_GAIN_H1: X corrector gains for waveform1 (16 bit signed, 0x7FFF -\> 200mA) |
| 0x020E - 0x021B |  | WF_GAIN_V1: Y corrector gains for waveform1 (16 bit signed, 0x7FFF -\> 200mA) |
| 0x021C - 0x0229 |  | WF_GAIN_H2: X corrector gains for waveform2 (16 bit signed, 0x7FFF -\> 200mA) |
| 0x022A - 0x0247 |  | WF_GAIN_V2: Y corrector gains for waveform2 (16 bit signed, 0x7FFF -\> 200mA) |
| 0x0248 - 0x03FF |  | Unused |

