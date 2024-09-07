# miLazyCracker  
**Mifare Classic Plus - Mfoc + Hardnested + mfkey32v2 Attack Implementation for PN532+PL2303**

## Installation

### 1. Register Your Reader and UART
Follow the steps in the link below to register your reader and UART on your computer:

[How to Use PN532 on macOS with libnfc, mfoc, and mfcuk](https://shop.mtoolstec.com/how-to-use-pn532-on-macos-with-libnfc-mfoc-and-mfcuk.html)

### 2. Preparation  
Make sure you have the following:

- **PN532, PN532 BLE, or PCR532 Reader**
- **USB Cable**
- **macOS 10.12 or later**

### 3. Install the USB Serial Driver  

The PN532 and PCR532 Readers from MTools Tec use the CH340E USB-to-serial chip. To communicate with the reader, install the USB serial driver:

- Download the driver from the [CH340G USB Serial Driver URL](https://www.wch.cn/download/CH341SER_MAC_ZIP.html). This driver is also included in the reader package.
- Open the `CH34xVCPDriver` app and click the install button.
- Reboot your computer after installation.
- Test the installation by running this command in your terminal:

    ```bash
    ls /dev/tty.*
    ```

- You should see something like `/dev/tty.wchusbserial1410`. The port name may vary.

### 4. Install libnfc

1. Install [Homebrew](https://brew.sh/) if you don’t already have it.
2. Run the following commands to install and link `libnfc`:

    ```bash
    brew install libnfc
    brew link libnfc
    ```

3. Plug in your PN532 or PCR532 Reader. Your system should automatically detect the USB serial port. Run `nfc-list` to check if the reader is recognized:

    ```bash
    nfc-list
    ```

    You should see output like this:

    ```bash
    ➜  ~ nfc-list
    nfc-list uses libnfc 1.8.0
    NFC device: pn532_uart:/dev/tty.usbserial-2140 opened
    1 ISO14443A passive target(s) found:
    ISO/IEC 14443A (106 kbps) target:
        ATQA (SENS_RES): 00  04
        UID (NFCID1): 01  02  03  04
        SAK (SEL_RES): 08
    ```

### 5. Troubleshooting libnfc

If you encounter the error `Unable to open NFC device: pn532_uart:/dev/tty.wchusbserialxxxxxx`, follow these steps to modify the libnfc configuration:

1. Open the libnfc configuration file:

    ```bash
    sudo nano /usr/local/etc/nfc/libnfc.conf
    ```

2. Make the following changes:

    - Set `allow_autoscan = false`
    - Set `allow_intrusive_scan = false`
    - Update `device.name` to:

      ```bash
      device.name = "pn532_uart:/dev/tty.wchusbserialxxxxxx:pn532"
      ```

3. Save the file and exit. Run `nfc-list` again; the issue should be resolved.

### 6. Fresh Install Script

For an automated installation, use the following script:

```bash
./miLazyCrackerFreshInstall.sh
