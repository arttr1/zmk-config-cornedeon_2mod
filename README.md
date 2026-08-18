# zmk-config-cornedeon_2mod

ZMK firmware config for the [Cornedeon 2mod (Lite)](https://cornedeon.ru) keyboard —
a wired, RP2040-based split keyboard, ported from the
[QMK/Vial firmware](https://github.com/arttr1/cornedeon_2mod).

## Hardware

- MCU: RP2040 (both halves), `board=rpi_pico`
- Matrix: 4 rows x 6 columns per half (3 letter rows + 1 combined bottom/thumb row)
- Split link: wired, full-duplex UART over the TRRS/USB-C style cable
  (GP0 = TX, GP1 = RX on both halves — same wiring as the QMK firmware)
- Left half = central (connects to the host over USB), right half = peripheral
- ZMK Studio supported on the left (central) half via USB

## Layout

Base layer keeps the same key positions as the QMK/Vial keymap. Layers
(`num`, `nav`, `sys`) and home row mods were ported from
[zmk-config-fuji44](https://github.com/arttr1/zmk-config-fuji44).

The four outer bottom-row keys (Ctrl+Left on the left half, Down+Ctrl on the
right half) were intentionally left untouched. The remaining 8 bottom-row
positions carry the fuji44 thumb cluster: `mo NUM`, `mo NAV`, Enter, Cmd/GUI
on the left, and Shift, Backspace, Space, `mo SYS` on the right.

Hold-taps use the `tap-preferred` flavor (200ms tapping term, 125ms
require-prior-idle) to avoid the accidental-hold issue from QMK's default
tap-hold resolution.

## Building

Firmware is built automatically via GitHub Actions on every push (see
`build.yaml` / `.github/workflows/build.yml`), producing:

- `cornedeon_2mod_left-rpi_pico-zmk.uf2` — flash to the **left** half
- `cornedeon_2mod_right-rpi_pico-zmk.uf2` — flash to the **right** half

To flash: hold BOOTSEL while plugging in the RP2040, then drag the
corresponding `.uf2` file onto the `RPI-RP2` USB drive that appears.

## Editing the keymap

The keymap lives in `config/cornedeon_2mod.keymap`. After changing it, push
to GitHub and download the new firmware from the Actions run, or use
[ZMK Studio](https://zmk.dev/docs/features/studio) connected to the left
half over USB for live changes without reflashing.
