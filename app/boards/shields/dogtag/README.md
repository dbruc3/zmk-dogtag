# DogTag for ZMK

This shield ports the [DogTag QMK definition](https://github.com/qmk/qmk_firmware/tree/master/keyboards/takashicompany/dogtag) to ZMK for one nine-key pad and its rotary encoder. This diagnostic layout temporarily uses the key that previously typed Z to switch to the Bluetooth layer. Press it again to return to the base layer. The matrix transform temporarily includes all ten possible positions so the physical ninth switch can be located.

To test this build, stay on the base layer and tap the top-row keys from left to right. Record the characters they produce. The two possible end positions are marked **X** and **V**. No physical key produced X in the previous build, so locating V will identify the unused matrix position. The encoder changes volume instead of scrolling.

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

The nine switches and encoder press have been tested on physical DogTag hardware. Encoder rotation and Bluetooth layer activation are being diagnosed. Check the controller model and PCB orientation when flashing it.
