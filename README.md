# MIDI BytePulse

A USB class-compliant MIDI interface and clock utility built around the Pro Micro (ATmega32U4, 5V) that acts as a universal MIDI clock follower/converter.

## Overview

MIDI BytePulse locks to **USB MIDI** (from DAW) or **DIN MIDI IN** (from hardware sequencers/drum machines) and generates a clean analog sync pulse output (0-5V) suitable for Volca, Teenage Engineering, and modular gear.

### Key Features

- **Dual Clock Input Sources**
  - USB MIDI from DAW
  - DIN MIDI IN from external hardware
  - Selectable clock master mode: AUTO (USB > DIN), FORCE USB, or FORCE DIN

- **Outputs**
  - Analog clock sync output (2 PPQN default, 0-5V)
  - Protected open-collector transistor driver
  - Plug-detect on 3.5mm jack (clock only when cable inserted)

- **Controllers**
  - 3 analog potentiometers (Volume, Cutoff, Resonance) sending MIDI CC over USB
  - 1 rotary encoder with pushbutton (assignable + menu navigation)
  - 3 utility buttons (Function, Play/Pause, Stop)

- **Display & Feedback**
  - 0.96" I2C OLED display (128×64, SSD1306)
  - Red beat LED (quarter-note pulse indicator)
  - Green power LED (always on when powered)

## Hardware Requirements

### Core Components

- **Pro Micro 5V** (ATmega32U4) - Main controller with USB MIDI capability
- **6N138 Optocoupler** - MIDI IN isolation
- **DIN-5 Female Connector** - MIDI IN port
- **3.5mm Mono Jack with Switch** - Sync output with cable detection
- **0.96" I2C OLED** (SSD1306, 128×64) - Status display
- **2N3904/2N2222 NPN Transistor** - Open-collector clock driver

### User Interface

- **3× 10kΩ Linear Potentiometers** - Volume (CC7), Cutoff (CC74), Resonance (CC71)
- **1× Rotary Encoder with Switch** - Menu navigation and assignable control
- **3× Momentary Pushbuttons** - Function, Play/Pause, Stop
- **1× Red LED** - Beat indicator
- **1× Green LED** - Power indicator

### Passive Components

- Resistors: 220Ω (×2), 1.8kΩ, 2.2kΩ, 10kΩ (×4), 100Ω (×3)
- Capacitors: 100nF (×5), 10µF (×1)
- Diodes: 1N4148 (×1)

## Pin Assignments

### Pro Micro Connections

**MIDI & Serial**
- Pin 0 (RX1) - MIDI IN from optocoupler
- Pin 1 (TX) - Reserved for future MIDI OUT

**Clock Output**
- Pin 5 (D6) - Clock driver (to transistor base)
- Pin 4 - Sync cable detect (jack switch)

**Analog Controls**
- Pin 18 (A0) - Volume pot
- Pin 19 (A1) - Cutoff pot
- Pin 20 (A2) - Resonance pot

**I2C Display**
- Pin 2 (SDA) - OLED data
- Pin 3 (SCL) - OLED clock

**Rotary Encoder**
- Pin 6 - Encoder A
- Pin 7 - Encoder B
- Pin 8 - Encoder switch

**Buttons**
- Pin 9 - Function button
- Pin 14 - Play/Pause button
- Pin 15 - Stop button

**LEDs**
- Pin 16 - Beat LED (red)
- +5V rail - Power LED (green, always on)

## Circuit Details

### MIDI IN Circuit
Standard MIDI optocoupler isolation using 6N138:
- DIN pins 4 & 5 drive optocoupler LED through 220Ω resistors
- 1N4148 diode provides reverse polarity protection
- Output pulled up to 5V with 10kΩ resistor
- Isolated output feeds Pro Micro RX1

### Clock Output Circuit
Protected open-collector design:
- Pro Micro drives 2N3904 transistor base through 10kΩ resistor
- Collector pulled up to 5V via 10kΩ resistor
- Open-collector output protects MCU if external signal accidentally connected
- Jack switch enables cable detection

### Analog Inputs
Each potentiometer:
- Outer lugs connected between +5V and GND
- Wiper buffered with 100Ω series resistor
- 100nF capacitor to ground for noise filtering

## Firmware Features

- **MIDI Clock Processing**
  - Receives MIDI Clock, Start, Stop, Continue messages
  - Generates precise analog sync pulses (configurable PPQN)
  - Clock output only active when cable detected

- **MIDI CC Generation**
  - Reads 3 analog pots and sends CC messages on change
  - Debouncing and threshold filtering to prevent noise

- **Transport Control**
  - Play/Pause and Stop buttons control internal playback state
  - Beat LED pulses on quarter notes during playback

- **Configuration Menu**
  - OLED display shows current mode and settings
  - Rotary encoder navigates menu system
  - Configurable clock source priority and PPQN

## Power

Powered entirely from USB host (5V). Typical current consumption: ~100-150mA including OLED and LEDs.

## Status

This is a breadboard prototype design. The KiCad project contains schematics and PCB layout for a permanent build.

## License

[Add your license information here]

## Author

[Add your information here]
