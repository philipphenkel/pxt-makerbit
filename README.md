# MakerBit

[![Build Status](https://travis-ci.org/1010Technologies/pxt-makerbit.svg?branch=master)](https://travis-ci.org/1010Technologies/pxt-makerbit)

The MakerBit connects to the BBC micro:bit to provide easy connections to a wide variety of sensors, actuators and other components. This is a package for Microsoft Makecode.

http://makerbit.com/

| ![MakerBit](https://github.com/1010Technologies/pxt-makerbit/raw/master/MakerBit.png "MakerBit") | ![MakerBit+R](https://github.com/1010Technologies/pxt-makerbit/raw/master/MakerBit+R.png "MakerBit+R")  |
|:--:|:--:|
| *MakerBit* | *MakerBit+R with motor controller* |


## Motors
The MakerBit board provides a motor controller that can control two bi-directional DC motors, or four one-direction motors.

### MakerBit runMotor
Sets the speed of a motor in the range of -100 to 100.
```sig
makerbit.runMotor(makerbit.Motor.A, 80)
```

### MakerBit stopMotor
Stops a motor.
```sig
makerbit.stopMotor(makerbit.Motor.A)
```

### MakerBit setMotorDirection
Sets the direction of a motor. Use this function at start time to configure your motors without the need to rewire.
```sig
makerbit.setMotorDirection(makerbit.Motor.A, makerbit.MotorDirection.Reverse)
```


## Touch
MakerBit offers built-in support for up to 12 touch sensors via the proximity capacitive touch sensor controller MPR121.

### MakerBit isTouchDetected
Returns true if a specific touch sensor is touched. False otherwise.
```sig
makerbit.isTouchDetected(makerbit.TouchSensor.T1)
```

### MakerBit onTouchDetected
Do something when a touch event is detected.
```sig
makerbit.onTouchDetected(makerbit.TouchSensor.T5, () => {})
```


## Serial MP3
This package includes support for external Serial MP3 devices with the YX5300 chip.

The microSD card has to be formatted as FAT16 or FAT32. exFAT is not supported properly and shall not be used.

To support all commands properly, the file structure needs to follow a strict pattern:
- Directory names are two-digit numbers, e.g. `01`.
- Track names within the directories shall start with a three digit numbers such as `001.mp3` or `002.wav`

Up to 99 directories and 255 tracks are supported.

```
├── 01/
│   ├── 001.mp3
│   ├── 002 second track.mp3
│   └── 003 third track.mp3
├── 02/
│   ├── 001.mp3
│   └── 002.mp3
│
…
```

The MP3 device reads files and folders in alphabetic order. It is required to create a sequence of folders like `01`, `02`, `03` and name the tracks within each folder starting at `001`. Make sure to avoid gaps in your number based naming scheme. This allows you to use folder and track names as parameters in the playback functions below.

If you experience playback problems, check for deviations to the naming convention and the file system format.

### MakerBit connectSerialMp3
Connects to serial MP3 device with chip YX5300. The first pin needs to be attached the MP3 device receiver pin (RX) and the second pin to the MP3 device transmitter pin (TX).
```sig
makerbit.connectSerialMp3(makerbit.Pin.A0, makerbit.Pin.A1)
```

### MakerBit playMp3TrackFromFolder
Plays a single track from a folder.
```sig
makerbit.playMp3TrackFromFolder(1, 1, makerbit.Repeat.No)
```

### MakerBit playMp3Folder
Plays all tracks in a folder.
```sig
makerbit.playMp3Folder(1, makerbit.Repeat.No)
```

### MakerBit setMp3Volume
Sets the volume.
```sig
makerbit.setMp3Volume(30)
```

### MakerBit runMp3Command
Dispatches a command to the MP3 device.
```sig
makerbit.runMp3Command(makerbit.Mp3Command.PLAY_NEXT_TRACK)
```


## Ultrasonic
Attach an external HC-SR04 ultrasonic distance sensor to steer your robots.

### MakerBit getUltrasonicDistance
Measures the distance and returns the result in a range from 1 to 300 centimeters or up to 118 inch. The maximum value is returned to indicate when no object was detected.
```sig
makerbit.getUltrasonicDistance(makerbit.DistanceUnit.CM, makerbit.Pin.P5, makerbit.Pin.P8)
```

### Ultrasonic Example: Distance Graph
```blocks
let distance = 0
basic.forever(() => {
    distance = makerbit.getUltrasonicDistance(makerbit.DistanceUnit.CM, makerbit.Pin.P5, makerbit.Pin.P8)
    led.plotBarGraph(distance, 0)
})
```


## LCD
Use an I2C LCD 1602 to display numbers and text.

### MakerBit showStringOnLcd
Displays a string on the LCD at a given position. The text is wrapped automatically at line end.
```sig
makerbit.showStringOnLcd("Hello world", 0)
```

### MakerBit showNumberOnLcd
Displays a number on the LCD at a given position. The number is wrapped automatically at line end.
```sig
makerbit.showNumberOnLcd(42, 16)
```

### MakerBit clearLcd
Clears the LCD completely.
```sig
makerbit.clearLcd()
```

### MakerBit setLcdBacklight
Enables or disables the backlight of the LCD.
```sig
makerbit.setLcdBacklight(makerbit.LcdBacklight.On)
```

### MakerBit connectLcd
Connects to the LCD at a given I2C address.
```sig
makerbit.connectLcd(39)
```

### MakerBit position
Turns a LCD position value into a number.
```sig
makerbit.position(makerbit.LcdPosition.P16)
```


### LCD Example
```blocks
makerbit.connectLcd(39)
makerbit.setLcdBacklight(makerbit.LcdBacklight.Off)
makerbit.showStringOnLcd("MakerBit", 0)
makerbit.showNumberOnLcd(42, 16)
basic.pause(2000)
makerbit.clearLcd()
```

## License

MIT

## Supported targets

* for PXT/microbit
