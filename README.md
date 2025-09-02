On Arduino UNO / Nano, the hardware UART pins are:

Pin 0 → RX (receive)

Pin 1 → TX (transmit)

So when you connect your Bluetooth module (HC-05 / HC-06):

Bluetooth TX → Arduino RX (pin 0)

Bluetooth RX → Arduino TX (pin 1)
⚠️ Important: use a voltage divider (2 resistors) on the Arduino TX → Bluetooth RX line, because Arduino sends 5V but the HC-05 RX pin is 3.3V tolerant.

VCC → 5V

GND → GND
1. Adafruit Motor Shield Library

Adafruit makes Motor Shield boards for Arduino so you can control motors easily without wiring many transistors/diodes yourself.
The library gives you ready-made functions to control motors like:

setSpeed(value) → Set motor speed (0–255).

run(FORWARD) → Run motor clockwise.

run(BACKWARD) → Run motor anti-clockwise.

run(RELEASE) → Stop motor.

There are two versions of the library:

AFMotor (V1 shield, blue board)

Uses L293D chips directly.

Library: #include <AFMotor.h>

Motor object: AF_DCMotor motor(1);

Adafruit_MotorShield (V2 shield, green board, I²C)

Uses a PWM driver (PCA9685) + MOSFETs.

Library: #include <Adafruit_MotorShield.h>

Motor object:

Adafruit_MotorShield AFMS = Adafruit_MotorShield();
Adafruit_DCMotor *motor1 = AFMS.getMotor(1);


👉 The library hides the low-level work of sending signals to the motor driver chips (like L293D). You just call FORWARD / BACKWARD instead of manually toggling pins.

🔹 2. L293D Motor Driver IC

The L293D is a motor driver chip (an H-Bridge IC).
It allows a low-power microcontroller (like Arduino) to control higher-power DC motors.

Features:

Can control 2 DC motors (or 1 stepper motor).

Works with motor voltage up to 36V.

Each motor channel provides about 600mA current.

Has built-in diodes for protection from back-EMF (when motor spins down).

Working:

The chip has two H-bridges. Each H-bridge can drive one motor forward or backward.

You give logic signals from Arduino to tell the chip which direction and whether to enable the motor.

The chip then sends the proper current from the external power supply (e.g., 9V, 12V) to the motors.

✅ In summary:

Adafruit library = Arduino code that makes motor control very easy.

L293D = The actual hardware chip on the shield that drives the motors.

The library talks to the chip, the chip drives the motors.
