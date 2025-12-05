# SAMV71 SD Card Throughput Demo on NuttX

This project is a demonstration of SD card read/write throughput on the SAMV71 Xplained Ultra board running the NuttX RTOS. It is a port of a Microchip Harmony-based application to a NuttX-based application.

## Hardware Requirements

*   **Board:** [SAMV71 Xplained Ultra](https://www.microchip.com/en-us/development-tool/ATSAMV71-XULT)
*   **SD Card:** A microSD card.

## Software Requirements

*   A Linux environment or [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install) on Windows.
*   **ARM GCC Toolchain:** `gcc-arm-none-eabi`
*   **NuttX Build Tools:** `build-essential`, `kconfig-frontends`, etc.

## Getting Started

### 1. Set up the Build Environment

The recommended way to build NuttX is to use a Linux environment. If you are on Windows, it is highly recommended to use WSL.

1.  **Install WSL:** Follow the [official guide](https://docs.microsoft.com/en-us/windows/wsl/install) to install WSL and a Linux distribution like Ubuntu.

2.  **Install Build Tools:** Open your WSL terminal and install the necessary tools:

    ```bash
    sudo apt-get update
    sudo apt-get install \
        build-essential \
        gcc-arm-none-eabi \
        kconfig-frontends \
        bison flex gettext texinfo libncurses5-dev libncursesw5-dev \
        gperf automake libtool pkg-config
    ```

### 2. Clone the Repository

Clone this repository to your local machine:

```bash
git clone https://github.com/vivkum/samv71_nuttx_sdcard.git
cd samv71_nuttx_sdcard
```

### 3. Configure NuttX

1.  **Run the configure script:**

    ```bash
    cd nuttx
    ./tools/configure.sh samv71-xult:nsh
    ```

2.  **Customize the configuration:**

    ```bash
    make menuconfig
    ```

3.  **Enable the application and drivers:** In `menuconfig`, ensure the following are enabled:

    *   `Application Configuration` -> `Examples` -> `SD Card Throughput App`
    *   `System Type` -> `SAMV71 Peripheral Support` -> `HSMCI`
    *   `Device Drivers` -> `MMC/SD/SDIO Driver Support`
    *   `Device Drivers` -> `GPIO Driver Support`

    Save the configuration and exit `menuconfig`.

### 4. Uncomment the Code

The file `nuttx-apps/sdcard_app/sdcard_app_main.c` has been prepared with the application logic, but the NuttX-specific API calls are commented out. You will need to uncomment them.

*   Open `nuttx-apps/sdcard_app/sdcard_app_main.c`.
*   Uncomment the lines containing `mount`, `open`, `write`, `read`, `close`, `lseek`, and `clock_gettime`.
*   You may need to adjust the GPIO pin number for the LED based on your board's documentation.

### 5. Build the Firmware

Run the `make` command from the `nuttx` directory:

```bash
make
```

This will build the NuttX firmware and generate a `nuttx.bin` file in the `nuttx` directory.

### 6. Flash the Firmware

Connect the SAMV71 Xplained Ultra to your computer via the EDBG USB port. The board should appear as a mass storage device. Copy the `nuttx.bin` file to this device to flash the board.

### 7. Run the Application

1.  Connect to the board's serial console using a terminal emulator (e.g., PuTTY, Tera Term) with the following settings:
    *   **Baud Rate:** 115200
    *   **Data Bits:** 8
    *   **Parity:** None
    *   **Stop Bits:** 1

2.  Press `Enter` to get to the NuttShell (NSH) prompt (`nsh>`).

3.  Run the application:

    ```bash
    nsh> sdcard_app
    ```

The application will then mount the SD card, perform the throughput test, and print the results to the console.

## License

This project is licensed under the [Apache License 2.0](LICENSE).
