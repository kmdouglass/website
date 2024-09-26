<!--
.. title: Raspberry Pi I2C Quickstart
.. slug: raspberry-pi-i2c-quickstart
.. date: 2024-09-24 15:01:45 UTC+02:00
.. tags: raspberry pi, i2c
.. category: homelab
.. link: 
.. description: Get started with I2C on the Raspberry Pi with a MCP4725 DAC
.. type: text
.. has_math: true
-->

Below are my notes concerning the control of a [Sparkfun MCP4725 12-bit DAC](https://www.sparkfun.com/products/12918) over I2C with a Raspberry Pi.

# Rasbperry Pi Setup

1. Enable the I2C interface if isn't already with `raspi-config`. Verify that the I2C device file(s) are present in `/dev/` with `ls /dev | grep i2c`. (I had two files: `i2c-1` and `i2c-2`.)
2. Install the `i2c-tools` package for debugging I2C interfaces.

```console
sudo apt update && sudo apt install -y i2c-tools
```

## i2cdetect

Attach the DAC to the Raspberry Pi. The pinout is simple:

| Raspberry Pi | MCP4725 |
|--------------|---------|
| GND          | GND     |
| 3.3V         | Vcc     |
| SCL          | SCL     |
| SDA          | SDA     |

Next, run  the command `i2cdetect -y 1`. This will check for a device on bus 1 (`/dev/i2c-1`) and automatically accept confirmations:

```console
leb@raspberrypi:~/$ i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: 60 -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

Each I2C device must have a unique 7-bit address, i.e. 0x00 to 0x7f. The ranges [0x00, 0x07] and [0x78, 0x7f] are reserved. The above output indicates the DAC is at address 0x60. (Rows are the value of the first hexadecimal number of the address, columns are the second.)

## i2cset

`i2cset` is a command line tool that is part of `i2c-tools` and that is used to write data to I2C devices. I can set the voltage output of the DAC to 0 as follows:

```console
i2cset -y 1 0x60 0x40 0x00 0x00 i
```

The arguments mean the following:

- **-y** : Auto-confirm
- **1** : Use the device on bus 1
- **0x60** : Use the device at address **0x60**
- **0x40** : This is a command byte
- **0x00 0x00** : These two data bytes specify the DAC output level
- **i** : This is the write mode. `i` means I2C block write: [https://docs.kernel.org/i2c/smbus-protocol.html#i2c-block-write](https://docs.kernel.org/i2c/smbus-protocol.html#i2c-block-write)

### Command byte

The command byte is explained on pages 23 and 25 of the [MCP4725 datasheet](https://ww1.microchip.com/downloads/en/devicedoc/22039d.pdf). From most-significant to least-significant bits, the bits mean:

1. **C2** : command bit
2. **C1** : command bit
3. **C0** : command bit 
4. **X** : unused
5. **X** : unused
6. **PD1** : Power down select
7. **PD0** : Power down select
8. **X** : unused

According to Table 6-2 and Figure 6-2, `C2, C1, C0 = 0, 1, 0` identifies the command to write to the DAC register and NOT also to the EEPROM. In normal operation, the power down bits are 0, 0 (page 28).

So, to write to the DAC register, we want to send `0b01000000` which in hexadecimal is `0x40`.

### Data bytes to voltage

The data bytes are explained in Figure 6-2 of the datasheet. The first byte contains bits 11-4, and the second byte bits 3-0 in the most-significant bits:

```D11 D10 D9 D8   D7 D6 D5 D4 | D3 D2 D1 D0  X X X X```

12-bits are used because this is a 12-bit DAC. The mapping between bytes and voltage is:

| Data bytes, hex | Data bytes, decimal | Voltage |
|-----------------|---------------------|---------|
| 0x00 0x00       | 0                   | 0       |
| 0xFF 0xF0       | 65520               | V_max   |

where V_max is the voltage supplied to the chip's Vcc pin (3.3V in my case). The output step size is \\( \Delta V = V_{max} / 4096 \\) or about 0.8 mV.

# Control via Python

This is modified from [Sparkfun's tutorial](https://learn.sparkfun.com/tutorials/raspberry-pi-spi-and-i2c-tutorial/all) and uses the smbus Python bindings. Be aware that the tutorial example has a bug in how it prepares the list of bytes to send to the DAC.

```python
import smbus


OUTPUT_MAX: int = 4095
V_MAX = 3.3


def send(output: float, channel: int = 1, device_address: int = 0x60, command_byte: int = 0x40):
    assert output > 0.0 and output <= 1.0, "Output voltage must be expressed as fraction of the maximum in the range [0.0, 1.0]"

    bus = smbus.SMBus(channel)

    output_bytes = int(output * OUTPUT_MAX) & 0xfff
    data_byte_0: int = (output_bytes & 0xff0) >> 4  # First data byte
    data_bytes: list[int] = [data_byte_0, (output_bytes & 0xf) << 4]  # Second data byte

    bus.write_i2c_block_data(device_address, command_byte, data_bytes)


if __name__ == "__main__":
    output: float = 0.42
    send(output)

    print(f"Estimated output: {output * V_MAX}")

```

# Misc.

## Basic Calculator `bc`

This is a command line calculator and can be used for hexadecimal, binary, and decimal conversions. Install with `apt install bc`.

```console
# Convert 0x40 to binary
echo "ibase=16; obase=2; 40" | bc

# Convert 0x40 to decimal
echo "ibase=16; 40" | bc
```

**Note that hexadecimal values must be uppercase, e.g. 0xC7, not 0xc7!**
