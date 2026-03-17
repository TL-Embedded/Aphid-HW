# Aphid-HW

The Aphid Debugger provides a SWD and UART link via FT2232.

The 10 pin connector is compatible with the [10 pin standard here](https://github.com/TL-Embedded/SWD-UART-CTX-10).

The DTR signal can be used to apply +5V to the target `VCC AUX`.
The RTS signal can be used to ground the target `Detect`.

| Pin | Signal     | IO                  | Description |
| --- | ---------- | ------------------- | ----------- |
| 1   | VCC Target | Power input         | Reference voltage for logic IO |
| 2   | SWDIO      | Logic bidirectional | SWD Data line |
| 3   | GND        |                     | Common ground |
| 4   | SWCLK      | Logic output        | SWD Clock line |
| 5   | VCC Aux    | Power output        | 5V power output |
| 6   | N/C        |                     | Not connected |
| 7   | TX         | Logic output        | UART Transmit |
| 8   | Detect     | Open drain          | Debugger presence indicator |
| 9   | RX         | Logic input         | UART Receive |
| 10  | Reset      | Open drain          | Reset signal |

> [!CAUTION]
> DTR is usually automatically enabled by most terminal programs, which will enable `VCC Aux`. Ensure your target supports +5V on this pin, or remove R11 to disable this output.

# SWD interface

The SWD Debug interface is available on port 0 of the FT2232 as a D2XX device. It provides control of the SWD interface & Reset.

This is intended to be compatible with [OpenOCD](https://openocd.org/) using the [aphid-v1.cfg](./config/aphid-v1.cfg) config.

This may be copied into your openocd scripts folder, ie:
```
cp ./config/aphid-v1.cfg /usr/share/openocd/scripts/interface/ftdi/
```

The following example will validate the SWD interface is working (make sure to use the correct target).
```
openocd -f config/aphid-v1.cfg -f target/stm32g0x.cfg \
    -c "init" \
    -c "reset halt" \
    -c "mdw 0x08000000 4" \
    -c "resume" \
    -c "shutdown" \
```

# UART interface

The UART interface is available on port 1 of the FT2232 as a VCOM device.

The RTS and DTR signals are used for additional control:
 * RTS active enables the Detect signal
 * DTR active enables the VCC Aux output

# FTDI Programming

The FT2232 is programmed using [FTDI PROG](https://ftdichip.com/utilities/) via USB. The programming template is provided [here](./config/aphid.ftdiconf.xml).
