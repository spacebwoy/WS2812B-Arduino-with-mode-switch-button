# WS2812B Arduino LED Controller

An Arduino Nano controller for a 349-pixel WS2812B strip. The sketch uses
[WS2812FX](https://github.com/kitesurfer1404/WS2812FX) for non-blocking effects,
two push buttons for control, and EEPROM to remember the selected mode, hue,
and effect speed.

## Hardware

- Arduino Nano ATmega328P (new bootloader)
- WS2812B LED strip (349 LEDs by default)
- Two normally-open push buttons
- External regulated 5 V power supply sized for the LED strip
- Common ground between the Nano and LED power supply
- Recommended: 300–500 ohm resistor in series with the data line and a 1000 uF
  capacitor across the strip's 5 V and ground near its input

Do not power the LED strip from the Nano's 5 V pin. Connect the strip's data
input to **D2**, its ground to the supply ground and Nano ground, and its 5 V
input to the external supply. Connect the mode button between **D7** and ground,
and the option button between **D8** and ground. Both button pins use the
internal pull-up resistors.

## Build and upload

Open the PlatformIO project containing the sketch in VS Code with the
PlatformIO extension, or use the PlatformIO CLI from that project's directory:

```sh
pio run
pio run --target upload
```

The project depends on WS2812FX 1.4.7 or newer; PlatformIO installs the
dependency from `platformio.ini`. Change `upload_port` in that file to match
your Nano's port if it is not `COM21`.

## Effects

The mode button advances through these modes and saves the selected mode:

| Mode | Effect |
|---:|---|
| 0 | Rainbow cycle |
| 1 | Running lights |
| 2 | Static hue |
| 3 | White light |
| 4 | Dual scan |
| 5 | Fade |
| 6 | Fire flicker |
| 7 | Three independently animated color-wipe sections |
| 8 | Rainbow chase |
| 9 | Hyper sparkle |

The defaults are 349 LEDs, data on D2, and brightness 200 (out of 255). Update
the constants at the top of the sketch to suit a different strip or pin.

## Option button

- **Short press in modes 1 and 2:** advance the hue by one step.
- **Short press in mode 3:** lower white brightness by 15; after the lowest
  step, return to the default brightness.
- **Short press in all other modes:** increase effect speed in 100 ms steps,
  wrapping from 1000 ms back to 100 ms.
- **Hold for at least 700 ms outside mode 3:** sweep smoothly through the full
  hue range in about three seconds. The selected hue is saved when the button
  is released. This changes the effect's primary color where the effect uses
  one; effects with built-in rainbow or other fixed colors may not visibly
  respond to hue.
- **Hold in mode 3:** restore default white brightness.

The selected mode, hue, and effect speed are restored from EEPROM at startup.
