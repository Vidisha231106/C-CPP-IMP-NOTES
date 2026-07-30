# IoT with Raspberry Pi & Arduino — Detailed Exam-Ready Notes
*(Mapped topic-by-topic to the syllabus, Units I–V. Every topic that has an associated program/command is covered; pure theory topics are summarized concisely.)*

---

## UNIT I — Introduction to IoT, Sensors, Design Methodology
*(No code in this unit — purely conceptual. Kept concise since no program is applicable.)*

| Topic | Key Points |
|---|---|
| **Characteristics of IoT** | Connectivity, heterogeneity, dynamic & self-adapting nature, scalability, interoperability, safety, huge scale |
| **Design Principles** | Unique identification of devices, sensing capability, energy-efficient communication, standardized protocols, security & privacy |
| **IoT Architecture** | 3-layer: Perception → Network → Application. 5-layer: Perception, Transport, Processing, Application, Business |
| **IoT Protocols** | OSI reference; Application layer — MQTT, CoAP, HTTP, XMPP, AMQP; Network/Link layer — 6LoWPAN, Zigbee, BLE, RFID |
| **Enabling Technologies** | WSN (Wireless Sensor Networks), Cloud Computing, Big Data Analytics, Embedded Systems, Communication Protocols |
| **IoT Levels** | Level 1 → single node/single app (local); ... Level 6 → multiple nodes, distributed, with coordinator + cloud |
| **IoT vs M2M** | M2M = point-to-point over telecom network, single protocol, proprietary. IoT = internet-based, multiple protocols, cloud-integrated, heterogeneous devices |
| **Sensor Classification** | Active vs Passive; Analog vs Digital; Scalar vs Vector |
| **Working Principle of Sensors** | Transduction — converting a physical quantity (light, heat, pressure) into a proportional electrical signal (voltage/current/resistance) |
| **Criteria to Choose a Sensor** | Range, accuracy, resolution, sensitivity, response time, cost, power consumption, environmental durability |
| **Generation of Sensors** | 1st Gen — basic transducers; 2nd Gen — solid-state sensors; 3rd Gen — smart sensors with signal conditioning; 4th Gen — MEMS/networked sensors with on-chip processing |
| **IoT Design Methodology** | Purpose & Requirements → Process Specification → Domain Model → Information Model → Service Specifications → IoT Level Selection → Functional View → Operational View → Device & Component Integration → Application Development |
| **Challenges in IoT Design** | Security, interoperability across vendors, power constraints, scalability, massive data handling, latency |
| **IoT System Management** | Configuration, monitoring, and fault management of deployed devices — often via a cloud dashboard |
| **IoT Servers/Platforms** | ThingSpeak, Blynk, Cayenne, Xively, Carriots, KAA, IBM IoT, Microsoft Azure IoT, ThingWorx, etc. — provide data storage, visualization, and device APIs |

---

## UNIT II — Basics of Arduino & Basics of Raspberry Pi

### PART A: Arduino

#### 2.1 Introduction to Arduino
Arduino is an open-source microcontroller platform (ATmega328P on the Uno) programmed in C/C++ through the Arduino IDE. It has digital I/O pins, analog input pins, PWM-capable pins, and a USB interface for programming/serial communication.

#### 2.2 Arduino IDE — Installation & Setup Steps
```bash
# 1. Download the Arduino IDE from arduino.cc for your OS
# 2. Install and open the IDE
# 3. Connect the Arduino board via USB
# 4. Select the board:  Tools > Board > Arduino Uno
# 5. Select the port:   Tools > Port > (COMx / /dev/ttyUSBx)
# 6. Write code in the editor, then Verify (compile) and Upload
```

#### 2.3 Basic Commands for Arduino (Reference Table)
| Command | Purpose |
|---|---|
| `pinMode(pin, mode)` | Configure a pin as `INPUT`, `OUTPUT`, or `INPUT_PULLUP` |
| `digitalWrite(pin, HIGH/LOW)` | Set a digital pin HIGH (5V) or LOW (0V) |
| `digitalRead(pin)` | Read HIGH/LOW state of a digital pin |
| `analogRead(pin)` | Read analog voltage (0–5V) as a value 0–1023 |
| `analogWrite(pin, value)` | Output PWM signal (0–255) on a PWM-capable pin |
| `delay(ms)` | Pause execution for given milliseconds |
| `millis()` | Returns time (ms) since the board started — used for non-blocking timing |
| `Serial.begin(baud)` | Initialize serial communication (e.g., 9600 bps) |
| `Serial.print()` / `Serial.println()` | Send data to Serial Monitor |

```cpp
// Demonstration sketch using several basic commands together
int ledPin = 13;
int sensorPin = A0;

void setup() {
  pinMode(ledPin, OUTPUT);      // set pin 13 as output
  Serial.begin(9600);           // start serial monitor at 9600 baud
}

void loop() {
  int sensorValue = analogRead(sensorPin);  // read analog value 0-1023
  Serial.println(sensorValue);              // print value to Serial Monitor

  if (sensorValue > 512) {
    digitalWrite(ledPin, HIGH); // turn LED on if value crosses threshold
  } else {
    digitalWrite(ledPin, LOW);
  }
  delay(500);
}
```

#### 2.3b LCD Commands (Reference Table)
| Command | Purpose |
|---|---|
| `lcd.begin(cols, rows)` | Initialize the LCD geometry, e.g. `lcd.begin(16, 2)` for a 16×2 display |
| `lcd.print("text")` | Print a string starting at the current cursor position |
| `lcd.setCursor(col, row)` | Move the cursor to a given column/row before the next `print()` |
| `lcd.clear()` | Erase all characters and return the cursor to (0,0) |
| `lcd.home()` | Return cursor to (0,0) without clearing the display |

#### 2.3c Serial Communication Commands (Reference Table)
| Command | Purpose |
|---|---|
| `Serial.begin(baud)` | Start serial communication at a given baud rate (e.g. 9600) |
| `Serial.print(data)` | Send data to the Serial Monitor without a line break |
| `Serial.println(data)` | Send data to the Serial Monitor followed by a line break |
| `Serial.write(byte)` | Send a single raw byte over the Tx line |
| `Serial.read()` | Read one incoming byte from the Rx line |
| `Serial.available()` | Returns the number of bytes waiting to be read from the buffer |

#### 2.4 Play with LED and Arduino
**Circuit:** LED anode → 220Ω resistor → Digital Pin 13; LED cathode → GND.
**Libraries used:** None (native Arduino functions)

