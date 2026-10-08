# DogTag for ZMK

This shield ports the [DogTag QMK definition](https://github.com/qmk/qmk_firmware/tree/master/keyboards/takashicompany/dogtag) to ZMK. It supports the reversible PCB as a left or right half, its nine switches per half, and rotary encoder. The default 18-key map follows QMK's `LAYOUT` order. Tap the top outside key for Escape or hold it for the Bluetooth layer. On that layer, Q, W, E, A, and S select Bluetooth profiles 1 through 5 respectively. The encoder scrolls vertically.

## Controller and connection

ZMK does not run on the original ATmega32U4 Pro Micro. Replace each Pro Micro with a pin-compatible ZMK controller such as a **nice!nano**. Use `dogtag_left` for the central half and `dogtag_right` for the peripheral half. These builds communicate over Bluetooth; the DogTag TRRS cable is **not used**. Do not connect the TRRS cable between powered nice!nano halves. One left half works as a nine-key pad without a right half attached.

The shield uses the QMK pin assignment translated to Pro Micro connector labels:

| Function | QMK pins | ZMK Pro Micro pins |
| --- | --- | --- |
| Columns, left | F4 F5 F6 F7 B1 | 21 20 19 18 15 |
| Columns, right | B1 F7 F6 F5 F4 | 15 18 19 20 21 |
| Rows | B2 B6 | 16 10 |
| Encoder A/B | D4 C6 | 4 5 |

The original split serial pin D2 maps to connector pin 1, but this port uses ZMK's Bluetooth split transport. Each half needs its own power source for wireless use.

## Build

On GitHub, open **Actions → DogTag firmware → Run workflow** and choose `main`. The workflow builds both halves; download the `dogtag_left-nice_nano` and `dogtag_right-nice_nano` artifacts from the completed run.

For a local build, use a [ZMK west workspace](https://zmk.dev/docs/development/setup) and run from the repository root:

```sh
west build -s app -p -b nice_nano/nrf52840/zmk -- -DSHIELD=dogtag_left
west build -s app -p -b nice_nano/nrf52840/zmk -- -DSHIELD=dogtag_right
```

Copy each resulting `build/zephyr/zmk.uf2` before starting the next build. Flash the matching image to each half. Edit `dogtag.keymap` for bindings and `dogtag.conf` for features.

The pin map and firmware have not yet been tested on physical DogTag hardware. Check the controller model, PCB revision, and encoder direction when flashing the first pair.
