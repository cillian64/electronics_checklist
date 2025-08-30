# Electronics Checklist
This is a checklist intended to help avoid common mistakes in electronics
schematics and PCBs.  It is based on the full checklist in README.md but
stripped down to remove many of the requirements irrelevant to basic hobbyist
PCBs.

This is based on the [CUSF PCB
checklist](https://wiki.cusf.co.uk/Pcb_checklist).  Feel free to use this on
your own work.  If you make a copy, please provide attribution (to me and
CUSF).

# Schematic
## Parts to include
- Dummy schematic parts for mounting holes
- Decoupling capacitors where necessary
- FETs should have a gate pull-down or pull-up

## Components
- Check component values
- Check component power ratings, mainly resistors
- Check capacitor voltage ratings
- Check component pin numbering match datasheet
- Check microcontroller pin assignments against pin functions.
- Correct voltage rail (5V, 3.3V, 1.8V) connected to parts
- Check FET power dissipation (work out the actual R_DS_ON for your gate
  voltage)
- Check FETs will actually turn on enough (Use the charts in the datasheet, the
  simple threshold voltage / R_DS_ON test voltage aren't very helpful)

## General
- Check any UARTs have RX/TX swapped if necessary
- Run Electrical Rule Check (ERC)


# PCB
## General / setup
 - Check minimum via drill and annular ring against fab capability
 - Check minimum track/gap spacing against fab capacity

## Footprints
- Check all parts have footprints
- Check footprint pin numbering matches datasheet and schematic pin numbering,
  especially on connectors and passives
- Check pinouts against datasheets
- Check component footprints against datasheets
- Check footprints are not mirrored

## Mounting, physical clearance
- Add mounting holes, with enough clearance for screw-heads and washers
- Check PCB outline/geometry against enclosure or other mechanical parts
- Check that connectors will actually have clearance to be plugged/unplugged

## Routing and planes
- Power nets should be simple, tree-like, and have no loops
- Check power traces are as thick as practical with chunky vias where needed
- Check ground planes are as complete as possible
- Check for parts which need metal exclude (eg. antennas)
- Check planes use thermal relief spokes

## Silkscreen
- Name, contact details, copyright/license
- URL of the repository if open source (and if there is space)
- If using JLC, placeholder for order number
- PCB Revision
- Label connector functions and possibly pinout. Check pin 1 is marked.
- Check that electrolytic capacitors have very clear polarity marking

## Miscellaneous / final checks
- Run Design Rule Check (DRC)
