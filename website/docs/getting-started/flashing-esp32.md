---
id: flashing-esp32
title: Flashing ESP32 devices firmware
sidebar_label: ESP32 devices
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## Prerequisites

### Check Data Cable

Plug your device into your computer using a USB cable and then do the following:
:::important
You may need to install a driver from Silicon Labs for the [CP210X USB to UART bridge](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)
:::

<Tabs
  groupId="operating-system"
  defaultValue="linux"
  values={[
    {label: 'Linux', value: 'linux'},
    {label: 'macOS', value: 'macos'},
    {label: 'Windows', value: 'windows'},
  ]}>
  <TabItem value="linux">

  > Open a `Terminal` and enter the following command:
  > ```bash
  > lsusb
  > ```

  </TabItem>
  <TabItem value="macos">

  > Navigate to `Apple Menu  > About This Mac > System Report... > Hardware > USB`.

  </TabItem>
  <TabItem value="windows">

  > Navigate to `Device Manager > Ports (COM & LPT)`.

  </TabItem>
</Tabs>

You should see something like `CP210X USB to UART Bridge Controller`. If you don't, you will need to install the appropriate driver from [Silicon Labs](https://silabs.com/developers/usb-to-uart-bridge-vcp-drivers).

### Download Latest Firmware

Prebuilt binaries for the supported radios are available in our releases. Your initial installation has to happen over USB from your Mac, Windows or Linux PC. Once our software is installed, all future software updates happen over bluetooth from your phone.

