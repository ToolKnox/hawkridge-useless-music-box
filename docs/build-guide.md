# Build Guide: Useless Music Box with Arduino Nano

Remix of the Useless Music Box by Dawieder (Printables model 1747960, public domain). Written from scratch from the upstream model page and the firmware project. Read it once before printing.

## 1. Firmware first

The sketch is the original author's project at https://github.com/Dawied/Useless-Music-Box (use commit 329e46acdc6e, the version this guide was written against). It is not included in this package; download it from that link.

1. Install the Arduino IDE 2.x (or PlatformIO; the project has a `platformio.ini` with the `nanoatmega328` environment). Board: **Arduino Nano**, processor **ATmega328P** (try "old bootloader" if the upload times out on a cheap clone).
2. Libraries: **Servo** (built in) and **Streaming** (Mikal Hart) from the Library Manager. The repo also carries both under `lib/`.
3. Open `src/main.cpp`, rename it to a `.ino` if you use the Arduino IDE, and flash it. Serial monitor at 9600 baud.
4. Pin table (from `main.cpp`):

| Signal | Nano pin |
|---|---|
| Lid servo | D2 |
| Switch servo | D3 |
| Toggle switch (to GND, internal pull-up) | D4 |
| Passive buzzer | D7 |

5. Servo end positions are two numbers in the sketch: switch finger 15 -> 115 degrees, lid finger 90 -> 5 degrees. You will tune these after test-fitting (step 4).

## 2. Printing the parts

Print one each of `box.stl`, `lid_hinge.stl`, `lid_fixed.stl`, `switch_finger.stl`, `lid_finger.stl` and `cable_holder.stl`. The author printed with a 0.6 mm nozzle and supports; a 0.4 mm nozzle works, and most parts print without supports if you orient the fingers flat. PLA, 0.2 mm layers, 3 walls. About 5 hours total.

## 3. Electronics and wiring

1. Mount the Nano on the I/O expansion shield: every pin comes out as a G/V/S 3-pin header, so the servos plug straight in and the rest is Dupont wires, no soldering. (A screw-terminal Nano adapter has no servo headers; it is not the board for this build.)
2. 18650 holder with 5 V output: solder two male Dupont wires to its 5 V output and GND. Put the main rocker switch between the holder's + output and a V pin of the expansion board; GND to a G pin. The battery holder you buy must have a 5 V boost output (an "18650 battery shield" with USB and 5 V/3 V pins); a plain holder gives 3.7 V, which is too low for the servos.
3. Servos: lid servo to the D2 header (signal, 5 V, GND), switch servo to D3.
4. Toggle switch: one terminal to D4, the other to GND. The sketch uses the internal pull-up, so no resistor.
5. Passive buzzer: + to D7, - to GND, with Dupont wires; it clicks into the buzzer pocket in the box.
6. Fix the expansion board and the battery holder in the box with the 8 M2 x 4 mm screws.

## 4. Fitting the fingers

1. Power up once with the servos connected but the fingers off: the sketch moves both servos to their rest positions.
2. Press the **switch finger** onto the switch servo horn so that, at rest, it stops a few millimetres before the back wall of the box.
3. Press the **lid finger** onto the lid servo horn so that, at rest, it sits a few millimetres above the battery.
4. Fit the toggle switch into the hole in the lid and tighten its ring nut.
5. If a finger over- or under-travels, change the end angles in the sketch (115 for the switch finger, 5 for the lid finger) in steps of 5 degrees and re-flash.

## 5. Assembly

1. Push the MG90S servos into their printed retainers in the box.
2. Hinge: a short piece of 1.75 mm filament through the hinge knuckles of `lid_hinge.stl` and the box.
3. Snap `lid_fixed.stl` into the box.
4. Route the buzzer and battery wires through the cable holder so they stay clear of the fingers.

## 6. First run

Main switch on, flip the toggle: lid opens, a random tune plays, the finger flips the toggle back, the lid closes. Send any serial command at 9600 baud to see the debug output if something does not move.

## 7. Troubleshooting

- Servos twitch or the Nano resets when a tune plays: battery holder output cannot supply the servo peaks; charge the cell or use a holder with a stronger boost converter.
- Finger does not reach the switch: increase the end angle, or check the horn is seated one spline further.
- Nothing happens when the toggle is flipped: D4 must read LOW when the switch is on; swap to the other pair of switch terminals.
- No sound: the buzzer must be a passive type (it needs the tone signal); an active buzzer only beeps one note.
