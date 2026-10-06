---
title: FOFB cc registers
---
# CC Control register

## Addr 0x0001, CC_CTRL

<table>
<tr>
<td>0</td>
<td>0</td>
<td>

RTS (Request To Send) bit:<br>Set to ‘1’ to apply new settings written in the configuration registers. Interface logic automatically de-asserts this bit as soon as these data are made taken into account.
</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>Unused</td>
</tr>
<tr>
<td>2</td>
<td>0</td>
<td>fofb_err_clear (hard, soft and frame error)</td>
</tr>
<tr>
<td>3</td>
<td>0</td>
<td>

CC enabled/disabled<br>1 -\> enabled
</td>
</tr>
<tr>
<td>

\[31..4\]
</td>
<td>

</td>
<td>Unused</td>
</tr>
</table>

# Ctrl/Status registers

## Addr 0x0002, CORR_CTRL

<table>
<tr>
<td>

**Bit**
</td>
<td align="center">

**Default**
</td>
<td>

**R/W**
</td>
<td>

**require RTS**
</td>
<td>

**Description**
</td>
</tr>
<tr>
<td>0</td>
<td align="center">

0
</td>
<td>R/W</td>
<td align="center">

</td>
<td>

RTS (Request To Send) bit:<br>Set to ‘1’ to apply new PID coeffs and position offsets. Interface logic automatically de-asserts this bit as soon as the new values have been taken into account.
</td>
</tr>
<tr>
<td>1</td>
<td align="center">0</td>
<td>R/W</td>
<td align="center">X</td>
<td>

Start bit:<br>Corrector requests to start (from user)
</td>
</tr>
<tr>
<td>2</td>
<td align="center">0</td>
<td>R/W</td>
<td align="center">X</td>
<td>

Stop bit:<br>Corrector requests to stop processing (from user)
</td>
</tr>
<tr>
<td>3</td>
<td>

</td>
<td>R</td>
<td align="center">

</td>
<td>

Correction status<br>0: correction is ON<br>1: correction is OFF
</td>
</tr>
<tr>
<td>4</td>
<td align="center">

</td>
<td>R</td>
<td align="center">

</td>
<td>

Same signal as above (bit 3)

→ Not in the original documentation
</td>
</tr>
<tr>
<td>

\[7..5\]
</td>
<td align="center">X</td>
<td>R/W</td>
<td align="center">X</td>
<td>Unused</td>
</tr>
<tr>
<td>

\[17..8\]
</td>
<td align="center">All 1</td>
<td>R/W</td>
<td align="center">X</td>
<td>

Corrector nodes mask (1bit for each corrector)<br>Bit set to 0 indicates that the associated corrector is not in service. Bit set to 1 otherwise.

Ex: if bit 9 = 0 then corrector id=241+ (9-8)=242 is not operational.
</td>
</tr>
<tr>
<td>

\[27..20\]
</td>
<td align="center">X</td>
<td align="center">R/W</td>
<td align="center">X</td>
<td>Unused</td>
</tr>
<tr>
<td>

\[29..28\]
</td>
<td align="center">

</td>
<td>R</td>
<td align="center">

</td>
<td>

Correction status (both bits)<br>0: correction is OFF<br>1: correction is ON

→ Not in the original documentation
</td>
</tr>
<tr>
<td>

\[31..30\]
</td>
<td>

</td>
<td>R</td>
<td align="center">

</td>
<td>Unused</td>
</tr>
</table>

# CC configuration registers (0x6400 - 0x65FF)

Read/Write registers:

WARNING: after changing one of the register below, you need to set the ackknowledge bit in the CC_CTRL register (addr: 1) for the new value(s) to be taken into account.

<table>
<tr>
<td>bpm_id</td>
<td>0x6400</td>
<td>BPM ID value. From 0 to 255.</td>
</tr>
<tr>
<td>time_frame_length</td>
<td>0x6401</td>
<td>

Time frame length for data distribution in clock cycles in mgt clock domain. Set to required_frame_length \* 106.25e6. Default value 9000, which gives appx. 90usec.
</td>
</tr>
<tr>
<td>mgt_powerdown_rxuserrdy</td>
<td>0x6402</td>
<td>

\[7:4\]: RXUserRdy<br>\[3:0\]: Powerdown (these bits a not connected internally)<br>Set individual bit (3:0) to 1 in order to power down RocketIO.
</td>
</tr>
<tr>
<td>mgt_loopback</td>
<td>0x6403</td>
<td>Set individual bits (7:0) to required value in order to loopback RocketIO.</td>
</tr>
<tr>
<td>buf_clr_dly</td>
<td>0x6404</td>
<td>Delay before sending packet on the CC. Default value is 0.</td>
</tr>
<tr>
<td>golden_orb_x</td>
<td>0x6405</td>
<td>

Golden orbit x in \[nm\] (not used)
</td>
</tr>
<tr>
<td>golden_orb_y</td>
<td>0x6406</td>
<td>