<!--- TODO I'd like to create prettier buttons for this than just a table --->
| [Current Firmware](https://github.com/meshtastic/meshtastic-device/releases/latest) | [List of Firmware Versions](https://github.com/meshtastic/meshtastic-device/releases/) |
| :--------------: | :-----------------------: |

:::note
The [T-Beam 0.7](../hardware/tbeam-hardware#t-beam---v07) board is an earlier version of the T-Beam board, and due to changes in the design in subsequent iterations this board uses a specific firmware file different from the other T-Beam boards.

`firmware-tbeam0.7-1.x.x.bin` is the correct firmware. `firmware-tbeam-1.x.x.bin` is incompatible. For all other [T-Beam](../hardware/tbeam-hardware) boards `firmware-tbeam-1.x.x.bin` is the correct selection.
:::

## Command Line Interface Instructions

<Tabs
  groupId="operating-system"
  defaultValue="linux"
  values={[
    {label: 'Linux', value: 'linux'},
    {label: 'macOS', value: 'macos'},
    {label: 'Windows', value: 'windows'},
  ]}>
  <TabItem value="linux">

  ### Install Prerequisite Software

  > Check if you have `python3` and `pip` installed with the following command
  > ```bash
  > python3 --version
  >  pip3 --version
  > ```
  > If `python3` is not installed, install with
  > ```bash
  > sudo apt-get update
  > sudo apt-get install python3.6
  > ```
  > If `pip` is not installed, install with
  > ```bash
  > sudo apt-get install python3-pip
  > ```

  ### Install `esptool`

  > ```bash
  > pip3 install --upgrade esptool
  > ```

  </TabItem>
  <TabItem value="macos">

  ### Install Prerequisite Software

  > OS X comes with `Python 2.7` installed, but not `pip`. The following uses Homebrew to install `python3` which includes `pip3`. On MacOS you will use `pip3` instead > of `pip`.

:::note
Check if you have Homebrew installed with the following command
```bash
brew -v
```
If it's not installed, follow the instructions on the [Homebrew website](https://brew.sh) before continuing.
:::

  > Check if you have `python3` and `pip` installed with the following command
  > ```bash
  > python3 --version
  > pip3 --version
  > ```
  > If `python3` is not installed, install with
  > Install Python3
  > ```bash
  > brew install python3
  > ```
  > Confirm `pip3` was installed alongside `python3`
  > ```bash
  > pip3 -v
  > ```

  ### Install `esptool`

  > ```bash
  > pip3 install --upgrade esptool
  > ```


  </TabItem>
  <TabItem value="windows">

  > - Download and install [Python](https://www.python.org/). When installing, make sure to click `Add Python X.Y to PATH`.
  > - Download and install [Gitbash](https://gitforwindows.org/) (or other appropriate shell) and run all subsequent commands from that shell.

:::note
Confirm installation of `python` & `pip` with the following commands.
```bash
py --version
```
```bash
pip --version
```
:::

  ### Install `esptool`

  > ```bash
  > pip install --upgrade esptool
  > ```

  </TabItem>
</Tabs>

### Confirm Communication With Chip

Connect the radio to your computer using a data USB cable. Confirm your device is talking to your computer using the following command:

:::important
On windows, you must explicitly declare esptools as a .py script. Use `esptools.py chip_id`.
:::

  ```bash title="Command"
  esptool chip_id
  ```
  ```bash title="Expected Output"
  # You should see a result similar to this:
  mydir$ esptool chip_id
  esptool.py v2.6
  Found 2 serial ports
  Serial port /dev/ttyUSB0
  Connecting....
  Detecting chip type... ESP32
  Chip is ESP32D0WDQ6 (revision 1)
  Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
  MAC: 24:6f:28:b5:36:71
  Uploading stub...
  Running stub...
  Stub running...
  Warning: ESP32 has no Chip ID. Reading MAC instead.
  MAC: 24:6f:28:b5:36:71
  Hard resetting via RTS pin...
  ```

### Navigate to Firmware

`cd` into the directory where you unzipped the latest release. For example:
```bash title="Example"
cd /Downloads/firmware/
```

### Install/Update Firmware
Install or Update the device that you have by using the following commands according to your operating system:

:::caution
Be very careful to install the correct load for your board. In particular the popular 'T-BEAM' radio from TTGO is not called 'TTGO-Lora' (that is a different board). So don't install the 'TTGO-Lora' build on a TBEAM, it won't work correctly.
:::

<Tabs
  groupId="operating-system"
  defaultValue="linux"
  values={[
    {label: 'Linux', value: 'linux'},
    {label: 'macOS', value: 'macos'},
    {label: 'Windows', value: 'windows'},
  ]}>
  <TabItem value="linux">

  #### Install

  > ```bash title="Command"
  > ./device-install.sh -f firmware-BOARD-VERSION.bin
  > ```

  #### Update

  > ```bash title="Command"
  > ./device-update.sh -f firmware-BOARD-VERSION.bin
  > ```

  </TabItem>
  <TabItem value="macos">

  #### Install

  > ```bash title="Command"
  > ./device-install.sh -f firmware-BOARD-VERSION.bin
  > ```

  #### Update

  > ```bash title="Command"
  > ./device-update.sh -f firmware-BOARD-VERSION.bin
  > ```

  </TabItem>
  <TabItem value="windows">

  #### Install

  > ```bash title="Command"
  > ./device-install.bat -f firmware-BOARD-VERSION.bin
  > ```

  #### Update

  > ```bash title="Command"
  > ./device-update.bat -f firmware-BOARD-VERSION.bin
  > ```

  </TabItem>
</Tabs>


## Graphical User Interface Instructions


### Download `ESPHome Flasher`

:::note
ESPHome Flasher also provides instructions to build from source or install via `pip` on their [README](https://github.com/esphome/esphome-flasher/).
:::

<Tabs
  groupId="operating-system"
  defaultValue="linux"
  values={[
    {label: 'Linux', value: 'linux'},
    {label: 'macOS', value: 'macos'},
    {label: 'Windows', value: 'windows'},
  ]}>
  <TabItem value="linux">

  | [ESPHome Flasher](https://github.com/esphome/esphome-flasher/releases/latest) |
  | :-----------------------------------------------------------: |

  ### Open ESPHome Flasher

  > There should be no installation required. Connect your device using a data USB cable and refresh the `Serial Port`. Select the port that your device is connected to.

:::note
ESPHome Flasher for linux is a prebuilt binary for Ubuntu. These instructions were only tested on Ubuntu. Mileage may very on other distros.
:::


  </TabItem>
  <TabItem value="macos">

  ### Open ESPHome Flasher

  > There should be no installation required. Connect your device using a data USB cable and refresh the `Serial Port`. Select the port that your device is connected to.

:::note
If you get an error saying `The application "ESPHome-Flasher.app" can't be opened` it is likely a permissions issue. See [here](https://github.com/esphome/esphome-flasher/issues/26#issuecomment-671061140) for instructions.
:::
:::note
>If you get an error saying `"ESPHome-Flasher.app" cannot be opened because the developer cannot be verified`, click `cancel`. Right click on the application and select `Open`. Select `Open` on the confirmation alert.
:::

  </TabItem>
  <TabItem value="windows">

  | [ESPHome Flasher](https://github.com/esphome/esphome-flasher/releases/latest) |
  | :-----------------------------------------------------------: |

  ### Open ESPHome Flasher

  > There should be no installation required. Connect your device using a data USB cable and refresh the `Serial Port`. Select the port that your device is connected to.


  </TabItem>
</Tabs>

### Select Firmware

Browse to the previously downloaded firmware and select the correct firmware based on the board type and frequency.

:::caution
Be very careful to install the correct load for your board. In particular the popular 'T-BEAM' radio from TTGO is not called 'TTGO-Lora' (that is a different board). So don't install the 'TTGO-Lora' build on a TBEAM, it won't work correctly.
:::

### Flash Firmware

Select `Flash ESP`. It may take a minute or two. Once complete, "Done! Flashing is complete!" will be shown.

## Over the Air Update Instructions

OTA updates are only currently available on Android.

<!--- TODO --->