```cpp
// Blinks an LED connected to digital pin 13 every 1 second
int ledPin = 13;          // onboard LED pin

void setup() {
  pinMode(ledPin, OUTPUT); // configure pin as output
}

void loop() {
  digitalWrite(ledPin, HIGH); // turn LED ON
  delay(1000);                // wait 1 sec
  digitalWrite(ledPin, LOW);  // turn LED OFF
  delay(1000);                // wait 1 sec
}
```
**Explanation:** `pinMode` sets pin 13 as an output once in `setup()`. The `loop()` function repeats forever, toggling the pin HIGH/LOW with a 1-second gap, producing a visible blink.

#### 2.5 Play with LCD with Arduino
**Circuit:** 16x2 LCD in 4-bit mode — RS→pin7, EN→pin8, D4-D7→pins 9,10,11,12; VSS/RW→GND; VDD→5V; contrast pin (V0) via 10kΩ potentiometer.
**Libraries used:** `LiquidCrystal.h` (bundled with Arduino IDE)

```cpp
// Displays static text and a live-updating counter on a 16x2 LCD
#include <LiquidCrystal.h>

// LiquidCrystal(RS, EN, D4, D5, D6, D7)
LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

void setup() {
  lcd.begin(16, 2);          // initialize 16 columns, 2 rows
  lcd.print("Hello, IoT!");  // print on row 1, column 0
}

void loop() {
  lcd.setCursor(0, 1);        // move cursor to row 2, column 0
  lcd.print("Time: ");
  lcd.print(millis() / 1000); // seconds elapsed since power-on
  lcd.print("s ");
  delay(1000);
}
```
**Explanation:** `lcd.begin()` initializes the display geometry. `setCursor(col, row)` positions where the next `print()` will write, allowing the second line to be refreshed independently of the static first line.

---

### PART B: Basics of Raspberry Pi

#### 2.6 Introduction to Raspberry Pi
A Raspberry Pi is a full single-board Linux computer (ARM CPU, GPIO header, HDMI, USB, Wi-Fi/Ethernet) capable of running Python/C programs and controlling external hardware through 40 GPIO pins.

#### 2.7 Installing NOOBS on SD Card
```bash
# 1. Format the SD card as FAT32 using SD Card Formatter
# 2. Download NOOBS (New Out Of the Box Software) from raspberrypi.org
# 3. Extract the NOOBS zip file and copy all contents to the root of the SD card
# 4. Insert SD card into Raspberry Pi, connect display/keyboard/power
# 5. On first boot, NOOBS shows an OS selection menu (choose Raspberry Pi OS)
```

#### 2.8 Installing Raspbian (Raspberry Pi OS) on SD Card
```bash
# 1. Download Raspberry Pi Imager (or the Raspbian .img file) from raspberrypi.org
# 2. Insert the SD card into your computer
# 3. Open Raspberry Pi Imager -> Choose OS -> Raspberry Pi OS
# 4. Choose Storage -> select the SD card
# 5. Click Write, wait for flashing + verification to complete
# 6. Insert the SD card into the Pi and power it on
```

#### 2.9 Terminal Commands (Reference Table)
| Command | Purpose |
|---|---|
| `ls` | List files/directories in current folder |
| `cd <dir>` | Change directory |
| `pwd` | Print current working directory |
| `mkdir <name>` | Create a new directory |
| `sudo apt-get update` | Refresh package list |
| `sudo apt-get install <pkg>` | Install a package |
| `tree -d` | Show directory tree |
| `clear` | Clear the terminal screen |
| `sudo reboot` | Restart the Raspberry Pi |
| `sudo halt` / `sudo shutdown -h now` | Shut down the Raspberry Pi |
| `startx` | Launch the desktop GUI (LXDE) from CLI |
| `ifconfig` / `ip a` | Show network interfaces & IP addresses |

#### 2.10 Installing Libraries on Raspberry Pi
```bash
# Update package lists first
sudo apt-get update
sudo apt-get upgrade -y

# Install pip (Python package manager) if not present
sudo apt-get install python3-pip -y

# Install commonly used IoT libraries
sudo pip3 install RPi.GPIO
sudo pip3 install adafruit-circuitpython-dht
sudo apt-get install python3-smbus i2c-tools -y   # for I2C
sudo apt-get install python3-spidev -y            # for SPI
```

#### 2.11 Getting the Static IP Address of Raspberry Pi
```bash
# Edit the dhcpcd configuration file
sudo nano /etc/dhcpcd.conf

# Add these lines at the end (example for Ethernet, interface eth0):
# interface eth0
# static ip_address=192.168.1.50/24
# static routers=192.168.1.1
# static domain_name_servers=192.168.1.1 8.8.8.8

# Save (Ctrl+O, Enter) and exit (Ctrl+X), then reboot
sudo reboot
```

#### 2.12 Running a Program on Raspberry Pi
There are several standard ways to run a program on the Pi:

| Method | Typical Use |
|---|---|
| Direct terminal execution | Quick testing during development |
| `rc.local` | Auto-run a script once at every boot, before other services |
| `crontab` | Schedule a script at fixed times/intervals |
| `.bashrc` | Auto-run a script every time a new terminal session opens |
| `systemd` service | Run a script as a proper background service (auto-restart on crash) |

```bash
# --- Direct terminal execution ---
nano my_program.py        # write your code, save with Ctrl+O, exit with Ctrl+X
python3 my_program.py     # execute the script
sudo python3 my_program.py  # run with root privileges (needed for some GPIO setups)
chmod +x my_program.py && ./my_program.py   # make executable and run directly

# --- Auto-run at boot using rc.local ---
sudo nano /etc/rc.local
# Add this line ABOVE the final "exit 0" line:
#   sudo python3 /home/pi/my_program.py &
# The trailing "&" is essential for scripts with an infinite loop —
# it runs the script as a background process so the Pi can finish booting.
sudo reboot   # test it
```
**Explanation:** `rc.local` runs once, automatically, right before the Pi finishes booting — ideal for IoT nodes that must start sensing/logging as soon as they power on, without a user logging in manually.

#### 2.13 Installing the Remote Desktop Server
```bash
# Standard method: xrdp (Remote Desktop Protocol server)
sudo apt-get update
sudo apt-get install -y xrdp
# Then connect from a Windows/Mac/Linux machine using any RDP client
# (e.g. Windows "Remote Desktop Connection") to <raspberry_pi_ip>

# Alternative method: VNC (built into Raspberry Pi OS)
sudo raspi-config
# Interface Options -> VNC -> Enable
# Connect using a VNC Viewer app to <raspberry_pi_ip>
```
**Explanation:** `xrdp` lets you log in to the Pi's desktop using the standard RDP protocol (the same one used by Windows Remote Desktop), which is often more convenient than VNC since most OSes already ship an RDP client.

