<!--
.. title: Raspberry Pi I2C Quickstart
.. slug: raspberry-pi-i2c-quickstart
.. date: 2024-09-24 15:01:45 UTC+02:00
.. tags: raspberry pi, i2c
.. category: homelab
.. link: 
.. description: 
.. type: text
-->

Below are my notes concerning the control of a [Sparkfun MCP4725 12-bit DAC](https://www.sparkfun.com/products/12918) over I2C with a Raspberry Pi.

# Rasbperry Pi Setup

1. Enable the I2C interface if isn't already with `raspi-config`. Verify that the I2C device file(s) are present in `/dev/` with `ls /dev | grep i2c`. (I had two files: `i2c-1` and `i2c-2`.)
2. Install the `i2c-tools` package for debugging I2C interfaces.

```console
sudo apt update && sudo apt install -y i2c-tools
```

## i2cdetect

1. Attach the DAC to the Raspberry Pi. The pinout is simple:

| Raspberry Pi | MCP4725 |
|--------------|---------|
| GND          | GND     |
| 3.3V         | Vcc     |
| SCL          | SCL     |
| SDA          | SDA     |

2. Run  the command `i2cdetect -y 1`. This will check for a device on bus 1 (`/dev/i2c-1`) and automatically accept confirmations:

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

I haven't yet determined how to communicate with the DAC using `i2cset`.

# Python Operation

This is modified from [Sparkfun's tutorial](https://learn.sparkfun.com/tutorials/raspberry-pi-spi-and-i2c-tutorial/all). I don't know how to determine the register address from the datasheet.

```python
import smbus


def main(channel=1, device_address=0x60, register_address=0x40):
    # Initialize I2C (SMBus)
    bus = smbus.SMBus(channel)

    # Create a sawtooth wave 16 times
    for i in range(0x10000):

        # Create our 12-bit number representing relative voltage
        voltage = i & 0xfff

        # Shift everything left by 4 bits and separate bytes
        msg = (voltage & 0xff0) >> 4
        msg = [msg, (msg & 0xf) << 4]

        # Write out I2C command: address, reg_write_dac, msg[0], msg[1]
        bus.write_i2c_block_data(device_address, register_address, msg)


if __name__ == "__main__":
    main()

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
