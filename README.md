# XLink-HW

Xlink provides a SWD and UART link via FT2232.

The 10 pin connector is compatible with the [10 pin standard here](https://github.com/TL-Embedded/SWD-UART-CTX-10).

The DTR signal can be used to apply +5V to the V_AUX input.
The RTS signal can be used to ground the DETECT input.

# Configuration

The following signals are expected to be inverted (via the FTDI conf hopefully)
 * RTS
 * DTR
 * SRST

The SWD interface is intended to be compatible with OpenOCD.

