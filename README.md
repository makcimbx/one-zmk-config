# ZMK Firmware for Ergonaut One keyboard

This is a repository for a ZMK Firmware for Ergonaut One keyboard.

## BLE stable fork notes

This fork keeps the Ergonaut One firmware on a known-good ZMK release and includes a Bluetooth compatibility workaround for Windows hosts.

Solved issues observed with the upstream `main`-based build:

- Repeated Bluetooth connect/disconnect loop after pressing `RESET` once.
- Repeated Bluetooth reconnects after turning both halves off and on again.
- Pairing working immediately after `settings_reset`, then breaking after a reset or power cycle.
- The same issue reproduced when either XIAO BLE controller was flashed as the central/left half, which pointed to firmware/host compatibility rather than soldering.

Stability changes in this fork:

- ZMK is pinned to `v0.3.0` instead of tracking the moving `main` branch.
- The GitHub Actions build workflow is pinned to `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3.0`.
- `CONFIG_BT_CTLR_PHY_2M=n` is enabled to avoid 2M PHY pairing/reconnect issues seen with some Windows Bluetooth adapters.
- Keymap drawing uses the Ergonaut One layout definition from the ZMK module.

Prebuilt UF2 files are available in the fork release: https://github.com/makcimbx/one-zmk-config/releases/tag/ble-stable-2026-06-08

## Russian layout reference

The keyboard firmware sends normal US keycodes. Russian letters are produced by the operating system when the Russian input language is selected. Keymap Editor shows firmware keycodes, not OS language output, so use this table as the Russian ЙЦУКЕН reference while keeping the firmware layout unchanged.

```text
Left half                              Right half

]   Q   W   E   R   T                 Y   U   I   O   P   [
Ъ   Й   Ц   У   К   Е                 Н   Г   Ш   Щ   З   Х

`   A   S   D   F   G                 H   J   K   L   ;   '
Ё   Ф   Ы   В   А   П                 Р   О   Л   Д   Ж   Э

-   Z   X   C   V   B                 N   M   ,   .   /   \
-   Я   Ч   С   М   И                 Т   Ь   Б   Ю   .   \
```

Thumb keys keep their normal actions and do not correspond to Russian letters.

## Default keymap

Visual representation of the default keymap in keyboard-layout-editor: [KLE](http://www.keyboard-layout-editor.com/#/gists/13d0f7ae7a8b5835efcd23d61f50336a)

Below representation was generated with [`keymap-drawer`](https://github.com/caksoylar/keymap-drawer) – check out the automatically generated layouts using the [automated Github workflow](https://github.com/caksoylar/keymap-drawer/tree/main#setting-up-an-automated-drawing-workflow) for each keyboard in the [`keymap-drawer` folder](keymap-drawer/), which is always up to date with the config.

![Keymap Representation](./keymap-drawer/ergonaut_one.svg?raw=true "Keymap Representation")

This layout is heavily inspired from [Watchman 42-key layout](https://github.com/aroum/Watchman-layouts)

## FAQ

- [FAQ](#faq)
  - [How to change the keymap?](#how-to-change-the-keymap)
  - [How to flash the keyboard?](#how-to-flash-the-keyboard)
  - [How to pair halves?](#how-to-pair-halves)
  - [Problems](#problems)
    - [I'm getting File Transfer Error after copying firmware to the keyboard](#im-getting-file-transfer-error-after-copying-firmware-to-the-keyboard)

### How to change the keymap?

1. Fork or use this repository as a template https://github.com/ergonautkb/one-zmk-config.
2. Enable Github Actions for your repository.

You have two options on how to configure your desired keymap:

#### Option 1. Keymap Editor

1. Open [Keymap Editor](https://nickcoutsos.github.io/keymap-editor/).
2. Connect it to your Github account and give an access to your repository to Keymap Editor's app.
3. Make changes to your keymap and press `Save` - it will trigger software build. Wait for it to complete.
4. Grab the `firmware.zip` archive.

#### Option 2. Manual

1. Make changes to the [ergonaut_one.keymap](config/ergonaut_one.keymap) file using your favorite text editor.
2. Commit changes to your repository.
3. Go to `Actions` tab in your Github repository, locate the latest build and wait for it to complete.
4. Grab the `firmware.zip` archive

### How to flash the keyboard?

1. Obtain `firmware.zip`
2. Unzip `firmware.zip` - you should have `ergonaut_one_left-seeeduino_xiao_ble-zmk.uf2` and `ergonaut_one_right-seeeduino_xiao_ble-zmk.uf2` files
3. Turn off the power for selected halve (move slider to position `OFF`)
4. Connect selected halve to the PC via USB-C cable
5. Press `RESET` button **twice** to enter DFU mode - you should see new USB device in your file manager
6. Copy the corresponding firmware to the root directory of the new USB device
7. Disconnect selected halve from the PC
8. Repeat steps 3-7 for the other halve

### How to pair halves?

1. Turn off the power for both halves (move slider to position `OFF`)
2. Turn on the power for both halves (move slider to position `ON`)
3. Press `RESET` button **once** on both halves **simultaneously**

### Problems

#### I'm getting File Transfer Error after copying firmware to the keyboard

It's OK. Proof: https://zmk.dev/docs/troubleshooting#file-transfer-error