Golden orbit y in \[nm\] (not used)
</td>
</tr>
<tr>
<td>reset_stat</td>
<td>0x640B</td>
<td>

Reset min, max values _(added in v5.1.7)_
</td>
</tr>
</table>

Read-only registers:

<table>
<tr>
<td>firmware_ver</td>
<td>0x6500</td>
<td>0x3010</td>
</tr>
<tr>
<td>pmc_heart_beat</td>
<td>0x6501</td>
<td>

</td>
</tr>
<tr>
<td>link_partner_1</td>
<td>0x6502</td>
<td>

</td>
</tr>
<tr>
<td>link_partner_2</td>
<td>0x6503</td>
<td>

</td>
</tr>
<tr>
<td>link_partner_3</td>
<td>0x6504</td>
<td>

</td>
</tr>
<tr>
<td>link_partner_4</td>
<td>0x6505</td>
<td>

</td>
</tr>
<tr>
<td>link_up</td>
<td>0x6506</td>
<td>

\[7:4\]: TX Channels UP

\[3:0\]: RX Channels UP
</td>
</tr>
<tr>
<td>time_frame_count</td>
<td>0x6507</td>
<td>

</td>
</tr>
<tr>
<td>hard_err_cnt_1</td>
<td>0x6508</td>
<td>

</td>
</tr>
<tr>
<td>hard_err_cnt_2</td>
<td>0x6509</td>
<td>

</td>
</tr>
<tr>
<td>hard_err_cnt_3</td>
<td>0x650a</td>
<td>

</td>
</tr>
<tr>
<td>hard_err_cnt_4</td>
<td>0x650b</td>
<td>

</td>
</tr>
<tr>
<td>soft_err_cnt_1</td>
<td>0x650c</td>
<td>

</td>
</tr>
<tr>
<td>soft_err_cnt_2</td>
<td>0x650d</td>
<td>

</td>
</tr>
<tr>
<td>soft_err_cnt_3</td>
<td>0x650e</td>
<td>

</td>
</tr>
<tr>
<td>soft_err_cnt_4</td>
<td>0x650f</td>
<td>

</td>
</tr>
<tr>
<td>frame_err_cnt_1</td>
<td>0x6510</td>
<td>

</td>
</tr>
<tr>
<td>frame_err_cnt_2</td>
<td>0x6511</td>
<td>

</td>
</tr>
<tr>
<td>frame_err_cnt_3</td>
<td>0x6512</td>
<td>

</td>
</tr>
<tr>
<td>frame_err_cnt_4</td>
<td>0x6513</td>
<td>

</td>
</tr>
<tr>
<td>rx_pck_cnt_1</td>
<td>0x6514</td>
<td>

</td>
</tr>
<tr>
<td>rx_pck_cnt_2</td>
<td>0x6515</td>
<td>

</td>
</tr>
<tr>
<td>rx_pck_cnt_3</td>
<td>0x6516</td>
<td>

</td>
</tr>
<tr>
<td>rx_pck_cnt_4</td>
<td>0x6517</td>
<td>

</td>
</tr>
<tr>
<td>tx_pck_cnt_1</td>
<td>0x6518</td>
<td>

</td>
</tr>
<tr>
<td>tx_pck_cnt_2</td>
<td>0x6519</td>
<td>

</td>
</tr>
<tr>
<td>tx_pck_cnt_3</td>
<td>0x651a</td>
<td>

</td>
</tr>
<tr>
<td>tx_pck_cnt_4</td>
<td>0x651b</td>
<td>

</td>
</tr>
<tr>
<td>fod_process_time</td>
<td>0x651c</td>
<td>

</td>
</tr>
<tr>
<td>bpm_count</td>
<td>0x651d</td>
<td>

</td>
</tr>
<tr>
<td>bpm_id_rdback</td>
<td>0x651e</td>
<td>

</td>
</tr>
<tr>
<td>tf_length_rdback</td>
<td>0x651f</td>
<td>

</td>
</tr>
<tr>
<td>powerdown_rdback</td>
<td>0x6520</td>
<td>

</td>
</tr>
<tr>
<td>loopback_rdback</td>
<td>0x6521</td>
<td>

</td>
</tr>
<tr>
<td>faival_rdback</td>
<td>0x6522</td>
<td>

</td>
</tr>
<tr>
<td>feature_rdback</td>
<td>0x6523</td>
<td>

</td>
</tr>
<tr>
<td>rx_maxcount</td>
<td>0x6524</td>
<td>

</td>
</tr>
<tr>
<td>tx_maxcount</td>
<td>0x6525</td>
<td>

</td>
</tr>
<tr>
<td>fodprocess_time_min</td>
<td>0x6526</td>
<td>

_(added in v5.1.7)_
</td>
</tr>
<tr>
<td>fodprocess_time_max</td>
<td>0x6527</td>
<td>

_(added in v5.1.7)_
</td>
</tr>
</table>

