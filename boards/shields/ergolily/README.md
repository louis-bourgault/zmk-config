# ErgoLily Custom Keyboard Shield
# Note: written by Github Copilot, if you couldn't tell. Don't trust anything it says

This is a custom Lily58-inspired keyboard layout created with Ergogen and designed for the SuperMini nRF52840 (nice!nano v2 compatible) controller.

## Specifications

- **Layout**: 58 keys total (29 per side)
- **Controller**: SuperMini nRF52840 / nice!nano v2
- **Connection**: Split keyboard with wireless BLE
- **Diode Direction**: COL2ROW

## Pin Mapping

### Matrix Layout

The keyboard uses a 5-row × 7-column matrix per side:

#### Left Side Pins

**Column Pins (col-gpios):**
- P0.16 (P16) - Column 0: Outer column
- P0.06 (P6)  - Column 1: Pinkie column
- P0.07 (P7)  - Column 2: Ring column
- P0.08 (P8)  - Column 3: Middle column / Thumb 1
- P0.09 (P9)  - Column 4: Index column / Thumb 2
- P0.10 (P10) - Column 5: Inner column / Thumb 3
- P0.14 (P14) - Column 6: Innest column / Thumb 4

**Row Pins (row-gpios):**
- P0.00 (P0) - Row 0: Lower row
- P0.01 (P1) - Row 1: Home row
- P0.02 (P2) - Row 2: Upper row
- P0.03 (P3) - Row 3: Number row
- P0.04 (P4) - Row 4: Thumb row

### Key Layout

```
Main Matrix (Rows 0-3):
┌─────┬──────┬──────┬───────┬───────┬───────┬───────┐
│ Num │Outer │Pinkie│ Ring  │Middle │ Index │ Inner │
│ Row │ P16  │  P6  │  P7   │  P8   │  P9   │  P10  │
├─────┼──────┼──────┼───────┼───────┼───────┼───────┼──────────┐
│Upper│ RC30 │ RC31 │ RC32  │ RC33  │ RC34  │ RC35  │          │
│ P2  │      │      │       │       │       │       │          │
├─────┼──────┼──────┼───────┼───────┼───────┼───────┼──────────┤
│Home │ RC10 │ RC11 │ RC12  │ RC13  │ RC14  │ RC15  │          │
│ P1  │      │      │       │       │       │       │          │
├─────┼──────┼──────┼───────┼───────┼───────┼───────┼──────────┤
│Lower│ RC00 │ RC01 │ RC02  │ RC03  │ RC04  │ RC05  │ RC06     │
│ P0  │      │      │       │       │       │       │ P14      │
└─────┴──────┴──────┴───────┴───────┴───────┴───────┴──────────┘

Thumb Cluster (Row 4):
                    ┌───────┬───────┬───────┬───────┐
                    │Thumb1 │Thumb2 │Thumb3 │Thumb4 │
                    │ P8    │  P9   │  P10  │  P14  │
         ┌──────────┼───────┼───────┼───────┼───────┤
         │Thumb P4  │ RC43  │ RC44  │ RC45  │ RC46  │
         └──────────┴───────┴───────┴───────┴───────┘
```

### Matrix Coordinates (RC notation)

The keyboard maps to the following row-column coordinates:

**Main Matrix:**
- Row 3 (Num):   RC(3,0) to RC(3,5) - 6 keys
- Row 2 (Upper): RC(2,0) to RC(2,5) - 6 keys
- Row 1 (Home):  RC(1,0) to RC(1,5) - 6 keys
- Row 0 (Lower): RC(0,0) to RC(0,6) - 7 keys (includes innest at column 6)

**Thumb Cluster:**
- Row 4 (Thumb): RC(4,3), RC(4,4), RC(4,5), RC(4,6) - 4 keys

**Total: 29 keys per side**

## Right Side Pin Mapping

The right side uses the same pins but with reversed column order to create the mirrored layout. The `col-offset = <8>` in the device tree handles the matrix transformation.

## Wiring Notes

1. **Shared Columns**: The thumb cluster shares column pins with the main matrix:
   - Thumb 1 shares P8 with Middle column
   - Thumb 2 shares P9 with Index column
   - Thumb 3 shares P10 with Inner column
   - Thumb 4 shares P14 with Innest column (but Innest only uses lower row)

2. **Diode Direction**: COL2ROW means diodes point from columns to rows (cathode on row side)

3. **Pull-down Resistors**: Rows use GPIO_PULL_DOWN configuration

## Building Firmware

The firmware builds automatically via GitHub Actions when you push to your repository.

1. Commit and push your changes
2. Go to the "Actions" tab in your GitHub repo
3. Wait for the build to complete
4. Download the firmware files:
   - `ergolily_left-nice_nano_v2.uf2`
   - `ergolily_right-nice_nano_v2.uf2`

## Flashing

1. Connect the controller via USB
2. Double-press the reset button to enter bootloader mode
3. A drive named "NICENANO" (or similar) will appear
4. Drag and drop the appropriate `.uf2` file
5. The controller will automatically reboot with new firmware

Flash `ergolily_left` to the left controller and `ergolily_right` to the right controller.

## Customizing

To customize your keymap, edit `config/ergolily.keymap` with your desired key bindings.

To modify settings, edit `config/ergolily.conf`.

## Troubleshooting

- **Keys not responding**: Check your soldering connections and diode orientation
- **Wrong keys triggering**: Verify your pin assignments match your PCB wiring
- **Wireless not working**: Make sure both halves are flashed and paired via Bluetooth settings layer
