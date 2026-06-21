# :musical_score: EMBEDDED MUSIC PLAYER

## :open_book: OVERVIEW
Date: May 2025\
Developer(s): Ashneet Rathore

Embedded Music Player is an ATMega32 microcontroller-based system that plays predefined songs, composed as frequency-based note sequences. The breadboard circuit integrates a speaker for outputting audio, a LCD for displaying song titles, and a keypad for controlling playback, selecting songs, and adjusting pitch and tempo.

## :brain: FIRMWARE DESIGN
Run in **Microchip Studio**, the firmware generates individual music notes at the microcontroller level and combines them to play complete songs. Notes are encoded as pitch and duration pairs, and songs are represented as arrays of notes. Each note is played by toggling a GPIO output pin to produce a square-wave signal at the desired frequency for the specified duration.

**Precise timing** is critical for accurate audio generation. A high-resolution delay function provides a fine-grianed delay control in units of 8 microseconds, controlling the high and low periods of each waveform cycle to ensure consistent pitch and tempo.

User input is handled through **polling-based keypad scanning**, allowing playback control, song selection - *Twinkle Twinkle Little Star*, *Mary Had a Little Lamb*, and *Birthday Song* - and real-time pitch and tempo adjustment. The LCD updates to display the current song title.

## :open_file_folder: PROJECT FILE STRUCTURE
```bash
music-player/
│── main.c                  # Main program logic for the music player
│── avr.h                   # AVR macros and timing utilities
│── lcd.h                   # LCD control and display function declarations
│── lcd.c                   # LCD control and display function implementations
│── assets/               
│   │── circuit_image.jpg   # Image of circuit
│   └── schematic.png       # Schematic of circuit
│── README.md               # Project documentation
└── .gitignore              # Ignored files
```

## :gear: CIRCUIT SET UP GUIDE
> [!NOTE]
> View the circuit schematic and an image of the finished circuit in the [assets folder](assets/).

### :toolbox: REQUIRED HARDWARE
- ATMega 32 Microcontroller
- Breadboard (with solid core or jumper wires)
- ATMEL-ICE Programmer
- 9V Battery (with 9V battery connector)
- LCD Display
- 4 by 4 Keypad
- Speaker
- 5V Voltage Regulator
- 2 0.1µF Capacitors
- 8 MHz Crystal
- 1K Resistor

### :battery: BUILDING THE POWER SUPPLY
The voltage regulator has three pins: input (Pin 1), ground (Pin 2), and output (Pin 3). The circuit takes +9V as input and regulates it down to a +5V output, using the capacitor to smooth out the output voltage.
1. Connect the positive terminal of the 9V Battery to Pin 1.
2. Connect the negative terminal of the 9V Battery to Pin 2.
3. Connect the negative (shorter) leg of a capacitor to Pin 2, and the positive leg of a capacitor to Pin 3.
4. Connect Pin 3 to the positive rail of the breadboard and Pin 2 to the negative rail to power one side of the breadboard.
5. Join the two power rails on each side of the breadboard (positive together, negative together) to deliver voltage to both sides.

### :electric_plug: CONNECTING THE PROGRAMMER TO THE MICROCONTROLLER
1. Connect the ATMEGA32 microcontroller to the breadboard securely.
2. Connect the six programmer pins to the microcontroller as shown below:

    | Programmer Pin    | Microcontroller Pin |
    |-------------------|---------------------|
    | 1                 | PB6 (Pin 7)         |
    | 2                 | PB7 (Pin 8)         |
    | 3                 | RESET (Pin 9)       |
    | 4                 | VCC (Pin 10)        |
    | 5                 | PB5 (Pin 6)         |
    | 6                 | GND (Pin 11)        | 
3. Power the microcontroller by connecting VCC (Pin 10) to the positive rail of the breadboard and GND (Pin 11) to the negative rail.

