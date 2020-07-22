# Firmware
Starting with Marlin 2.0.5.3 from the [Github](https://github.com/MarlinFirmware/Marlin "Marlin firmware")

# Hardware Settings

## Motherboard
**Note:** Find board name in boards.h and use that for the define value
### from src/boards.h
```C++
#define BOARD_TANGO                   1148  // BIQU Tango V1
```
```C++
// Choose the name from boards.h that matches your setup
#ifndef MOTHERBOARD
    #define MOTHERBOARD BOARD_TANGO 
#endif
```

## Core XY
```C++
// Uncomment one of these options to enable CoreXY, CoreXZ, or CoreYZ kinematics
// either in the usual order or reversed
#define COREXY //
//#define COREXZ
```

## Stepper Drivers
```C++
#define X_DRIVER_TYPE  A4988 
#define Y_DRIVER_TYPE  A4988 
```
## Controller Board

**Note:** The LCD was readable but messed up..  like noise was getting into it. It turned out to be a timing issue. Found the solution in the comments of this [YouTube](https://www.youtube.com/watch?v=yh-PQ7BpZl8 "Marlin 1.1.9 reprap full graphic error") video.
```C++
// RepRapDiscount FULL GRAPHIC Smart Controller
// http://reprap.org/wiki/RepRapDiscount_Full_Graphic_Smart_Controller
//
#define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER 
#define ST7920_DELAY_1 DELAY_NS(0) 
#define ST7920_DELAY_2 DELAY_NS(250) 
#define ST7920_DELAY_3 DELAY_NS(250) 
```
# Servo

## Servo command
```
M280 P0 S<servoAngle>
```
## Enable Servo
```C++
/**
 * R/C SERVO support
 * Sponsored by TrinityLabs, Reworked by codexmas
 */

/**
 * Number of servos
 *
 * For some servo-related options NUM_SERVOS will be set automatically.
 * Set this manually if there are extra servos needing manual control.
 * Leave undefined or set to 0 to entirely disable the servo subsystem.
 */
#define NUM_SERVOS 1 // Servo index starts with 0 for M280 command

// (ms) Delay  before the next move will start, to give the servo time to reach its target angle.
// 300ms is a good value but you can try less delay.
// If the servo can't reach the requested position, increase it.
#define SERVO_DELAY { 300 }

// Only power servos during movement, otherwise leave off to prevent jitter
//#define DEACTIVATE_SERVOS_AFTER_MOVE

// Allow servo angle to be edited and saved to EEPROM
//#define EDITABLE_SERVO_ANGLES
```
### pins/ramps/pins_TANGO.h
No mention of servos here but includes RUMBA pins definitions
```C++
#include "pins_RUMBA.h" 
```

### pins/ramps/pins_RUMBA.h
```C++
//
// Servos
//
#define SERVO0_PIN                             5
```
Pin 5 of the ATMEGA2560 is Called PWM2 on the schematic and goes to Auxilary IO connector EXP3 pin 6.

# No Extruders
At Extruders = 1
   - Sketch uses 109734 bytes (43%) of program storage space. Maximum is 253952 bytes.
Global variables use 4353 bytes (53%) of dynamic memory, leaving 3839 bytes for local variables. Maximum is 8192 bytes.

Making Extruders = 0
```C++
// This defines the number of extruders
// :[1, 2, 3, 4, 5, 6, 7, 8]
#define EXTRUDERS 0
```
Sketch uses 94528 bytes (37%) of program storage space. Maximum is 253952 bytes.
Global variables use 4246 bytes (51%) of dynamic memory, leaving 3946 bytes for local variables. Maximum is 8192 bytes.

# Stepper Motors
The Y axis was reversed from my plans so I tried this
```C++
#define INVERT_Y_DIR FALSE
```
This didn't work. Now the axis are swapped but they go in the proper direction
Changed it back and swapped motor posistions.
```C++
#define INVERT_Y_DIR TRUE
```
This made proper axis both with inverted direction.

Reversed connectors in place. SUCCESS!!

# Calibration
No adjustment required from default values.
```C++
/**
 * Default Axis Steps Per Unit (steps/mm)
 * Override with M92
 *                                      X, Y, Z, E0 [, E1[, E2...]]
 */
#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 4000, 500 }
```
# Encoder Direction
I want the same direction as my Prusa 3D printer
```C++
/**
 * Encoder Direction Options
 *
 * Test your encoder's behavior first with both options disabled.
 *
 *  Reversed Value Edit and Menu Nav? Enable REVERSE_ENCODER_DIRECTION.
 *  Reversed Menu Navigation only?    Enable REVERSE_MENU_DIRECTION.
 *  Reversed Value Editing only?      Enable BOTH options.
 */
 #define REVERSE_ENCODER_DIRECTION 
 ```
# Travel Limits
```C++
// The size of the print bed
#define X_BED_SIZE 380
#define Y_BED_SIZE 380

```
# Custom Menues
- Pen Set - Raise the pen holder mid way to mount pen.
- Pen Up - Raise the pen holder all the way to be ready for printing.
- Pen Down - Lower the pen holder all the way to test engagement.
```C++
/**
 * User-defined menu items that execute custom GCode
 */
#define CUSTOM_USER_MENUS
#if ENABLED(CUSTOM_USER_MENUS)
  //#define CUSTOM_USER_MENU_TITLE "Custom Commands"
  //#define USER_SCRIPT_DONE "M117 User Script Done"
  
  #define USER_DESC_1 "Pen Set"
  #define USER_GCODE_1 "M280 P0 S50"

  #define USER_DESC_2 "Pen Up" 
  #define USER_GCODE_2 "M280 P0 S70" 

  #define USER_DESC_3 "Pen Down" 
  #define USER_GCODE_3 "M280 P0 S0"

#endif
```