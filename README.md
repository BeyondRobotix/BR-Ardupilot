# BR-Ardupilot

Beyond Robotix fork of [ArduPilot](https://github.com/ArduPilot/ardupilot).

This default branch contains only this README. The firmware source lives on the
branches listed below. Pick the branch for your board and clone it directly:

```sh
git clone --recurse-submodules -b <branch> https://github.com/BeyondRobotix/BR-Ardupilot.git
```

Build instructions are the same as upstream ArduPilot, see
[BUILD.md](https://github.com/ArduPilot/ardupilot/blob/master/BUILD.md). For example:

```sh
./waf configure --board BR_Core
./waf plane
```

## Branches

### Upstream

| Branch | Description |
|---|---|
| `master` | Mirror of ArduPilot `master`. Contains no Beyond Robotix changes; base for upstream contributions. |

### Flight controllers

| Branch | Board (hwdef) | Description |
|---|---|---|
| `BR_CORE_HW_DEF_2-4.7.0` | `BR_Core` | BR Core hardware **V1.5** (boards in the field) on ArduPilot 4.7.0 (Plane-4.7.0 tag). Board ID `AP_HW_MATEKH743` (1013), matching the MatekH743 bootloader V1.5 boards ship with. PWM 11 on PB1 (TIM3). |
| `BR_CORE_HW_DEF_V1.6-4.7.0` | `BR_Core` | BR Core hardware **V1.6** on ArduPilot 4.7.0. Board ID `AP_HW_BR_CORE` (1700): the board needs the V1.6 bootloader (`Tools/bootloaders/BR_Core_bl.bin` on this branch) flashed first. PWM 11 on PA3 (TIM15). |

Use the branch that matches the hardware revision. The uploader checks the board
ID, so V1.6 firmware won't load through the MatekH743 (1013) bootloader on V1.5
boards, and V1.5 firmware drives PWM 11 on the wrong pin on V1.6 boards.

### CAN / DroneCAN peripherals (AP_Periph)

The CAN node boards are built on the **latest stable ArduPilot release**,
currently `AP_Periph-4.7.3` (`d9c5097c3d`, the commit upstream's
`AP_Periph-stable` tag points to). Each branch is that tag plus our board
commits only, so they share one baseline. When a new stable release lands,
start a new `-<version>` branch from its tag and cherry-pick the board
commits across, rather than rebasing the old branch.

| Branch | Board (hwdef) | Baseline | Description |
|---|---|---|---|
| `Micro-Node-Periph-4.7.3` | `BeyondRobotixM-Periph` | `AP_Periph-4.7.3` | Micro Node (STM32L431) AP_Periph firmware. Also runs on the CAN Node and the Node Development Board. |
| `CAN-Node-Plus-Periph-4.7.3` | `BR_CANNodePlus` | `AP_Periph-4.7.3` | CAN Node Plus (STM32H723) AP_Periph firmware. |
| `Micro-Node-hwdef-2` | `BeyondRobotixM`, `BeyondRobotixM-Periph` | master, Mar 2025 | **Superseded** by `Micro-Node-Periph-4.7.3`. Embeds an ArduPilot bootloader. |
| `CAN-Node-Plus-Periph` | `BR_CANNodePlus` | master, Aug 2026 | **Superseded** by `CAN-Node-Plus-Periph-4.7.3`. |

Every CAN node ships with [BR_bootloader](https://github.com/BeyondRobotix/BR_bootloader),
whether it runs AP_Periph or Arduino DroneCAN, so either firmware can be
loaded over CAN. The hwdefs set `AP_BOOTLOADER_FLASHING_ENABLED 0`: AP_Periph
carries no ArduPilot bootloader and `FLASH_BOOTLOADER` can't overwrite ours.
A build gives `AP_Periph.bin` only. The factory `.hex` is BR_bootloader at
`0x08000000` plus that `.bin` at the app address (`0x0800A000` on the L431,
`0x08020000` on the H723); see
[BR-Flash-Test](https://github.com/BeyondRobotix/BR-Flash-Test) for the
merge script.

Other peripherals:

| Branch | Board (hwdef) | Description |
|---|---|---|
| `GNSS-MAG-Combo` | `BR_GNSS_MAG`, `BR_GNSS_DAN_F10N`, `BR_Mag_RM3100` | Combined GNSS + magnetometer node. |
| `BR_GNSS_DAN_F10N` | `BR_GNSS_DAN_F10N`, `BR_Mag_RM3100` | u-blox DAN-F10N GNSS node. |
| `BR_MAG_RM3100` | `BR_Mag_RM3100` | RM3100 magnetometer node. |
| `ArduinoDroneCAN-Bootloader` | `BeyondRobotixM` | Modified AP_Bootloader for the Arduino DroneCAN (Micro Node) board, May 2025. Predates BR_bootloader, which the CAN nodes now ship with. |

### Features

| Branch | Description |
|---|---|
| `dronecan-indicated-airspeed` | DroneCAN `IndicatedAirspeed` / `TrueAirspeed` support in AP_Airspeed, with autotest. Intended for upstream. |
| `AUAV-Add-All` | Additional AUAV airspeed sensor support (`MatekL431-AUAV`). |

### Personal / experimental

| Branch | Description |
|---|---|
| `dev/harvey` | Development branch based on 4.6.3. |
| `anon` | Experimental Micro Node work. |
| `copilot/*` | Automated assistant working branches. |

## Contributing upstream

Changes that are useful to the wider ArduPilot community should be submitted to
[ArduPilot/ardupilot](https://github.com/ArduPilot/ardupilot) from a branch based
on `master`, following the
[ArduPilot contributing guidelines](https://github.com/ArduPilot/ardupilot/blob/master/.github/CONTRIBUTING.md).

## License

ArduPilot is licensed under the
[GNU General Public License v3](https://github.com/ArduPilot/ardupilot/blob/master/COPYING.txt).
All branches in this repository are distributed under the same license.
