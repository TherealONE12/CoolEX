# CooleX

A laptop cooling stand with up to 5 fans and custom PWM control. Built because my laptop kept overheating and commercial cooling pads are overpriced.

And also why not?

## What it is

This is a 3D printed stand with five 80mm fans that sit directly under your laptop. The fans are controlled by a custom PCB that generates PWM signals from a simple analog circuit. Power comes from any USB-C PD charger that can do 12V.

The whole thing costs around $20-25 to build and hopefuly works better than those $60 "gaming" cooling pads on Amazon.

## Project structure

```
hardware/
├── gerbers/
│   └── pwm_Colex_gerbers.zip    ← send this to JLCPCB
├── pwm_Colex.kicad_pro
├── pwm_Colex.kicad_sch           ← schematic
├── pwm_Colex.kicad_pcb           ← PCB layout
├── pwm_Colex.csv                 ← detailed component list
└── BOM.csv                       ← shopping list

mechanical/
├── Part_Studio_1_-_Part_1.step   ← CAD model
└── Part_Studio_1_-_Part_1.stl    ← print this

docs/
└── JOURNAL.md                     ← dev log from the build process
```

## Bill of materials

| Part | Qty | Where | Price | Link |
|------|-----|-------|-------|------|
| Xilence XPF80.R.PWM fans (80mm, 4-pin) | up to 5, recommended 2-4 | Amazon | $16.90 total (For 5)| [Link](https://www.amazon.de/Xilence-XPF80-R-PWM-XF040-PC-L%C3%BCfter-schwarz/dp/B01L6QAX76) |
| PCB (custom) | 1 | JLCPCB | ~$2 | Upload gerbers zip |
| Electronic components | set | Mouser/Digikey | ~$3 | See hardware/pwm_Colex.csv |
| 3D printed stand | 1 | Your printer | ~$2 filament | mechanical/*.stl |

**Total: ~$24**

### Key components on the PCB

- **U1**: NE555DR timer - generates the PWM signal
- **U2**: USB PD chip - asks your charger for 12V
- **Q1-Q5**: MOSFETs - switch each fan on/off
- **D1-D5**: Flyback diodes - protect the MOSFETs from voltage spikes
- **R_POT**: Potentiometer - this is your speed knob


## How it works

**Power:** USB-C PD controller negotiates 12V from whatever charger you plug in

**PWM generation:** The NE555 is wired as an oscillator. Frequency is set by resistors/caps, duty cycle is adjustable with the pot. This gives you smooth fan speed control from silent to full blast.

**Fan control:** Each fan gets constant 12V on the positive rail. The MOSFETs switch the ground connection on/off rapidly (that's the PWM).

## Why I built this

My laptop would hit 95°C and throttle down to like 1.2GHz whenever I tried to compile or game anything. Regular cooling pads either:
- Don't actually blow air WHERE the laptop intakes it
- Use loud DC fans with no speed control
- Cost $40-60 for what's basically fans and plastic

I wanted PWM control (so it can be quiet when idle), proper positioning under the intake vents, and USB-C power. Nothing on the market did all three without being stupidly expensive.

Also I didn't want to use a microcontroller for this. It's literally just "make a square wave with adjustable duty cycle" - the NE555 has been doing that since 1972. Why overcomplicate it?

## Build notes

### PCB

Upload `hardware/gerbers/pwm_Colex_gerbers.zip` to JLCPCB or PCBWay. Default options are fine (2-layer, 1.6mm, HASL finish). Order 5 boards for like $2 + shipping.

Components are all through-hole for easy soldering. Check `hardware/pwm_Colex.csv` for part numbers. Total BOM cost is around $3-4 if you buy from Mouser/Digikey.

### 3D print

Print `mechanical/Part_Studio_1_-_Part_1.stl` in PLA or PETG. Use supports for the overhangs. 20% infill is plenty.

The design has cutouts for USB ports and stuff - these are sized for MY laptop (check JOURNAL.md for which one). You might need to modify them in CAD if your laptop has different port positions.

### Assembly

1. Solder everything to the PCB
2. Drop the five fans into the stand's mounting holes
3. Wire fans to the PCB headers (J2 through J6)
4. Mount the PCB in the little mount on the backside - May or May Not need Hot glue
5. Plug in USB-C and adjust the pot for desired speed

## Usage

Put your laptop on the stand. Plug the USB-C into any PD charger. Turn the pot to adjust fan speed. That's it.

The PD controller will automatically request 12V - if your charger can't do 12V it'll fallback to 9V/5V and the fans will just run slower (still works fine).

## Development

Built for Hack Club's Half Life program. See `docs/JOURNAL.md` for the full build log including all the mistakes I made (there were several).

Main learning: I spent 90 minutes trying to build a sawtooth generator from scratch before realizing the NE555 could just... do that. Read datasheets before reinventing wheels.

## Tools used

- OnShape for CAD (because SolidWorks doesn't run on Linux)
- KiCad 10.0 for PCB design


## Future ideas

- Add a temp sensor so it can auto-adjust based on laptop temperature
- Maybe some LEDs to show fan speed
- Modular mounting holes for different laptop sizes
- USB hub passthrough would be neat

## License

MIT - do whatever you want with it. If you build one, send me a picture!

## Credits

Built by Jakob for Hack Club Half Life

Thanks to asktheman for the sawtooth generator reference image that I ended up not using because the NE555 exists


Built by Jakob. With lots of <3 and some Bugs :)
