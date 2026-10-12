# DogTag for ZMK

This shield ports the [DogTag QMK definition](https://github.com/qmk/qmk_firmware/tree/master/keyboards/takashicompany/dogtag) to ZMK for one nine-key pad and its rotary encoder. The pin order follows the left-hand assembly in QMK. The first mapped key is temporarily bound to Left Shift to test that switch; the Bluetooth layer cannot be entered with this diagnostic keymap. When the Bluetooth layer is accessible again, Q, W, E, A, and S select Bluetooth profiles 1 through 5 respectively, and pressing the encoder enters the bootloader. Clockwise/counterclockwise encoder rotation sends Down/Up on the base layer and Right/Left on the Bluetooth layer.

## Controller and connection

ZMK does not run on the original ATmega32U4 Pro Micro. Replace it with a pin-compatible ZMK controller such as a **nice!nano v2**. This is a single-board configuration; the DogTag TRRS cable is not used.

The shield uses the QMK pin assignment translated to Pro Micro connector labels:

| Function       | QMK pins       | ZMK Pro Micro pins |
| -------------- | -------------- | ------------------ |
| Columns        | F4 F5 F6 F7 B1 | 21 20 19 18 15     |
| Rows           | B2 B6          | 16 10              |
| Encoder A/B    | D4 C6          | 4 5                |

## Build

On GitHub, open **Actions → DogTag firmware → Run workflow** and choose `main`. Download the `dogtag-nice_nano_v2` artifact from the completed run.

For a local build, use a [ZMK west workspace](https://zmk.dev/docs/development/setup) and run from the repository root:

```sh
west build -s app -p -b nice_nano@2.0.0/nrf52840/zmk -- -DSHIELD=dogtag
```

Flash `build/zephyr/zmk.uf2` to the nice!nano v2. Edit `dogtag.keymap` for bindings and `dogtag.conf` for features.

The matrix transform follows the original single-DogTag layout. The regular Left Shift key works. The former Escape/layer key is temporarily another Left Shift binding so its physical switch can be checked by holding it and tapping A.
