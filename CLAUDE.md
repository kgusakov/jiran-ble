# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a [ZMK firmware](https://zmk.dev) configuration repository for the Jiran BLE Lite — a split wireless ergonomic keyboard (GB revision). The keyboard has two halves (left/right), each built as a separate firmware image. The MCU is nRF52840 (Nordic Semiconductor).

## Building Firmware

Firmware is built exclusively via **GitHub Actions** — there is no local build setup. To trigger a build:

1. Push changes to the repository
2. GitHub Actions will automatically build firmware for both halves using `build.yaml`
3. Download the resulting `firmware.zip` artifact from the Actions tab

The `build.yaml` matrix defines two targets:
- `jiran_ble_lite_left`
- `jiran_ble_lite_right`

## Key Files

| File | Purpose |
|------|---------|
| `config/boards/arm/jiran_ble_lite/jiran_ble_lite.keymap` | Keymap definition (layers, bindings) |
| `config/jiran_ble_lite.conf` | ZMK Kconfig options (sleep, BT TX power, etc.) |
| `config/boards/arm/jiran_ble_lite/jiran_ble_lite_right.dts` | Right half GPIO pin assignments |
| `config/info.json` | Physical key layout positions (used by keymap editor) |
| `build.yaml` | GitHub Actions build matrix |

## Keymap Structure

The keymap uses ZMK's devicetree syntax (`.keymap` files). The current layout has two layers:

- **Layer 0** (`default_layer`): Standard QWERTY layout with modifier hold-taps (`mt`)
- **Layer 1** (`lower_layer`): Function keys, navigation (arrows, Home/End/PgUp/PgDn), media controls, and Bluetooth management (`bt BT_NXT`, `bt BT_PRV`, `bt BT_CLR`)

Layer 1 is activated by holding the `FN` key (`mo 1`) on either thumb cluster.

The keyboard matrix is 5 rows × 6 columns per half (30 keys/side), plus 3 thumb keys per side = 66 keys total.

## ZMK Behavior Reference

- `&kp KEY` — key press
- `&mt MOD KEY` — mod-tap (hold for modifier, tap for key)
- `&mo N` — momentary layer switch
- `&trans` — transparent (pass through to lower layer)
- `&sys_reset` — reset the controller
- `&bt BT_*` — Bluetooth profile management

ZMK docs: https://zmk.dev/docs
