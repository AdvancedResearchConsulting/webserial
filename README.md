# Web Serial Terminal

A single-page serial terminal that runs in the browser. List, connect to, send to and receive from the serial ports on your computer (USB-to-serial adapters, Arduino/ESP32/Pico boards, built-in COM ports, Bluetooth serial) on Windows and macOS, with nothing to install.

It's one file, [index.html](index.html), built on the browser's [Web Serial API](https://developer.mozilla.org/docs/Web/API/Web_Serial_API).

## Requirements

- **Google Chrome, Microsoft Edge or Opera** on Windows, macOS, Linux or ChromeOS. Firefox and Safari don't support Web Serial.
- The page must be opened from a local file, `http://localhost`, or `https://`. Web Serial is disabled on plain `http://` from other hosts.
- A driver for your adapter, if the OS doesn't include one (some CH340 and CP210x adapters on older Windows and macOS versions).

## Running it

Open `index.html` in Chrome or Edge (double-click it, or drag it into a browser window).

To serve it on localhost instead:

```bash
python3 -m http.server 8417
```

Then open http://localhost:8417. On Windows, use `py -m http.server 8417`.

## Using it

1. **Add port…** opens the browser's port picker, which lists every serial port on the computer (for example `COM3` on Windows or `cu.usbserial-1410` on macOS). Pick one to allow this page to use it.
2. Allowed ports appear in the **Ports** list. **Refresh** re-checks which of them are plugged in; the list also updates on its own when an allowed device is plugged in or removed. **Forget** removes the page's access to a port.
3. Choose the baud rate, data bits, parity, stop bits and flow control, then click **Connect** (or double-click the port).
4. Received data appears in the terminal. Switch between **Text** and **Hex**, and turn on **Timestamps** to see when each line arrived.
5. Type in the box at the bottom and press **Enter** to send. In **Text** mode the chosen line ending (LF, CR, CR+LF or none) is added. In **Hex** mode you type bytes like `48 65 6C 6C 6F 0D 0A` (also accepts `0x48,0x65` or `48656C`) and exactly those bytes are sent. Use ↑/↓ to recall earlier messages.
6. **DTR** and **RTS** toggle the control lines. Many dev boards reset when DTR changes.
7. **Save log** downloads the terminal contents as a text file.

Settings and send history are remembered in the browser between visits.

## Limitations

- For privacy, browsers only reveal ports the user has picked. The page can't list every port until you've added it once, and it can't see the OS name (`COM3`, `/dev/cu.*`), so ports are labeled by USB vendor and product ID instead. Chrome remembers your choices, so allowed ports show up again next time.
- A port can only be open in one program at a time. Close the Arduino IDE serial monitor, PuTTY, `screen` and other tabs before connecting.
- The terminal keeps the most recent ~2 MB of data; older output is dropped from the screen.
