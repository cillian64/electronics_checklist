# Electronics Checklist
This is a checklist intended to help avoid common mistakes in electronics
schematics and PCBs.  It mostly focuses on digital electronics because I don't
do a lot of high-speed or analog work.  This is based on the [CUSF PCB
checklist](https://wiki.cusf.co.uk/Pcb_checklist) and expanded a lot based on
my experiences (mistakes I have made personally, mistakes I have seen others
make, and things I have been taught by others).

Feel free to use this on your own work.  If you make a copy, please provide
attribution (to me and CUSF).

# Schematic
## Parts to include
- Dummy schematic parts for mounting holes
- ESD protection on anything sensitive and world-facing (connectors, antennas)
- Pull-ups on I2C buses
- Test points where needed, e.g. signals which only touch pads which are
  obscured (qfn/bga), power rails, things likely to need measuring/bodging
- Decoupling capacitors where necessary (100nF on the power pins of any IC
  unless the datasheet says otherwise)
- FETs should have a gate pull-down or pull-up (depending which option is more
  fail-safe) and potentially an edge-rate-limiting series resistor
- Oscillator crystals should have capacitors to aid with starting.  Values
  depend on track inductance.
- Check that parts which specify a crystal or ceramic resonator have the right
  one.  Resistors may be needed depending on voltage rating.

## Components
- Check component values
- Check component pin numbering match datasheet
- Check microcontroller pin assignments against pin functions.  Check for
  Alternate Function collisions, DMA collisions, etc.
- Correct voltage rail (5V, 3.3V, 1.8V) connected to parts
- Check datasheet app notes for complicated parts (power regulators,
  microcontrollers, etc.)

## General
- Check any UARTs have RX/TX swapped if necessary
- Check all parts have a component specified (order code)
- Check all parts have a footprint chosen
- LED indicators where appropriate (use a 1k resistor so as to not blind
  anyone, modern high-efficiency LEDs are far too bright with 20mA)

## Part selection
- Check component values are correct (for functionality, check datasheets where
  needed)
- Check component tolerances are correct anywhere precision is required.
  Specify matched components if needed
- Check component voltage ratings, check that footprints are appropriate for
  those power ratings (i.e. probably don't put mains across an 0603 even if the
  datasheet says you can)
- Check component power ratings
  - Particularly resistors, dividers can dissipate more than you might expect
  - Current sense resistors sometimes need to be massive
  - Capacitors passing AC current also dissipate power because of their ESR
- Check capacitor voltage ratings
- Check that chip capacitor dialectrics are appropriate.
  - Y5V is terrible and you probably shouldn't use it - it can lose up to 82%
    of its capacitance depending on temperature and ageing.
  - C0G is only available for small values but is good where precision is
    needed
  - X5R and X7R are good for general purpose power decoupling (0.1uF up to
    22uF)
- Check FET power dissipation (work out the actual R_DS_ON for your gate
  voltage)
- Check FETs will actually turn on enough (Use the charts in the datasheet, the
  simple threshold voltage / R_DS_ON test voltage aren't very helpful)
- Consider part availability
  - Check that parts are actually available from somewhere sensible
  - Try to use parts with common footprints so they can be swapped out without
    a PCB re-spin
  - For parts without equivalents consider ordering sufficient stock before
    committing to the PCB design

## Final checks
- Don't mix up global and local/hierarchical net labels
- Check nets labels / power rails actually connect as appropriate (i.e.
  misspelt labels won't connect)
- Check pin directions (so that ERC will work)
- Run Electrical Rule Check (ERC)


# PCB
## General / setup
 - Check minimum via drill and annular ring against fab capability
 - Check minimum track/gap spacing against fab capacity

## Footprints
- Check footprint pin numbering matches datasheet and schematic pin numbering,
  especially on connectors and passives
- Check all parts have footprints
- Check pinouts against datasheets
- Check component footprints against datasheets (especially for new footprints
  or library footprints you haven't used before).
- Check footprints are not mirrored - some datasheet footprints are above view
  (X-ray style), some are below view (i.e. turn the component upside-down
  style)

## Component placement
- Place any connectors first along with things requiring big heatsinks or with
  clearance constraints
- Decoupling capacitors close to IC power pins, smallest values closest
- ESD diodes as close to connector pins as possible

## Mounting, physical clearance
- Add mounting holes, with enough clearance for screw-heads and washers. If you
  have copper under the screw-heads check it is either isolated or an
  appropriate potential (e.g. chassis ground)
- Check PCB outline/geometry against enclosure or other mechanical parts
  - If possible, export a 3d model of the PCB including components and add it
    to your CAD model
  - Consider 3d printing the PCB with components to check fit
  - If neither of those are possible, print out the PCB 1:1 to check fit
- Check component shapes/height against enclosure or other mechanical parts
- Radius board corners (because it looks nice)
  - Be aware of the fab's minimum radius
- Check that connectors will actually have clearance to be plugged/unplugged
- Consider whether heatsinks need isolating, what voltage they will be at, and
  what they will touch
- Check test points have enough clearance to get a probe/wire on, aren't near
  anything you will accidentally short against.  Consider which side of the PCB
  they should be on

## Routing
- Power nets should be simple, tree-like, and have no loops
- Check power traces are as thick as practical and use multiple vias when
  changing layer
- Check high-speed traces go over uninterrupted ground plane (any interruptions
  will be an impedance discontinuity)
- Check high-speed or high-power traces have a sensible path for return current
  with minimum enclosed area (to avoid extra inductance and creating a loop
  antenna)
- Check high-speed traces have appropriate corners (45 degrees or curved to
  minimise impedance discontinuities)
- Check any impedance-matched traces are handled correctly (depends on your
  board-house)
- Crystal traces should be as short and symmetrical as possible

## Power / ground planes
- Check any ground/power planes don't have multiple big areas separated by a
  thin neck - this can oscillate, act as an antenna, or end up with a voltage
  difference if a lot of current flows
- Check ground planes are as complete as possible, budging traces around can
  eliminate holes in the plane
- Check for parts which require a plane exclude on some/all layers (e.g.
  magnetometers, some antennas)
- Check for parts which require a solid ground plane below them (e.g. some RF
  modules, some antennas)
- Check adequate clearance on planes - they need more clearance than the
  minimum used for traces/pads
- Check planes use thermal relief spokes rather than solid connections to pads
  to make hand-soldering easier
  - Consider only using thermal relief spokes for PTH and solid connections
    for SMD
- Consider filling empty layers with planes (makes etching easier, less
  important for commercial fabs)
- Plane stitching if multiple planes are used, especially if high-speed
- Plane stitching around any high speed signals, e.g. antenna feeds
- Plane stitching around the edge of the PCB (to avoid internal power plane
  edges acting as a slot antenna which can cause EMC problems)

## Silkscreen
- Name, contact details, copyright/license
- URL of the repository if open source (and if there is space)
- PCB Revision
- Space to write hardware revision or a knock-out box (e.g. for component value
  changes or bodge-wires)
- Label connector functions
- If there is space, label pinouts for non-standard connectors
- Label pin 1 or connector polarity if the connectors are not physically
  polarised
- Check that any polarised electrolytic capacitors have very clear polarity
  marking
- High voltage warnings, etc.

## Paste stencil
- Check stencil apertures are the correct shape
- Check apertures are the right size for stencil thickness
- Check that all parts to be reflowed have paste on pins
- Check that any parts not being reflowed don't have paste on pins
- Check no PTH parts have paste on pins
- Check there is no paste on edge connectors, pogo pin pads, TagConnect pads,
  etc.

## Miscellaneous / final checks
- Check any sensitive areas against reference designs
- Check high speed or sensitive signal routing
- Check high speed or high power signals have an appropriate return path with
  minimum enclosed area (e.g. unbroken ground plane below)
- Check any small passives have similar thermal sinking on each side to avoid
  tombstoning
- Check any high voltage isolation has sufficient gap, creep, slots, etc.
- Check for acid traps (acute angles on copper layers where etchant can get
  stuck and over-etch)
- Consider printing out the the PCB design 1:1 to look for anything obvious
  wrong and check mounting/clearance
- Check for issues which can cause PCB warping
  - Check the stackup is balanced (e.g. copper thicknesses)
  - Check the copper fill proportion is relatively balanced (e.g. solid ground
    pour on one side and minimal copper on the other side could cause problems)
  - Check directionality is balanced (e.g. you can have problems if one side
    has lots of traces alined to the X axis and the other side has traces
    mostly aligned to the Y axis.)
- Check minimum width on mask layer between pads.  Small mask slivers can break
  off and cause problems
- Run Design Rule Check (DRC)
