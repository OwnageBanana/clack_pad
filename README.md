# Clack Pad
a simple macro pad with a rotary encoder, an SPI SSD1306 128 x 64 0.96"  OLED display, WS2812 LEDs and of course 12 keyboard keys.

**this repo is a really big mess and still a work in progress**

outside of a micro-python starter kit for embedded programming this is one of my first learning experiences with pcb design, embedded programming and rust programming, I've almost exclusively been learning rust for embedded. So this is my excuse for all the messy code and choices that don't really make sense in this project.

## plans for the pad

- USB HID that has NKRO (currently a 6 key report)
- USB media device reports
- macro keys
  - modifiers + key
  - sequence of keys to type out
- layers for different programs
- rotary encoder with interrupts
- LED patterns for layers, per key, runtime adjustable

stretch goal:

- simple UI for the screen
- writing and reading to flash for configurations
  - goal: to avoid building code to reconfigure, one could instead send config though serial usb or a menu on the device

## build steps

build with how you build!
... follow the [RP-Hal](https://github.com/rp-rs/rp-hal) documentation to get things up and running.

If you are on linux, check out the next section to install the proper dependencies.

```
cargo build

cargo build --release

```

you can also run and the built binaries will be transfered to the micro controller when in USB Mass Storage Device mode

```
cargo run

cargo run --release
```

## Additional installation steps for linux

the RP-HAL repo leaves out some linux specific steps and dependencies. You'll need libusbx for using the thumbv6m-non-eabi, proberun and elf2uf2-rs to have cargo automatically transfer the built code to the rp2040 flash

pre-requisites are having rust/cargo
``` bash
# required deps on linux
# fedora
sudo dnf install -y libusbx-devel libftdi-devel libudev-devel
# debian, ubuntu (untested)
sudo apt install libusbx-devel libftdi-devel libudev-devel

rustup self update
rustup update stable
rustup target add thumbv6m-none-eabi

# Useful to creating UF2 images for the RP2040 USB Bootloader
cargo install elf2uf2-rs --locked
# Useful for flashing over the SWD pins using a supported JTAG probe
cargo install probe-run
```


https://github.com/probe-rs/probe-rs

https://github.com/rp-rs/rp-hal


## device setup

some additional notes on the setup of the usb device

```rust

/// USB VIP for a generic keyboard from
/// https://github.com/obdev/v-usb/blob/master/usbdrv/USB-IDs-for-free.txt
const VID: u16 = 0x16c0;

/// USB PID for a generic keyboard from
/// https://github.com/obdev/v-usb/blob/master/usbdrv/USB-IDs-for-free.txt
const PID: u16 = 0x27db;

```