---

## UNIT III — Programming with Raspberry Pi

#### 3.1 Installing I2C Driver on Raspberry Pi
**Tools used:** `raspi-config`, `i2c-tools`, `python3-smbus`

```bash
sudo raspi-config
# Interface Options -> I2C -> Enable -> Reboot

sudo apt-get update
sudo apt-get install -y i2c-tools python3-smbus

# Verify I2C devices connected on bus 1
i2cdetect -y 1
```
**Explanation:** Enabling I2C loads the `i2c-dev` kernel module and exposes `/dev/i2c-1`. `i2cdetect` scans all 7-bit addresses and prints a grid showing which addresses respond — used to confirm a sensor/LCD is wired correctly before writing code.

#### 3.2 Programming a Raspberry Pi (General Structure)
**Libraries used:** `RPi.GPIO`, `time`

```python
# General template followed by every GPIO-based Raspberry Pi program
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)     # choose BCM (Broadcom) pin numbering scheme
GPIO.setwarnings(False)    # suppress "channel already in use" warnings

# --- pin setup goes here (GPIO.setup) ---
# --- main try/except loop goes here ---
# --- GPIO.cleanup() in the finally/except block ---
```
**Explanation:** Every RPi GPIO program follows the same three-part structure: (1) select numbering mode — `BCM` (Broadcom SoC numbers) or `BOARD` (physical pin numbers), (2) configure each pin with `GPIO.setup()`, (3) always call `GPIO.cleanup()` on exit to release the pins.

#### 3.3 Controlling / Blinking an LED with Raspberry Pi
**Circuit:** LED anode → 330Ω resistor → GPIO18 (physical pin 12); cathode → GND (physical pin 6).
**Libraries used:** `RPi.GPIO`, `time`

```python
# Blinks an LED connected to GPIO pin 18 (BCM numbering)
import RPi.GPIO as GPIO
import time

LED_PIN = 18
GPIO.setmode(GPIO.BCM)        # use Broadcom pin numbering
GPIO.setup(LED_PIN, GPIO.OUT) # set pin as output

try:
    while True:
        GPIO.output(LED_PIN, GPIO.HIGH)  # LED ON
        time.sleep(1)
        GPIO.output(LED_PIN, GPIO.LOW)   # LED OFF
        time.sleep(1)
except KeyboardInterrupt:
    GPIO.cleanup()             # reset GPIO state on exit
```

#### 3.4 LED Blink Using a Function
```python
# Reusable function to blink an LED 'n' times with a given delay
import RPi.GPIO as GPIO
import time

LED_PIN = 18
GPIO.setmode(GPIO.BCM)
GPIO.setup(LED_PIN, GPIO.OUT)

def blink(times, delay):
    for _ in range(times):
        GPIO.output(LED_PIN, GPIO.HIGH)
        time.sleep(delay)
        GPIO.output(LED_PIN, GPIO.LOW)
        time.sleep(delay)

blink(5, 0.5)   # blink 5 times, 0.5 sec each
GPIO.cleanup()
```
**Explanation:** Wrapping the blink logic in a function makes the number of blinks and delay reusable parameters — useful later for signaling different events (e.g., 3 blinks = error, 1 blink = success).

#### 3.5 Reading the Digital Input (Push Button — Polling Method)
**Circuit:** Button between GPIO17 and 3.3V, with internal pull-down enabled (or external 10kΩ pull-down resistor to GND).

```python
# Reads state of a push button connected to GPIO 17
import RPi.GPIO as GPIO
import time

BUTTON_PIN = 17
GPIO.setmode(GPIO.BCM)
GPIO.setup(BUTTON_PIN, GPIO.IN, pull_up_down=GPIO.PUD_DOWN) # internal pull-down

try:
    while True:
        if GPIO.input(BUTTON_PIN) == GPIO.HIGH:
            print("Button Pressed")
        else:
            print("Button Released")
        time.sleep(0.2)
except KeyboardInterrupt:
    GPIO.cleanup()
```
**Explanation:** This is "polling" — the CPU continuously checks (`GPIO.input()`) the pin's state in a loop. Simple, but wastes CPU cycles and can miss very short pulses between poll intervals.

#### 3.6 Reading an Edge-Triggered Input — Pull-Down Configuration
```python
# Detects a rising edge (button press) using an interrupt instead of polling
import RPi.GPIO as GPIO
import time

BUTTON_PIN = 17
GPIO.setmode(GPIO.BCM)
GPIO.setup(BUTTON_PIN, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)

def button_callback(channel):
    print("Rising edge detected on pin", channel)

# Trigger callback on rising edge only, debounce 200ms
GPIO.add_event_detect(BUTTON_PIN, GPIO.RISING,
                      callback=button_callback, bouncetime=200)

try:
    while True:
        time.sleep(1)   # main thread idles; callback runs on interrupt
except KeyboardInterrupt:
    GPIO.cleanup()
```
**Explanation:** In pull-down configuration, the pin normally reads LOW; pressing the button connects it to 3.3V, producing a LOW→HIGH transition (**rising edge**). `add_event_detect` registers this transition at the hardware level, so the CPU is only interrupted when it actually happens — far more efficient than polling. `bouncetime` ignores further triggers for 200ms to filter mechanical switch bounce.

#### 3.7 Reading an Edge-Triggered Input — Pull-Up Configuration
```python
# Same concept but button pulled HIGH by default; pressing pulls it LOW (falling edge)
import RPi.GPIO as GPIO
import time

BUTTON_PIN = 17
GPIO.setmode(GPIO.BCM)
GPIO.setup(BUTTON_PIN, GPIO.IN, pull_up_down=GPIO.PUD_UP)

def button_callback(channel):
    print("Falling edge detected (button pressed)")

GPIO.add_event_detect(BUTTON_PIN, GPIO.FALLING,
                      callback=button_callback, bouncetime=200)

try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    GPIO.cleanup()
```
**Explanation:** With `PUD_UP`, the internal resistor holds the pin HIGH when the button is open. Pressing the button connects the pin to GND, causing a HIGH→LOW (**falling edge**) transition — the opposite polarity of the pull-down case, so `GPIO.FALLING` is used instead of `GPIO.RISING`.

#### 3.8 SPI (Serial Peripheral Interface) with Raspberry Pi
**Why needed:** The Pi's GPIO pins are purely digital — SPI is used here to talk to an **external ADC (MCP3008)** so the Pi can read analog sensors.
**Setup:**
```bash
sudo raspi-config
# Interface Options -> SPI -> Enable -> Reboot
sudo apt-get install -y python3-spidev
```
**Libraries used:** `spidev`

