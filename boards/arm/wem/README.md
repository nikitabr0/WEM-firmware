# WEM Mouse

## Hardware

- nRF52840 SoC
- 6 Mouse Buttons:
  - Left Mouse Button (LMB) - GPIO P0.02
  - Middle Mouse Button (MMB) - GPIO P0.03
  - Right Mouse Button (RMB) - GPIO P0.04
  - Back Button - GPIO P0.28
  - Forward Button - GPIO P0.29
  - F13 Button - GPIO P0.30
- USB-C connector for USB connectivity
- Bluetooth Low Energy support
- Optional WS2812 RGB LED strip support (GPIO P0.06)
- Battery voltage monitoring (ADC channel 2)
- Status LED (GPIO P0.15)

## Features

- USB HID Mouse
- Bluetooth LE Mouse (up to 5 connections)
- Battery level reporting
- Underglow RGB LED support (optional)

## Pin Configuration

### Buttons
| Button  | GPIO   | Function        |
|---------|--------|-----------------|
| LMB     | P0.02  | Left Click      |
| MMB     | P0.03  | Middle Click    |
| RMB     | P0.04  | Right Click     |
| Back    | P0.28  | Browser Back    |
| Forward | P0.29  | Browser Forward |
| F13     | P0.30  | F13 Key         |

### LEDs
| LED         | GPIO  | Function    |
|-------------|-------|-------------|
| Status LED  | P0.15 | Blue LED    |
| WS2812      | P0.06 | RGB Strip   |

### Other
| Function       | Pin/Channel |
|----------------|-------------|
| Battery ADC    | ADC2        |
| USB D+         | P0.31       |
| USB D-         | P0.29       |

## Building

To build ZMK firmware for the WEM mouse:

```bash
west build -b wem -- -DSHIELD=wem
```

## Flashing

### Using nrfjprog
```bash
west flash
```

### Using UF2 Bootloader
1. Enter bootloader mode (double-tap reset or hold button while plugging in)
2. Copy the generated `wem.uf2` file to the USB mass storage device

### Using J-Link
```bash
west flash --runner jlink
```

### Using OpenOCD
```bash
west flash --runner openocd
```

## Configuration

The default configuration includes:
- USB HID Mouse support
- Bluetooth LE with up to 5 simultaneous connections
- Battery monitoring
- WS2812 RGB underglow support

## Keymap

Create your keymap file at `config/wem.keymap`:

```dts
#include <behaviors.dtsi>
#include <dt-bindings/zmk/keys.h>
#include <dt-bindings/zmk/bt.h>
#include <dt-bindings/zmk/mouse.h>

/ {
    keymap {
        compatible = "zmk,keymap";
        
        default_layer {
            bindings = <
                &mkp LCLK    // Left mouse button
                &mkp MCLK    // Middle mouse button
                &mkp RCLK    // Right mouse button
                &kp K_BACK   // Back button
                &kp K_FORWARD // Forward button
                &kp F13      // F13 key
            >;
        };
    };
};
```

## Troubleshooting

### USB not detected
- Verify USB D+ and D- connections
- Check that `CONFIG_USB_DEVICE_STACK=y` is set
- Ensure the board is powered correctly

### Bluetooth pairing issues
- Clear bonding information: Press a defined BT clear key
- Ensure `CONFIG_BT_MAX_CONN` and `CONFIG_BT_MAX_PAIRED` are set appropriately
- Check antenna connection if using external antenna

### Buttons not responding
- Verify GPIO pin assignments match your PCB
- Check pull-up resistors are configured correctly
- Test continuity between button and GPIO pin

## License

MIT License