### :hourglass: CONNECTING THE 8 MHZ CRYSTAL TO THE MICROCONTROLLER
1. Connect one leg of a 8 MHz crystal to XTAL2 (Pin 12) on the microcontroller.
2. Connect the other leg of the crystal to XTAL1 (Pin 13) on the microcontroller.

### :wrench: CONFIGURING THE FUSES IN MICROCHIP STUDIO
1. In the top navigation bar, go to *Tools* → *Device Programming* → *Fuses* → *LOW_SUT_CKSEL*.
2. Select *Ext. Crystal/Resonator High Freq: Start-up time: 16k CK + 64 ms*.
3. Click *Program*.

### :1234: CONNECTING THE KEYPAD TO THE MICROCONTROLLER
1. Connect the 8 keypad pins to PORT C of the microcontroller as shown below:

    | Keypad Pin        | Microcontroller Pin |
    |-------------------|---------------------|
    | C0                | PC0 (Pin 22)        |
    | C1                | PC1 (Pin 23)        |
    | C2                | PC2 (Pin 24)        |
    | C3                | PC3 (Pin 25)        |
    | R0                | PC7 (Pin 29)        |
    | R1                | PC6 (Pin 28)        | 
    | R2                | PC5 (Pin 27)        |
    | R3                | PC4 (Pin 26)        | 

### :framed_picture: CONNECTING THE LCD TO THE MICROCONTROLLER
The LCD has a total of 16 pins, including source pins that supply power to the display, control pins that manage its operation, and data pins that carry the information displayed. LCD Pins 15 and 16 are unused for this project.
1. To supply power to the LCD, connect VSS (LCD Pin 1) to the negative rail of the breadboard and VDD (LCD Pin 2) to the positive rail.
2. Connect one leg of a 1k resistor to VO (LCD Pin 3) and the other leg to the negative rail of the breadboard.
3. Connect the control pins of the LCD to PORT B of the microcontroller as shown below:

    | LCD Pin           | Microcontroller Pin |
    |-------------------|---------------------|
    | RS (Pin 4)        | PB0 (Pin 1)         |
    | R/W (Pin 5)       | PB1 (Pin 2)         |
    | E (Pin 6)         | PB2 (Pin 3)         |

4. Connect the data pins of the LCD to PORT D of the microcontroller as shown below:

    | LCD Pin           | Microcontroller Pin |
    |-------------------|---------------------|
    | DB0 (Pin 7)       | PD0 (Pin 14)        |
    | DB1 (Pin 8)       | PD1 (Pin 15)        |
    | DB2 (Pin 9)       | PD2 (Pin 16)        | 
    | DB3 (Pin 10)      | PD3 (Pin 17)        |
    | DB4 (Pin 11)      | PD4 (Pin 18)        | 
    | DB5 (Pin 12)      | PD5 (Pin 19)        |
    | DB6 (Pin 13)      | PD6 (Pin 20)        |
    | DB7 (Pin 14)      | PD7 (Pin 21)        | 

### :loudspeaker: CONNECTING THE SPEAKER TO THE MICROCONTROLLER
1. Connect the negative terminal of the speaker to the negative rail of the breadboard.
2. Connect one leg of a capacitor to PB3 (Pin 4) on the microcontroller.
3. Connect the other leg of a capacitor to the positive terminal of the speaker.

## :point_up_2: KEYPAD CONTROLS
| Key | Function                               |
|-----|----------------------------------------|
| *   | Start / stop song                      |
| A   | Select *Twinkle Twinkle Little Star*   |
| B   | Select *Mary Had a Little Lamb*        |
| C   | Select *Birthday Song*                 |
| 1   | Set frequency to 220 Hz (low pitch)    |
| 2   | Set frequency to 440 Hz (normal pitch) |
| 3   | Set frequency to 880 Hz (high pitch)   |
| 4   | Set tempo to slow                      |
| 5   | Set tempo to normal                    |
| 6   | Set tempo to fast                      |