```python
# Reads analog value (channel 0) from an MCP3008 ADC over SPI
import spidev
import time

spi = spidev.SpiDev()
spi.open(0, 0)          # bus 0, device (CE0)
spi.max_speed_hz = 1350000

def read_channel(channel):
    # MCP3008 protocol: send start bit, single-ended bit, channel select
    adc = spi.xfer2([1, (8 + channel) << 4, 0])
    data = ((adc[1] & 3) << 8) + adc[2]   # combine bytes into 10-bit value
    return data

try:
    while True:
        value = read_channel(0)
        print("ADC Value:", value)
        time.sleep(1)
except KeyboardInterrupt:
    spi.close()
```
**Explanation:** SPI is a 4-wire full-duplex protocol (MOSI, MISO, SCLK, CS). `spi.xfer2()` simultaneously shifts out the 3 command bytes to the MCP3008 and shifts in the 3 response bytes; the 10-bit conversion result is reassembled from the last 10 bits of the 2nd and 3rd response bytes.

#### 3.9 Interfacing LCD with Raspberry Pi Using the Adafruit Library
**Circuit:** Standard 16x2 LCD wired directly to GPIO in 4-bit mode (RS, EN, D4–D7 + backlight pin), same wiring style as the Arduino LCD circuit — **no I2C backpack** used here.
**Libraries used:** `Adafruit_CharLCD` (`sudo pip3 install Adafruit_CharLCD`)

```python
# Displays text on a 16x2 LCD wired directly to GPIO, using the Adafruit library
import Adafruit_CharLCD as LCD
import time

# BCM pin numbers - adjust to match your wiring
lcd_rs = 25
lcd_en = 24
lcd_d4, lcd_d5, lcd_d6, lcd_d7 = 23, 17, 18, 22
lcd_backlight = 4

lcd = LCD.Adafruit_CharLCD(lcd_rs, lcd_en, lcd_d4, lcd_d5, lcd_d6, lcd_d7,
                           16, 2, lcd_backlight)   # 16 cols, 2 rows

lcd.clear()
lcd.message("Hello, RPi!\nAdafruit LCD")   # \n moves to the second row
time.sleep(3)
lcd.clear()
```
**Explanation:** The Adafruit library wraps the same low-level HD44780 4-bit protocol used in the Arduino `LiquidCrystal` library, but exposes it through Python calls like `.message()` and `.clear()` — it saves you from manually toggling the Enable pin and splitting bytes into nibbles.

#### 3.10 Interfacing LCD with Raspberry Pi in I2C Mode
**Why:** Wiring an LCD directly needs 6+ GPIO pins. An I2C backpack (PCF8574 chip) reduces this to just 2 wires (SDA, SCL), freeing up GPIO pins for other sensors.
**Circuit:** LCD's I2C backpack → SDA to GPIO2 (physical pin 3), SCL to GPIO3 (physical pin 5), VCC to 5V, GND to GND.
**Setup:** Enable I2C first (see §3.1), then find the backpack's address:
```bash
i2cdetect -y 1     # commonly shows 0x27 or 0x3f for PCF8574 backpacks
sudo pip3 install RPLCD    # a clean, actively-maintained I2C LCD library
```
**Libraries used:** `RPLCD.i2c`

```python
# Displays text on an I2C 16x2 LCD (PCF8574 backpack) using the RPLCD library
from RPLCD.i2c import CharLCD
import time

lcd = CharLCD(i2c_expander='PCF8574', address=0x27, port=1,
              cols=16, rows=2, charmap='A00')   # address from i2cdetect

lcd.clear()
lcd.write_string("I2C LCD Ready")
time.sleep(2)
lcd.cursor_pos = (1, 0)          # move to row 2, column 0
lcd.write_string("Hello, IoT!")
```
**Explanation:** `RPLCD` talks to the PCF8574 expander chip over the I2C bus (using `smbus` internally) and translates that into the same 4-bit HD44780 commands the LCD expects — so from your code's perspective it behaves just like the direct-wired version in §3.9, only over 2 wires instead of 6.

