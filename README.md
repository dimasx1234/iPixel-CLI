# iPixel CLI

Various reverse engineering tools and rewrite of the "iPixel Color" application in CLI Python for LED Matrix displays manufactured by Taizhou Yumo Lighting Co., Ltd.
Tested only on a 96x16 display, but should work on other sizes (if not please open an issue).

⚠️ Still experimental, some commands might change in the future. ⚠️

## Features

- [x] 🔗 Connect to the device
- [x] 🔆 Set brightness
- [x] 🟥 Set pixels on screen
- [x] ⏲️ Set clock mode
- [x] 🎉 Set the display mode to fun mode (do not save display)
- [x] ✏️ Write text on the screen (with custom font support)
- [x] 💥 Clear memory
- [x] 🎢 Send animations
- [x] 🖼️ Write frames to EEPROM
- [x] ✅ Maintain connection to the device (WebSocket server)
- [x] 🔧 Change orientation
- [x] 🔧 Set the date
- [X] 🔧 Set the clock

- [ ] 🔈 Audio mode
- [ ] 🔒 Set password

## Installation

Clone the repository and install the required packages.

```bash
git clone https://github.com/lucagoc/LED-Matrix-BLE-Tools
cd LED-Matrix-BLE-Tools
```

Then use the package manager [pip](https://pip.pypa.io/en/stable/) to install the required packages.

```bash
pip install -r requirements.txt
```

## Usage

### ⚠️ WARNING ⚠️

Invalid data sent to the device can lead to BOOTLOOPS

While it is possible to recover from a bootloop by sending a clear command before the device tries to read the EEPROM, it is a little bit tricky and the timing is very short.
This tool is still experimental, use at your own risk.

Commands are executed using the following format:

```bash
python ipixelcli.py -a <DEVICE_MAC_ADDRESS> -c <COMMAND> [PARAMETERS]
```

Example:

```bash
python ipixelcli.py -a 4B:1E:2E:35:73:A3 -c send_text "Hello World" rainbow_mode=5 speed=50
```

---

## Commands

### `set_clock_mode`

**Description:** Set the clock mode of the device.

**Parameters:**

- `style` (int): Clock style (0-8). Default: 1
- `date` (str): Date in `DD/MM/YYYY` format. Default: current date
- `show_date` (bool): Show date (true/false). Default: true
- `format_24` (bool): 24-hour format (true/false). Default: true

---

### `set_time`

**Description:** Set the time of the device.

**Parameters:**

- `hour` (int): Hour (0-23). Default: current hour
- `minute` (int): Minute (0-59). Default: current minute
- `second` (int): Second (0-59). Default: current second

If one or more parameter is missing, the current time will be used.

---

### `set_fun_mode`

**Description:** Set the DIY Fun Mode (Drawing Mode).

**Parameters:**

- `value` (bool): Enable or disable the mode (true/false). Default: false

---

### `set_orientation`

**Description:** Set the orientation of the device.

**Parameters:**

- `orientation` (int): Orientation value (0-2). Default: 0

---

### `clear`

**Description:** Clear the EEPROM.

**Parameters:**
None

---

### `set_brightness`

**Description:** Set the brightness of the device.

**Parameters:**

- `value` (int): Brightness level (0-100).

---

### `set_pixel`

**Description:** Set the color of a specific pixel. (EXPERIMENTAL)

**Parameters:**

- `x` (int): X-coordinate of the pixel.
- `y` (int): Y-coordinate of the pixel.
- `color` (str): Hexadecimal color value (e.g., `ff0000` for red).

---

### `send_text`

**Description:** Send a text to the device with configurable parameters.

**Parameters:**

- `text` (str): The text to display.
- `rainbow_mode` (int): Rainbow effect mode (0-9). Default: 0
- `animation` (int): Animation style (0-7). Default: 0
- `save_slot` (int): Save slot for the text (1-10). Default: 1
- `speed` (int): Speed of the text animation (0-100). Default: 80
- `color` (str): Hexadecimal color value. Default: `ffffff`

---

### `set_screen`

**Description:** Set the screen to display an image. (EXPERIMENTAL)

**Parameters:**

- `path` (str): Path to the image file.

---

### `send_animation`

**Description:** Send a GIF animation to the device, size must be 96x16 pixels and small.

**Parameters:**

- `path_or_hex` (str): Path to the GIF file or its hexadecimal representation.

---

### `delete_screen`

**Description:** Delete a screen from the EEPROM.

**Parameters:**

- `n` (int): Index of the screen to delete.

## WebSocket server

You can also start a basic WebSocket server using the following command :

```bash
python ipixelcli.py -a <bt_address> --server -p <port>
```

Then, send a request to the server with the following content:

```json
{
    "command": "<command>",
    "params": ["<param1>", "<param2>", "<param3>"]
}
```

For example :

```json
{
    "command": "send_text",
    "params": ["Hello World", "rainbow_mode=1", "speed=50"]
}
```

## Font generation

Edit the `all.png` file in the font folder to change characters. Then run `gen_font.py` to get the trimmed characters.
