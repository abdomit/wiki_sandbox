[[_TOC_]]

This documentation is for fofb version 5 and above.

# Paper documentation

The original documentation for the FOFB is a .doc document, it is referred here a "paper documentation". This documentation correspond to the FOFB before the firmware version v3.0.0.

Since v3.0.0, the documentation is now on Confluence.

[FOFB_doc_A08.doc](#)

# List of all registers

| **ADDRESS (in 32-bits words)** | **REGISTER** | **R/W mode** | **DESCRIPTION** |
|--------------------------------|--------------|--------------|-----------------|
| 0x0000 | VERSION | R | 0x00VVSSRR with: VV=version, SS=subversion, RR=revision |
| 0x0001 | CC_CTRL | R/W | [CC control register](https://confluence.esrf.fr/display/DIAGWK/FOFB+CC+registers) |
| 0x0002 | CORR_CTRL | R/W | [Corrector control register](https://confluence.esrf.fr/display/DIAGWK/FOFB+CC+registers) |
| 0x0003 - 0x0005 |  |  | not used |
| 0x0006 – 0x0007 | Scratch pads | R | 16 bit scratchpad registers (debugging info from Simulink model) |
| 0x0008 – 0x0009 | Trigger counters | R | 16 bit counters for, resp., "hardware" trigger and FA network one<br>(FA_trig_cnt _removed in v5.4.0_) |
| 0x000A | RF_FREQ | R | 16 bit RF frequency register |
| 0x000B | FAULT_STATUS | R | [Fault status register](#report-on-fault) |
| 0x000C | FAULT_DEVICE_X | R | [X faulty device register](#report-on-fault) |
| 0x000D | FAULT_DEVICE_Y | R | [Y faulty device register](#report-on-fault) |
| 0x000E - 0x0016 | SYSMON\_\* | R | [System Monitor registers](#system-monitor-registers-0x000e--0x0016) |
| 0x000E – 0x007F |  |  | not used |
| 0x0080 – 0x03FF | RAM_CORR | R/W | [Address space for FOFB parameters and sine excitation](#ram_corrbram-0x0080--0x00ff) |
| 0x0400 – 0x05FF | WF_H1 | R/W | X damping waveform for Septa kicks (16 bit signed, normalized: full scale -1/+1) |
| 0x0600 – 0x07FF | WF_V1 | R/W | Y damping waveform for Septa kicks (16 bit signed, normalized: full scale  -1/+1) |
| 0x0800 – 0x09FF | WF_H2 | R/W | 2nd set of X damping waveform for Septa kicks |
| 0x0A00 – 0x0BFF | WF_V2 | R/W | 2nd set of Y damping waveform for Septa kicks |
| 0x0C00 – 0x0DFF | BPM_DISABLE_X | R/W | Disable Fault on X positon limit reached for individual BPMs |
| 0x0E00 – 0x0FFF | BPM_DISABLE_Y | R/W | Disable Fault on Y positon limit reached for individual BPMs |
| 0x1000 - 0x11FF | OFFSET_X | R/W | X offsets of positions (16 bits signed, resolution 16nm, full scale \~+-500um) |
| 0x1200 - 0x13FF | OFFSET_Y | R/W | Y offsets of positions (16 bits signed, resolution 16 nm, full scale \~+-500um) |
| 0x1400 - x1FFF |  |  | not used |
| 0x2000 – 0x3BFF | RAM_COEF_X\_\* | R/W | [Address space for X matrix coefficients](#FOFBregisters-ORBIT_CORR) |
| 0x3C00 – 0x3FFF |  |  | not used |
| 0x4000 – 0x5BFF | RAM_COEF_Y\_\* | R/W | [Address space for Y matrix coefficients](#FOFBregisters-ORBIT_CORR) |
| 0x5C00 – 0x5FFF |  |  | not used |
| 0x6000 – 0x61FF | CC_POS_X | R/W | [Address space for X positions read from CC](https://confluence.esrf.fr/display/DIAGWK/FOFB+CC+registers) |
| 0x6200 – 0x63FF | CC_POS_Y | R/W | [Address space for Y positions read from CC](https://confluence.esrf.fr/display/DIAGWK/FOFB+CC+registers) |
| 0x6400 – 0x64FF | CC_CONFIG | R/W | [CC configuration registers](https://confluence.esrf.fr/display/DIAGWK/FOFB+CC+registers) (see CC documentation) |
| 0x6500 – 0x65FF | CC_STATUS | R/W | [CC status registers](https://confluence.esrf.fr/display/DIAGWK/FOFB+CC+registers) (see CC documentation) |
| 0x6600 – 0x7FFF |  |  | not used |

## System Monitor Registers (0x000E → 0x0016)

The SYSMON\_\* 16-bit unsigned registers give the measured system parameters. Two types of parameters are measured: temperature and voltages.

For temperature the conversion is the following:

```math
\text{temp} [^\circ C] = \dfrac{503.975 \times x}{2^{16}}-273.15
```

For voltage the conversion is the following:

```math
\text{volt} [\text{V}] = 3 \times \dfrac{x}{2^{16}}
```

| address | register name | description |
|---------|---------------|-------------|
| 0x000E | SYSMON_TEMP | FPGA chip temperature |
| 0x000F | SYSMON_TEMP_MAX | FPGA chip maximum measured temperature since last FPGA reconfiguration |
| 0x0010 | SYSMON_TEMP_MIN | FPGA chip minimum measured temperature since last FPGA reconfiguration |
| 0x0011 | SYSMON_VCCINT | V<sub>ccint</sub> measured voltage.<br>The expected measured voltage is 1 V. |
| 0x0012 | SYSMON_VCCINT_MAX | V<sub>ccint</sub> maximum measured voltage since last FPGA reconfiguration |
| 0x0013 | SYSMON_VCCINT_MIN | V<sub>ccint</sub> minimum measured voltage since last FPGA reconfiguration |
| 0x0014 | SYSMON_VCCAUX | V<sub>ccaux</sub> measured voltage.<br>The expected measured voltage is 2.5 V. |
| 0x0015 | SYSMON_VCCAUX_MAX | V<sub>ccaux</sub> maximum measured voltage since last FPGA reconfiguration |
| 0x0016 | SYSMON_VCCAUX_MIN | V<sub>ccaux</sub> minimum measured voltage since last FPGA reconfiguration |

## RAM_CORR BRAM (0x0080 → 0x00FF)

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
| 0x009E | 30 | RF_FREQ_CNT (_added in v5.4.8_)\_<br>_4-bit counter, increment each time RF_FREQ is updated_\_ |
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
| 0x00C3 | 67 | SINGEN_CORR_N_X<br>select X steerers to output sin wave (bit 0 :left_right_arrow: steerer #0, etc.) |
| 0x00C4 | 68 | SINGEN_AMPL_Y<br>16-bit signed (-1 → 1) |
| 0x00C5 | 69 | SINGEN_FREQ_Y<br>16-bit signed (65536 = FA_freq = 10.149 kHz) |
| 0x00C6 | 70 | SINGEN_CORR_N_Y<br>select Y steerers to output sin wave (bit 0 :left_right_arrow: steerer #0, etc.) |
| 0x00C7 – 0x01FF | 71 - 127 | Unused |
| 0x0200 - 0x020D |  | WF_GAIN_H1: X corrector gains for waveform1 (16 bit signed, 0x7FFF -\> 200mA) |
| 0x020E - 0x021B |  | WF_GAIN_V1: Y corrector gains for waveform1 (16 bit signed, 0x7FFF -\> 200mA) |
| 0x021C - 0x0229 |  | WF_GAIN_H2: X corrector gains for waveform2 (16 bit signed, 0x7FFF -\> 200mA) |
| 0x022A - 0x0247 |  | WF_GAIN_V2: Y corrector gains for waveform2 (16 bit signed, 0x7FFF -\> 200mA) |
| 0x0248 - 0x03FF |  | Unused |

# Functional descriptions

## Orbit Correction

| **Address** | **Description** |
|-------------|-----------------|
| 0x2000 | Coeff. for Corrector 1 multiplied by X data at address 0 |
| … | … |
| 0x21FF | Coeff. for Corrector 1 multiplied by X data at address 511 |
| 0x2200 | Coeff. for Corrector 2 multiplied by X data at address 0 |
| … | … |
| 0x23FF | Coeff. for Corrector 2 multiplied by X data at address 511 |
| … | … |
| 0x3A00 – 0x3BFF | Coeff. for Corrector 14 |

See paper documentation

## Total steerer current correction

The total current send to steerers has to be 0 to avoid a change in the beam energy. This is done by two means: first the correction matrix is computed such that steerers currents cancels out, but this condition may not be completely fulfilled and approximation due to numerical computation may introduce non-zero total current which adds up the previous error and increase with time. To compensate for this slow drift, a virtual BPM is added to the correction and is used to bring the total current back to 0.

To compensate for a finite total current, the procedure is the following:

* Compute the BPM_ISUM_X and BPM_ISUM_Y relative to the total current correction desired,
* Send these value to the FPGA,
* Increase ISUM_CNT by 1 to ask the correction algorithm to perform the total current correction on the next correction step. The correction will only be applied on the next step, BPM_ISUM_X and BPM_ISUM_Y will be considered as 0’s on the following steps.

This feature is enabled or not, depending on the value of ISUM_MODE:

* 0: Total current correction enabled
* 1: BPM_ISUM_X and BPM_ISUM_Y taken into account each cycle (for test purposes only)
* 2: Total current correction disabled

## Waveform for Septa damping

There are 2 sets of waveforms is order to damp the perturbations created when septa are fired, namely {WF_H1, WF_V1} and {WF_H2, WF_V2}. Each waveform is associated with gain coefficients for each steerer.

Each waveform can be enabled or not with the **WAVEFORMS_ENA** register (see description below).

Bits description for **WAVEFORMS_ENA** register:

| **Bit** | **Default** | **Description** |
|---------|-------------|-----------------|
| 0 | 0 | Enable WF_H1 when set to '1' |
| 1 | 0 | Enable WF_V1 when set to '1' |
| 2 | 0 | Enable WF_H2 when set to '1' |
| 3 | 0 | Enable WF_V2 when set to '1' |

## Triggers

_(new in v5.4.0)_

Trigger signals are used for multiple purposes: damping waveforms, signal generator or FOFB blanking.

Each FPGA boards have 4 trigger inputs, which makes a total of 32 possible trigger signal input. Each of these trigger signals are broadcasted on the Communication Controlled to all FPGA boards.

For each application, the input signal can be chosen using the Source Selection Registers defined below. The trigger can be an OR combinaison of many trigger inputs, but only triggers from the same FPGA board can be used for this OR combinaison.

The Trigger Counter register can be used to check when a new trigger was issued.

| Address(es) | Name | R/W | default value | comment |
|-------------|------|-----|---------------|---------|
| 0x0094 | s2hack_disabled_trig_num | R/W | 4 | not documented (_added in v5.1.13)_ |
| 0x0095 | trig_sin_delay | R |  | not documented (Debug register) |
| 0x0096 | TRIG_WF1_SELECT | R/W | 0x08 | Source Selection Register for S2 compensation:<br>select source to trigger S2 compensation (see description below) |
| 0x0097 | TRIG_WF1_CNT | R | 0 | Trigger Counter Register for S2 compensation:<br>increment each time a trigger is taken into account |
| 0x0098 | TRIG_WF2_SELECT | R/W | 0x04 | Source Selection Register for S3 compensation |
| 0x0099 | TRIG_WF2_CNT | R | 0 | Trigger Counter Register for S3 compensation |
| 0x009A | TRIG_SIGGEN_SELECT | R/W | 0x01 | Source Selection Register for the [Signal Generator](#FOFBregisters-siggen) |
| 0x009B | TRIG_SIGGEN_CNT | R | 0 | Trigger Counter Register for the Signal Generator |
| 0x009C | TRIG_BLANK_SELECT | R/W | 0x02 | Source Selection Register for FOFB [blanking](#FOFBregisters-blanking) |
| 0x009D | TRIG_BLANK_CNT | R | 0 | Trigger Counter Register for FOFB blanking |

Source Selection Register description:

| **Bit** | **Description** |
|---------|-----------------|
| \[3 .. 0\] | Input channel selection mask.<br>Bit #0 corresponds to the trigger input 0, setting it to 1 means the input will be used.<br>Same for the 3 other bits. |
| \[7 .. 4\] | FOFB FPGA board selection (should not exceed the total number of FPGA board minus 1) |

## Blanking

_(new in v5.4.3)_

The FOFB can be stopped for a short duration after a trigger signal is received. This is typically useful during the refill process to prevent the orbit distortion created by a pulse magnet to interfere with the FOFB.

To stop the FOFB, the blanking mechanism sets the error values to 0. By doing so (instead of sending a null correction) the integrated correction value is "kept" during the blanking interval.

To enable the blanking mechanism, 2 conditions has to be fulfilled:

1. The corresponding trigger has to be configured (see register TRIG_BLANK_SELECT)
2. The blanking duration register BLANKING_DURATION has to be different than 0

| Address(es) | Name | R/W | default value | comment |
|-------------|------|-----|---------------|---------|
| 0x00AF | BLANKING_DURATION | R/W | 30 | 16-bit unsigned register.<br>number of FA cycles during which the FOFB will be disabled after a blanking trigger. |

## Signal generator

It is possible to add a signal on a single corrector to the FOFB corrections. Signal can be a sinus or a pseudo-random noise. Output is active only when signal amplitude is non-zero.

Bits description for siggen_ampl register:

| **Bit** | **Default** | **Description** |
|---------|-------------|-----------------|
| \[6 .. 0\] |  | Signal amplitude (0/+127, scaled internally as 0/+1) |
| 7 | X | Unused |
| 8 |  | Axis selection:<br>Bit set to 0: X direction<br>Bit set to 1: Y direction |
| \[31 .. 9\] | X | Unused |

 Bits description for siggen_freq register:

| **Bit** | **Default** | **Description** |
|---------|-------------|-----------------|
| \[9 .. 0\] |  | Frequency of generated signal (10 bits unsigned):<br>Value = 1 to 1022: sinusoidal output with frequency between 1 Hz and 1022 Hz<br>Value = 1023: output a pseudo-random noise (4 kHz BW) |
| \[31.. 10\] | X | Unused |

 Bits description for siggen_corr_n register:

| **Bit** | **Default** | **Description** |
|---------|-------------|-----------------|
| \[3 .. 0\] |  | Select corrector output for the signal generator (0 to 13) |
| \[31.. 4\] | X | Unused |

## Fault on BPM position

As a safety measure the FOFB can stop if one position exceed a specified value. This protects the machine from errors from BPM measurement and a subsequent orbit correction which would, instead, degrade the orbit.

By default the BPM limit feature is enabled, which means that the FOFB will stop immediately if this condition is fulfilled:

FOFB stops if $`\left|X_i - X_{i,\text{offset}} \right| >= \text{BPM\_POSLIMIT\_X}`$ for any valid BPM $`i`$

Same thing in Y plane.

This feature can be disabled for all BPM using the [LIMITS_DISABLE](#FOFBregisters-LIMITS_DISABLE) register.

| Address(es) | Name | R/W | comment |
|-------------|------|-----|---------|
| 0x00BB | [LIMITS_DISABLE](#FOFBregisters-LIMITS_DISABLE) | R/W | disable fault on X BPM position for all BPMs |
| 0x00BC | [LIMITS_ENABLE](#FOFBregisters-LIMITS_ENABLE) | R/W | The "Enable limit" part of the register was removed |
| 0x00BD | BPM_POSLIMIT_X | R/W | BPM deviation limit value for X plane (16-bit integer expressed in µm) |
| 0x00BD | BPM_POSLIMIT_Y | R/W | BPM deviation limit value for Y plane (16-bit integer expressed in µm) |

### LIMITS_DISABLE

Bits description for limits_disable register:

<table>
<tr>
<td>

**Bit**
</td>
<td>

**Default**
</td>
<td>

**Description**
</td>
</tr>
<tr>
<td align="center">0</td>
<td align="center">0</td>
<td>Set to '1' to disable error on X BPM overflow</td>
</tr>
<tr>
<td align="center">1</td>
<td align="center">0</td>
<td>Set to '1' to disable error on Y BPM overflow</td>
</tr>
<tr>
<td align="center">2</td>
<td align="center">0</td>
<td>Set to '1' to disable error on X BPM not synchro</td>
</tr>
<tr>
<td align="center">3</td>
<td align="center">0</td>
<td>Set to '1' to disable error on Y BPM not synchro</td>
</tr>
<tr>
<td align="center">4</td>
<td align="center">0</td>
<td>

Set to '1' to disable error on X Steerer \> limit
</td>
</tr>
<tr>
<td align="center">5</td>
<td align="center">0</td>
<td>

Set to '1' to disable error on Y Steerer \> limit
</td>
</tr>
<tr>
<td align="center">6</td>
<td align="center">0</td>
<td>

Set to '1' to disable error on X Pos \> limit
</td>
</tr>
<tr>
<td align="center">7</td>
<td align="center">0</td>
<td>

Set to '1' to disable error on Y Pos \> limit
</td>
</tr>
<tr>
<td>

\[31 .. 0\]
</td>
<td>X</td>
<td>Unused</td>
</tr>
</table>

### LIMITS_ENABLE

Bits description for limits_enable register:

<table>
<tr>
<td>

**Bit**
</td>
<td>

**Default**
</td>
<td>

**Description**
</td>
</tr>
<tr>
<td>

\[3 .. 0\]
</td>
<td>X</td>
<td>Unused</td>
</tr>
<tr>
<td>4</td>
<td></td>
<td>

Reset errors<br>Reset all errors when this bit is changed from 0 to 1 (the '1' state has to be kept for at least 200 micro-seconds). Note that the user has to put it back to 0 before reseting errors again.
</td>
</tr>
<tr>
<td>5</td>
<td></td>
<td>

Bit set to 1: Disable channel #14<br>To be used on the 8th station only since the channel is used for the RF correction calculation and PID’s  output must be inhibited
</td>
</tr>
<tr>
<td>

\[31.. 6\]
</td>
<td>X</td>
<td>Unused</td>
</tr>
</table>

## Disable a BPM

It is possible to remove one BPM from the fast orbit correction. This can be done on the fly (without the need to stop the FOFB), and can be performed on one plane only, or both planes. For that purpose, two arrays of 1 bit are available (one for each plane): BPM_DISABLE_X and BPM_DISABLE_Y (see description below).

When the value '1' is used in the corresponding address of a BPM, the position value used for that BPM (after subtraction of the offset) is 0. The "Fault on BPM position" feature is also disabled for that BPM, on that plane. Be careful that this feature used the same correction matrix as when the BPM was not disabled.

| Address(es) | Name | R/W | comment |
|-------------|------|-----|---------|
| 0x0C00 – 0x0DFF | BPM_DISABLE_X | R/W | an array of 1-bit registers used to disable X BPM position from the fast orbit correction ('1' means disabled),<br>BPMs' ID start at 1, so C04-01 is controlled with register 0x0C01,<br>default value is '0' for all BPMs |
| 0x0E00 – 0x0FFF | BPM_DISABLE_Y | R/W | same as above, for Y plane |

## Report on Fault

see paper documentation

## Communication Controller

see paper documentation