*(If you ever need to write the I2C protocol yourself instead of using a library — e.g., for a viva/practical exam — the core idea is: open the bus with `smbus.SMBus(1)`, then send each character as two 4-bit nibbles via `bus.write_byte(address, byte)`, toggling the Enable bit high-then-low between nibbles to latch the data, exactly like the LCD's parallel protocol but shifted through the I2C expander one byte at a time.)*

#### 3.11 Combined Example: PIR Motion Sensor with Raspberry Pi + I2C LCD Display
**Circuit:** PIR OUT → GPIO17 (physical pin 11); PIR VCC → 5V; PIR GND → GND. I2C LCD wired as in §3.10 (SDA→GPIO2, SCL→GPIO3).
**Libraries used:** `RPi.GPIO`, `RPLCD.i2c`, `time`

```python
# Reads a PIR motion sensor on the Raspberry Pi and shows live status on an I2C LCD
import RPi.GPIO as GPIO
from RPLCD.i2c import CharLCD
import time

PIR_PIN = 17
GPIO.setmode(GPIO.BCM)
GPIO.setup(PIR_PIN, GPIO.IN)

lcd = CharLCD(i2c_expander='PCF8574', address=0x27, port=1,
              cols=16, rows=2, charmap='A00')

try:
    while True:
        if GPIO.input(PIR_PIN):          # HIGH = motion detected
            lcd.clear()
            lcd.write_string("Motion Detected!")
            print("Motion Detected")
        else:
            lcd.clear()
            lcd.write_string("No Motion")
        time.sleep(1)
except KeyboardInterrupt:
    lcd.clear()
    GPIO.cleanup()
```
**Explanation:** This merges the two previous recipes — `GPIO.input()` polls the PIR sensor exactly as in §3.5 (digital input), and instead of just printing to the console, the result is pushed to the I2C LCD from §3.10 using `write_string()`. This same pattern (read sensor → clear LCD → write new status) is how you'd add a live display to *any* of the digital/analog sensor recipes in this document.

> **Note on "Arduino" version:** The equivalent PIR sketch for the **Arduino** board (with an LED status indicator instead of an LCD) is already covered in **§4.2**. If your practical specifically requires an Arduino driving an I2C LCD, the logic is identical to the RPi version above — swap `RPi.GPIO`/`RPLCD` calls for `digitalRead()`/`LiquidCrystal` (or `LiquidCrystal_I2C.h` for an I2C-backpack LCD on Arduino):
```cpp
// PIR + I2C LCD on Arduino (using LiquidCrystal_I2C library, address 0x27)
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);   // I2C address, columns, rows
int pirPin = 2;

void setup() {
  pinMode(pirPin, INPUT);
  lcd.init();
  lcd.backlight();
}

void loop() {
  if (digitalRead(pirPin) == HIGH) {
    lcd.clear();
    lcd.print("Motion Detected!");
  } else {
    lcd.clear();
    lcd.print("No Motion");
  }
  delay(500);
}
```

---

## UNIT IV — Interfacing Raspberry Pi and Arduino

#### 4.1 Installing Arduino IDE on Raspberry Pi
```bash
sudo apt-get update
sudo apt-get install -y arduino     # installs Arduino IDE from the Raspbian repo
arduino                             # launches the IDE (also in the Start Menu -> Programming)
```
**Explanation:** Installing the IDE directly on the Pi lets you write and upload Arduino sketches from the Pi itself (via USB to the Arduino board), instead of needing a separate laptop — useful when the Pi and Arduino are combined in a single embedded IoT node.

#### 4.2 Play with Digital Sensor — PIR Motion Sensor (Arduino Sketch)
**Circuit:** PIR OUT → Digital pin 2; VCC → 5V; GND → GND.
**Libraries used:** None

```cpp
// Detects motion using a PIR sensor and turns on an LED when motion is sensed
int pirPin = 2;     // PIR OUT pin
int ledPin = 13;

void setup() {
  pinMode(pirPin, INPUT);
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int motion = digitalRead(pirPin);
  if (motion == HIGH) {
    digitalWrite(ledPin, HIGH);
    Serial.println("Motion Detected!");
  } else {
    digitalWrite(ledPin, LOW);
  }
  delay(200);
}
```
**Explanation:** A PIR sensor outputs a simple digital HIGH when it detects a change in infrared radiation (motion) within its field of view, and LOW otherwise — so it's read exactly like a button with `digitalRead()`.

#### 4.2b Extended Version — PIR Sensor with LCD Status + Dual LED Indicator (Arduino Sketch)
**Circuit:** PIR OUT → digital pin 2; 16x2 (or 20x4) LCD wired as in §2.5 (RS→7, EN→8, D4-D7→9,10,11,12); one "motion" LED → pin 5 (through 330Ω resistor); one "idle" LED → pin 6 (through 330Ω resistor); both LED cathodes → GND.
**Libraries used:** `LiquidCrystal.h`

```cpp
// PIR motion sensor with an LCD status message and two indicator LEDs:
// motionLED lights when motion is detected, idleLED lights otherwise.
#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

int pirPin = 2;
int motionLED = 5;
int idleLED = 6;

void setup() {
  pinMode(pirPin, INPUT);
  pinMode(motionLED, OUTPUT);
  pinMode(idleLED, OUTPUT);

  lcd.begin(16, 2);
  lcd.print("PIR Motion System");
  delay(1000);
  lcd.clear();
}

void loop() {
  int motion = digitalRead(pirPin);
  lcd.setCursor(0, 0);

  if (motion == HIGH) {
    lcd.print("Motion Detected ");
    digitalWrite(motionLED, HIGH);
    digitalWrite(idleLED, LOW);
  } else {
    lcd.print("No Motion       ");
    digitalWrite(motionLED, LOW);
    digitalWrite(idleLED, HIGH);
  }
  delay(200);
}
```
**Explanation:** This combines three ideas already covered separately — digital input (§4.2), the LCD from §2.5 — into one program: the same `digitalRead()` check now drives both the LCD text and which of the two LEDs is lit, giving a visual and textual status at the same time. Padding the printed strings with trailing spaces (`"No Motion       "`) prevents leftover characters from the longer message from staying on screen when it switches back.

#### 4.3 Play with Analog Sensor — LDR / Potentiometer (Arduino Sketch)
**Circuit:** LDR forms a voltage divider with a 10kΩ resistor; midpoint → analog pin A0.
**Libraries used:** None

```cpp
// Reads an analog sensor (e.g., LDR) on A0 and prints value to Serial Monitor
int sensorPin = A0;
int sensorValue = 0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  sensorValue = analogRead(sensorPin);   // 0-1023
  Serial.print("Analog Value: ");
  Serial.println(sensorValue);
  delay(500);
}
```
**Explanation:** Arduino's ADC converts the analog voltage (0–5V) on A0 into a 10-bit digital number (0–1023), giving finer resolution than a digital sensor — useful for measuring light intensity, temperature, or any continuously varying quantity.

#### 4.3b Extended Version — LDR Sensor with LCD Display + Threshold LED (Arduino Sketch)
**Circuit:** LDR voltage-divider output → A0 (as in §4.3); LCD wired as in §2.5; one LED → pin 5 (through 330Ω resistor), cathode → GND.
**Libraries used:** `LiquidCrystal.h`

```cpp
// Reads an LDR, shows the live ADC value on an LCD, and lights an LED
// once the light level crosses a set threshold.
#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

int ldrPin = A0;
int ledPin = 5;
int threshold = 500;   // adjust based on your LDR/room lighting

void setup() {
  pinMode(ledPin, OUTPUT);
  lcd.begin(16, 2);
  lcd.print("Light Monitoring");
  delay(1000);
  lcd.clear();
}

void loop() {
  int ldrValue = analogRead(ldrPin);

  lcd.setCursor(0, 0);
  lcd.print("ADC Value: ");
  lcd.print(ldrValue);
  lcd.print("   ");   // clears leftover digits from a previous, longer number

  if (ldrValue >= threshold) {
    digitalWrite(ledPin, HIGH);
    lcd.setCursor(0, 1);
    lcd.print("Status: BRIGHT ");
  } else {
    digitalWrite(ledPin, LOW);
    lcd.setCursor(0, 1);
    lcd.print("Status: DARK   ");
  }
  delay(200);
}
```
**Explanation:** Same ADC read as §4.3, but now the raw value is printed to the LCD every loop, and a simple `if (ldrValue >= threshold)` comparison turns the extra sensor reading into a decision — this threshold pattern (read → compare → act) is the same one used for the air-quality and landslide examples in Unit V, just applied to light level here.

#### 4.4 Play with Actuators — Servo Motor (Arduino Sketch)
**Circuit:** Servo signal wire → digital pin 9 (PWM); VCC → 5V (external supply recommended for larger servos); GND → common GND.
**Libraries used:** `Servo.h` (built into Arduino IDE)

```cpp
// Sweeps a servo motor from 0 to 180 degrees and back
#include <Servo.h>

Servo myServo;

void setup() {
  myServo.attach(9);   // servo signal pin connected to D9
}

void loop() {
  for (int pos = 0; pos <= 180; pos++) {
    myServo.write(pos);
    delay(15);
  }
  for (int pos = 180; pos >= 0; pos--) {
    myServo.write(pos);
    delay(15);
  }
}
```
**Explanation:** `myServo.write(angle)` internally generates a PWM pulse (roughly 1–2ms, repeated every 20ms) whose width corresponds to the requested angle (0°–180°); the servo's internal circuitry converts this pulse width into a proportional shaft position.

---

## UNIT V — Connecting to the Cloud (ThingSpeak & Automation)

> The textbook implements these projects on the **Raspberry Pi in Python** (sensor libraries + an HTTP request to ThingSpeak), not on an Arduino/ESP board — so all Unit V code below has been rewritten to match that: Python 3, `RPi.GPIO`/`spidev` for the sensors, and the `requests` library for the cloud upload. Replace the API key placeholder with your own channel's Write API Key before running.

#### 5.1 Smart IoT Systems — Concept
A smart IoT system = **Sensor → Raspberry Pi (Python) → Wi-Fi/Ethernet → Cloud Platform (ThingSpeak) → Dashboard/Alert**. Data is periodically POSTed to a REST endpoint using the channel's Write API Key, then visualized as live charts on ThingSpeak.

#### 5.2 DHT11 Data Logger with ThingSpeak Server
**Circuit:** DHT11 VCC → 5V; GND → GND; OUT → GPIO4 (physical pin 7).
**Steps to create a ThingSpeak channel:**
```text
1. Sign up / log in at thingspeak.com
2. Click "New Channel" -> name it, enable Field1 (Temperature) & Field2 (Humidity)
3. Save channel -> go to "API Keys" tab -> copy the Write API Key
```


**Software Setup (Adafruit Library):**
To interface the DHT11 sensor with a Raspberry Pi, install the Adafruit DHT library using the following commands:

```bash
sudo apt-get update
sudo apt-get install build-essential python-dev
git clone https://github.com/adafruit/Adafruit_Python_DHT.git
cd Adafruit_Python_DHT
sudo python setup.py install
```

**Libraries used:** `Adafruit_DHT` (`sudo pip3 install Adafruit_DHT`), `requests` (`sudo pip3 install requests`)

```python
# Reads temperature & humidity from a DHT11 sensor on the Raspberry Pi
# and uploads both readings to a ThingSpeak channel.
import Adafruit_DHT
import requests
import time

SENSOR = Adafruit_DHT.DHT11
DHT_PIN = 4
WRITE_API_KEY = "YOUR_THINGSPEAK_WRITE_API_KEY"
URL = "https://api.thingspeak.com/update"

while True:
    humidity, temperature = Adafruit_DHT.read_retry(SENSOR, DHT_PIN)

    if humidity is not None and temperature is not None:
        payload = {
            "api_key": WRITE_API_KEY,
            "field1": temperature,
            "field2": humidity
        }
        response = requests.get(URL, params=payload)
        print("Uploaded - Temp:", temperature, "Humidity:", humidity,
              "| Status:", response.status_code)
    else:
        print("Failed to read from DHT11 sensor")

    time.sleep(20)   # ThingSpeak free tier needs >=15s between updates
```
**Explanation:** `Adafruit_DHT.read_retry()` handles the DHT11's finicky single-wire timing protocol for you, retrying automatically if a read glitches. `requests.get()` with a `params` dict does the URL-encoding of the field values automatically — no manual string building needed, unlike a raw socket/HTTP approach.

#### 5.3 Ultrasonic Sensor Data Logger with ThingSpeak Server
**Circuit:** HC-SR04 VCC → 5V; GND → GND; Trig → GPIO23; Echo → GPIO24 (through a voltage divider, since Echo outputs 5V and the Pi's GPIO is 3.3V-tolerant only).
**Libraries used:** `RPi.GPIO`, `requests`

```python
# Measures distance with an HC-SR04 ultrasonic sensor on the Raspberry Pi
# and uploads it to ThingSpeak.
import RPi.GPIO as GPIO
import requests
import time

TRIG = 23
ECHO = 24
WRITE_API_KEY = "YOUR_THINGSPEAK_WRITE_API_KEY"
URL = "https://api.thingspeak.com/update"

GPIO.setmode(GPIO.BCM)
GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)
GPIO.output(TRIG, False)
time.sleep(1)   # let the sensor settle

def get_distance():
    GPIO.output(TRIG, True)
    time.sleep(0.00001)          # 10 microsecond trigger pulse
    GPIO.output(TRIG, False)

    while GPIO.input(ECHO) == 0:
        start_time = time.time()
    while GPIO.input(ECHO) == 1:
        end_time = time.time()

    duration = end_time - start_time
    return (duration * 34300) / 2   # speed of sound (cm/s), round trip

try:
    while True:
        distance_cm = get_distance()
        payload = {"api_key": WRITE_API_KEY, "field1": round(distance_cm, 1)}
        response = requests.get(URL, params=payload)
        print("Distance:", round(distance_cm, 1), "cm | Status:", response.status_code)
        time.sleep(20)
except KeyboardInterrupt:
    GPIO.cleanup()
```
**Explanation:** Same HC-SR04 principle as the theory (a 10µs trigger pulse fires an ultrasonic burst, then Echo stays HIGH for the round-trip time), but timed here with Python's `time.time()` around the two `while` loops that wait for Echo to go HIGH and then LOW again, instead of Arduino's built-in `pulseIn()`.

#### 5.4 Air Quality Monitoring System and Data Logger with ThingSpeak Server
**System components (per the textbook):** Raspberry Pi, an MQ2 (smoke) sensor, an MQ7 (CO) sensor, a DHT11 (temperature/humidity), a Pi Camera, a GP2Y1030 dust sampler, an Arduino Uno, and a power supply.
**Why the Arduino Uno is needed:** the Pi has no analog input pins, so the three analog sensors (MQ2, MQ7, GP2Y1030) are wired to the Arduino's analog pins, and the Pi reads them over USB using the **Firmata protocol** (`pyfirmata`) instead of a direct GPIO connection.

**Circuit:**
- MQ2 analog out → Arduino **A0**; MQ7 analog out → Arduino **A1**
- GP2Y1030 analog out → Arduino **A2**; GP2Y1030 LED-control pin → Arduino **D8**
- DHT11 OUT → Raspberry Pi **GPIO4**
- Arduino connected to the Pi via USB

**Libraries used:** `pyfirmata` (`sudo pip3 install pyfirmata`), `Adafruit_DHT`, `picamera`, `requests`

```python
# Air Quality Monitoring System
# MQ2/MQ7/dust sensor readings come from the Arduino Uno via Firmata over USB;
# DHT11 and the camera are handled directly by the Raspberry Pi.
import pyfirmata
import Adafruit_DHT
import requests
import time
from picamera import PiCamera

# --- Arduino Uno over USB (handles the analog sensors) ---
board = pyfirmata.Arduino('/dev/ttyUSB0')
it = pyfirmata.util.Iterator(board)
it.start()

mq2_pin = board.get_pin('a:0:i')       # MQ2 smoke sensor
mq7_pin = board.get_pin('a:1:i')       # MQ7 CO sensor
dust_pin = board.get_pin('a:2:i')      # GP2Y1030 analog output
dust_led_pin = board.get_pin('d:8:o')  # GP2Y1030 internal LED control

mq2_pin.enable_reporting()
mq7_pin.enable_reporting()
dust_pin.enable_reporting()

# --- Raspberry Pi peripherals ---
DHT_PIN = 4
camera = PiCamera()
WRITE_API_KEY = "YOUR_THINGSPEAK_WRITE_API_KEY"
URL = "https://api.thingspeak.com/update"

SMOKE_THRESHOLD = 0.5
CO_THRESHOLD = 0.5
DUST_THRESHOLD = 0.15   # mg/m3 - roughly where this sensor is considered "unhealthy"

def read_dust_density():
    # GP2Y1030 datasheet timing: briefly pulse the internal LED, then sample
    dust_led_pin.write(0)          # LED ON (active LOW on this module)
    time.sleep(0.00028)            # sampling time
    voltage = (dust_pin.read() or 0) * 5.0
    time.sleep(0.00004)            # delta time before turning LED off
    dust_led_pin.write(1)          # LED OFF
    time.sleep(0.00968)            # completes the ~10ms measurement cycle
    density = 0.17 * voltage - 0.1   # linear approximation from the datasheet curve
    return max(density, 0.0)

image_count = 0

try:
    while True:
        humidity, temperature = Adafruit_DHT.read_retry(Adafruit_DHT.DHT11, DHT_PIN)
        smoke_level = mq2_pin.read() or 0
        co_level = mq7_pin.read() or 0
        dust_density = read_dust_density()

        payload = {
            "api_key": WRITE_API_KEY,
            "field1": temperature,
            "field2": humidity,
            "field3": round(smoke_level, 3),
            "field4": round(co_level, 3),
            "field5": round(dust_density, 3)
        }
        response = requests.get(URL, params=payload)
        print("Temp:", temperature, "Hum:", humidity, "Smoke:", smoke_level,
              "CO:", co_level, "Dust:", round(dust_density, 3),
              "| Status:", response.status_code)

        # Capture a photo whenever any pollutant crosses its alert threshold
        if smoke_level > SMOKE_THRESHOLD or co_level > CO_THRESHOLD or dust_density > DUST_THRESHOLD:
            image_count += 1
            camera.capture('/home/pi/air_quality_%d.jpg' % image_count)
            print("Threshold exceeded - photo captured")

        time.sleep(20)
except KeyboardInterrupt:
    board.exit()
```
**Explanation:** `pyfirmata` uploads a small "StandardFirmata" sketch to the Arduino once, then lets Python read/write its pins directly over the USB serial link — so `mq2_pin.read()` behaves just like `GPIO.input()` would, except the pin physically lives on the Arduino. The GP2Y1030 needs a precise LED-pulse-then-sample timing sequence (per its datasheet) to get an accurate reading, which is why `read_dust_density()` has three separate `time.sleep()` calls instead of a single `read()`.

#### 5.5 Landslide Detection and Disaster Management System
**System components (per the textbook):** Raspberry Pi, a Pi Camera, a PIR (motion) sensor, a BMP180 (pressure) sensor, a DHT11 (temperature/humidity), a rain sensor, an accelerometer (tilt detector), a buzzer, an Arduino Uno, and a power supply.
**Role split:** PIR, BMP180, DHT11, the accelerometer, the buzzer, and the camera are wired directly to the Raspberry Pi (I2C for BMP180/accelerometer, GPIO for the rest). The rain sensor is wired to the **Arduino Uno** and read over USB via Firmata, the same pattern as §5.4.

**Circuit:**
- PIR OUT → Pi **GPIO27**; Buzzer → Pi **GPIO22**; DHT11 OUT → Pi **GPIO4**
- BMP180 and accelerometer (e.g. ADXL345) → Pi **I2C bus** (SDA/SCL, as in §3.1)
- Rain sensor digital output → Arduino **D7**; Arduino connected to the Pi via USB

**Libraries used:** `RPi.GPIO`, `Adafruit_DHT`, `Adafruit-BMP` (`sudo pip3 install Adafruit-BMP`), `adxl345` (`sudo pip3 install adxl345`), `pyfirmata`, `picamera`, `requests`

```python
# Landslide Detection and Disaster Management System
# Combines readings from six sensors across the Pi (I2C + GPIO) and an
# Arduino Uno (rain sensor via Firmata) to flag landslide risk.
import RPi.GPIO as GPIO
import Adafruit_DHT
from Adafruit_BMP.BMP085 import BMP085
from adxl345 import ADXL345
import pyfirmata
import requests
import time
from picamera import PiCamera

# --- Raspberry Pi GPIO peripherals ---
PIR_PIN = 27
BUZZER_PIN = 22
DHT_PIN = 4

GPIO.setmode(GPIO.BCM)
GPIO.setup(PIR_PIN, GPIO.IN)
GPIO.setup(BUZZER_PIN, GPIO.OUT)

# --- I2C sensors ---
pressure_sensor = BMP085(0x77)   # BMP180 shares the BMP085 driver/address
tilt_sensor = ADXL345()

# --- Arduino Uno over USB (handles the rain sensor) ---
board = pyfirmata.Arduino('/dev/ttyUSB0')
it = pyfirmata.util.Iterator(board)
it.start()
rain_pin = board.get_pin('d:7:i')
rain_pin.enable_reporting()

camera = PiCamera()
WRITE_API_KEY = "YOUR_THINGSPEAK_WRITE_API_KEY"
URL = "https://api.thingspeak.com/update"

TILT_THRESHOLD = 3.0   # deviation (m/s^2) from resting gravity that counts as "tilted"
image_count = 0

try:
    while True:
        humidity, temperature = Adafruit_DHT.read_retry(Adafruit_DHT.DHT11, DHT_PIN)
        motion = GPIO.input(PIR_PIN)
        pressure_hpa = pressure_sensor.read_pressure() / 100.0
        altitude_m = pressure_sensor.read_altitude()
        rain_detected = rain_pin.read()   # 1 = rain sensor triggered (wet)

        axes = tilt_sensor.get_axes()
        tilt_magnitude = abs(axes['x']) + abs(axes['y']) + abs(axes['z'] - 9.8)

        risk_alert = (tilt_magnitude > TILT_THRESHOLD and rain_detected == 1)
        GPIO.output(BUZZER_PIN, GPIO.HIGH if risk_alert else GPIO.LOW)

        payload = {
            "api_key": WRITE_API_KEY,
            "field1": temperature,
            "field2": humidity,
            "field3": motion,
            "field4": round(pressure_hpa, 1),
            "field5": round(altitude_m, 1),
            "field6": rain_detected,
            "field7": round(tilt_magnitude, 2),
            "field8": int(risk_alert)
        }
        response = requests.get(URL, params=payload)
        print("Risk alert:", risk_alert, "| Status:", response.status_code)

        if motion == 1:   # capture a photo whenever the PIR also senses movement
            image_count += 1
            camera.capture('/home/pi/landslide_%d.jpg' % image_count)

        time.sleep(20)
except KeyboardInterrupt:
    GPIO.output(BUZZER_PIN, GPIO.LOW)
    GPIO.cleanup()
    board.exit()
```
**Explanation:** This is a broader **sensor-fusion** example than §5.5's earlier two-sensor version — it now folds in all six inputs the textbook specifies. The core decision logic is still a simple threshold rule (`tilt_magnitude > TILT_THRESHOLD and rain_detected == 1`): rainfall alone is common and not dangerous, and small vibrations/tilts alone can be normal ground settling, but **both together** are treated as the landslide-risk condition that sounds the buzzer and gets logged with `field8=1` for a ThingSpeak alert view.

#### 5.6 Smart Motion Detector and Upload Image to Gmail.com
**System components (per the textbook):** Raspberry Pi, a Pi Camera, a PIR sensor, an LED (indicator), and a power supply.

**Step 1 — Configuring Raspberry Pi with Camera and Gmail:**
```bash
# Enable the camera interface
sudo raspi-config
# Interface Options -> Camera -> Enable -> Reboot

# Install camera library
sudo apt-get install -y python3-picamera

# Generate a Gmail "App Password" (Google Account -> Security -> App Passwords)
# — required because Gmail blocks plain-password SMTP logins from scripts
```
**Circuit:** PIR OUT → GPIO17; indicator LED anode → 330Ω resistor → GPIO27, cathode → GND.
**Libraries used:** `RPi.GPIO`, `picamera` (or `picamera2`), `smtplib`, `email` (all available on Raspberry Pi OS)

```python
# Detects motion via PIR sensor on Raspberry Pi, lights an indicator LED,
# captures an image, and emails it as an attachment using Gmail's SMTP server.
import RPi.GPIO as GPIO
import time
from picamera import PiCamera
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email import encoders

PIR_PIN = 17
LED_PIN = 27      # motion indicator LED
GPIO.setmode(GPIO.BCM)
GPIO.setup(PIR_PIN, GPIO.IN)
GPIO.setup(LED_PIN, GPIO.OUT)
camera = PiCamera()

SENDER_EMAIL = "your_email@gmail.com"
APP_PASSWORD = "your_gmail_app_password"   # use an App Password, not your login password
RECEIVER_EMAIL = "receiver_email@gmail.com"

def send_email(image_path):
    msg = MIMEMultipart()
    msg['Subject'] = 'Motion Detected!'
    msg['From'] = SENDER_EMAIL
    msg['To'] = RECEIVER_EMAIL

    with open(image_path, 'rb') as f:
        img = MIMEBase('application', 'octet-stream')
        img.set_payload(f.read())
    encoders.encode_base64(img)
    img.add_header('Content-Disposition', 'attachment', filename='motion.jpg')
    msg.attach(img)

    with smtplib.SMTP('smtp.gmail.com', 587) as server:
        server.starttls()                       # secure the connection (TLS)
        server.login(SENDER_EMAIL, APP_PASSWORD)
        server.send_message(msg)

try:
    while True:
        if GPIO.input(PIR_PIN):
            GPIO.output(LED_PIN, GPIO.HIGH)      # turn on the motion indicator LED
            print("Motion detected, capturing image...")
            camera.capture('/home/pi/motion.jpg')
            send_email('/home/pi/motion.jpg')
            time.sleep(10)                       # cool-down before next trigger
        else:
            GPIO.output(LED_PIN, GPIO.LOW)
        time.sleep(0.5)
except KeyboardInterrupt:
    GPIO.cleanup()
```
**Explanation:** The PIR sensor is polled just like in the Unit IV digital-sensor example. A HIGH reading now does three things: lights the indicator LED (`GPIO.output(LED_PIN, GPIO.HIGH)`) so someone standing nearby can visually confirm the system triggered, grabs a JPEG frame with `camera.capture()`, and emails it (base64-encoded, as required by MIME) over an authenticated, TLS-encrypted (`starttls()`) SMTP session to Gmail's mail server on port 587. The LED turns back off as soon as the PIR reading goes back LOW.

---

## Quick Reference — Libraries & Tools by Platform

| Platform | Library / Tool | Purpose |
|---|---|---|
| Arduino | `LiquidCrystal.h` | 16x2 LCD display |
| Arduino | `Servo.h` | Servo motor control |
| Arduino | `LiquidCrystal_I2C.h` | I2C-backpack LCD display |
| Raspberry Pi | `RPi.GPIO` | Digital I/O, interrupts (edge detection) |
| Raspberry Pi | `spidev` | SPI communication (e.g., MCP3008 ADC) |
| Raspberry Pi | `smbus`, `i2c-tools` | I2C communication & bus scanning |
| Raspberry Pi | `Adafruit_CharLCD` | Direct-GPIO character LCD control |
| Raspberry Pi | `RPLCD` | I2C-backpack character LCD control |
| Raspberry Pi | `Adafruit_DHT` | DHT11/DHT22 temperature-humidity |
| Raspberry Pi | `Adafruit-BMP` | BMP180/BMP085 pressure & altitude |
| Raspberry Pi | `adxl345` | ADXL345 accelerometer (tilt detection) |
| Raspberry Pi + Arduino | `pyfirmata` | Read/write Arduino pins from Python over USB (Firmata protocol) |
| Raspberry Pi | `requests` | HTTP GET/POST to ThingSpeak (Unit V) |
| Raspberry Pi | `picamera` / `picamera2` | Camera capture (image/video) |
| Raspberry Pi | `smtplib`, `email` | Sending email (Gmail SMTP) |
| Raspberry Pi | `raspi-config` | Enabling I2C/SPI/Camera/VNC interfaces |

**Note:** All code above independently demonstrates the exact concepts named in your syllabus (not copied verbatim from any textbook) — pin numbers, thresholds, and credentials are placeholders to match to your own hardware/accounts.
