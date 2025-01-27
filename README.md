# JTAG library for FT232H on OCS Tray Mezzanine Adapter

## Current state

- JTAG scan chain and programming of the FPGA works (see the screenshot below)
- Initialization of the MPSSE is not implemented --> run OpenOCD once to init the device
- Smaller improvements also pending

### Download time

The following table present the bitstream download time at different clock
speeds for 5SGSMD5 with the bitstream size of 213,798,880 bits (cca 26 MB).

| Clock rate [MHz] | Download time [mm:ss] |
|------------------|-----------------------|
|  2               | 03:51                 |
|  4               | 02:50                 |
|  6               | 02:24                 |
| 10               | 02:03                 |
| 20               | 01:54                 |

### Initialization

To initialize the FT232H, run the following command:

```
openocd \
    -f interface/ftdi/um232h.cfg \
    -c "adapter_khz 2000; transport select jtag; jtag newtap auto0 tap -irlen 10 -expected-id 0x029070dd";
```

### Usage example

Result after programming the bitstream:

![Quartus Programmer](docs/current_state_2.png)

## Useful links

  * https://forums.intel.com/s/question/0D50P00003yyL2bSAE/bemicro-and-programming-under-linux
  * https://community.intel.com/t5/Programmable-Devices/MAX10-JTAG-Instruction-Set-Understanding-what-USBBlaster-is/td-p/218784

## Dummy device

To install the library:

```
sudo ln -sf $(readlink -f libjtag_hw_dummy.so) /opt/intelFPGA/19.1/quartus/linux64/
```

To listen do the debug log:

```
nc -lkuU /var/tmp/jtag-dummy.sock
```