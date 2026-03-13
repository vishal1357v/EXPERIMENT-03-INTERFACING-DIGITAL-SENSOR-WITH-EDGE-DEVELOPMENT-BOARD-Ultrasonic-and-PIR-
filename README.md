# EXPERIMENT-03-INTERFACTING-DIGITAL-SENSOR-WITH-EDGE-DEVELOPMENT-BOARD-ULTRASONIC-AND-PIR-SENSOR-(RASPBERRYPI-PI4)
### NAME : VISHAL P
### DEPARTMENT : AI&DS
### ROLL NO : 212224230306
### DATE OF EXPERIMENT : 12.02.2026

### AIM
To interface a digital sensor (Ultrasonic and PIR) with the Raspberry Pi 4 and control it using Python.

## APPARATUS REQUIRED
Raspberry Pi 4
LED (Light Emitting Diode)
330Ω Resistor
IR Sensor
Breadboard
Jumper Wires
USB Cable
 ## THEORY

![Raspberry Pi Pin](https://github.com/user-attachments/assets/19e5a1e7-cb46-4909-ba59-e4f4560cae03)





 
 
 
 ### FIGURE-01 RASPI PI 4 PINOUT DIAGRAM 


The Raspberry Pi 4 Model B is built around a Broadcom BCM2711 system-on-chip that integrates a quad-core ARM Cortex-A72 (64-bit) CPU, VideoCore VI GPU, memory controller, and peripheral interfaces, forming a compact yet complete computer architecture where the SoC connects internally to RAM, USB 3.0 controller, Gigabit Ethernet, HDMI display, and wireless modules. Its 40-pin GPIO header provides a flexible pin configuration consisting of power pins (5 V and 3.3 V), multiple ground pins, and general-purpose input/output pins that operate at 3.3 V logic and can be programmed for digital I/O or alternate functions. Key alternate functions include I²C (SDA, SCL) for sensor communication, SPI (MOSI, MISO, SCLK, CS) for high-speed peripheral interfacing, UART (TX, RX) for serial communication, and PWM for control applications.  For communication, I2C (SDA, SCL), SPI (MOSI, MISO, SCK), and UART (TX, RX) interfaces are mapped across different GPIO pins, allowing seamless connectivity with sensors and peripherals. All GPIO pins support PWM (Pulse Width Modulation), making it useful for motor control, LED brightness adjustment, and sound applications. The BOOTSEL button enables USB mass storage mode for firmware flashing, while the DEBUG pins (SWD interface) provide debugging capabilities. With its low power consumption, flexible GPIO options, and rich interface support, the Raspberry Pi Pico is widely used for IoT, embedded systems, robotics, and automation projects.This architecture and pin multiplexing allow the Raspberry Pi 4 to act as both a general-purpose computing platform and an embedded controller, supporting rapid prototyping, hardware interfacing, and IoT applications.
## Ultrasonic Sensor:
An ultrasonic sensor is a distance-measuring device that uses high-frequency sound waves to detect objects and calculate how far away they are. It works by emitting ultrasonic pulses (typically around 40 kHz) and measuring the time taken for the echo to bounce back after hitting an object. Using the speed of sound in air, the sensor converts this time into distance. Ultrasonic sensors are widely used in robotics, obstacle detection, parking systems, and automation because they provide reliable, contactless measurement regardless of lighting conditions.
<img width="600" height="411" alt="image" src="https://github.com/user-attachments/assets/d879d982-347a-45ff-92c1-b78b57e9fdef" />


## PIR Sensor:
A Passive Infrared (PIR) sensor is a motion detection device that senses changes in infrared radiation emitted by warm objects such as humans or animals. Instead of emitting signals, it passively detects heat variations within its field of view. When a warm body moves across the sensor’s detection zones, it triggers an electrical signal indicating motion. PIR sensors are commonly used in security alarms, automatic lighting systems, and energy-saving smart devices due to their low power consumption and ability to detect human presence effectively.
<img width="428" height="494" alt="image" src="https://github.com/user-attachments/assets/bb6b0f22-33d7-4d63-b5c6-05e6d655e71d" />

## Working Principle:
Experiment 1A
The Ultrasonic sensor Trig pin is connected to one of the GPIO pins of the Raspberry Pi 4.
The Ultrasonic sensor Echo pin is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the take the distance taken echo output and shown in Thingspeak cloud with current status and Console.
CIRCUIT DIAGRAM
Connect the Vcc of the Ultrasonic sensor +5V in Raspberrry Pi4.
Connect the Gnd of the Ultrasonic sensor Gnd in Raspberrry Pi4.
Connect the Trig pin to any one GPIO.
Connect the Echo pin to any one GPIO.


Experiment 1B
The IR sensor is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the PIR sensor value based on the motion detected and shown in Thingspeak and console.
CIRCUIT DIAGRAM
Connect the PIR sensor Vcc to any +5V.
Connect the PIR sensor GND to any GND.
Connect the PIR sensor OUT to any one GPIO. 

## Experiment 3A(Ultrasonic Sensor):
## PROGRAM:
```
import RPi.GPIO as GPIO
import time
import requests

# ThingSpeak settings
API_KEY = "VVWU73OPRWSIBGCH"
THINGSPEAK_URL = "https://api.thingspeak.com/update"

# GPIO pins
TRIG = 23
ECHO = 24

GPIO.setmode(GPIO.BCM)
GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)

def get_distance():
    GPIO.output(TRIG, False)
    time.sleep(0.5)

    # Trigger pulse
    GPIO.output(TRIG, True)
    time.sleep(0.00001)
    GPIO.output(TRIG, False)

    while GPIO.input(ECHO) == 0:
        pulse_start = time.time()

    while GPIO.input(ECHO) == 1:
        pulse_end = time.time()

    pulse_duration = pulse_end - pulse_start
    distance = pulse_duration * 17150
    distance = round(distance, 2)

    return distance

try:
    while True:
        distance = get_distance()

        # Console output
        print("distance =", distance, "cm")

        # Text message for ThingSpeak
        status_text = f"distance = {distance} cm"

        # Send data to ThingSpeak
        payload = {
            "api_key": API_KEY,
            "field3": distance,   # numeric for chart
            "status": status_text # text message
        }

        response = requests.get(THINGSPEAK_URL, params=payload)
        print("Sent to ThingSpeak")

        time.sleep(15)

except KeyboardInterrupt:
    GPIO.cleanup()
```

### OUTPUT  (Experiment 3A)
![WhatsApp Image 2026-02-16 at 11 03 54 AM](https://github.com/user-attachments/assets/f15ad79f-5398-4c6a-85f7-b092c217c826)
![WhatsApp Image 2026-02-16 at 11 03 55 AM](https://github.com/user-attachments/assets/dec8e0d2-5ee3-42a0-bc19-565dbee23831)
<img width="1919" height="907" alt="Screenshot 2026-02-12 144917" src="https://github.com/user-attachments/assets/a7002434-f093-4bf0-b52e-c2ccb68b6924" />
<img width="1919" height="903" alt="Screenshot 2026-02-12 144929" src="https://github.com/user-attachments/assets/7d1f3b56-f8dc-4af1-8aae-b986e27a399a" />


## Experiment 3B(PIR Sensor):
## PROGRAM:
```
import RPi.GPIO as GPIO
import time
import requests

WRITE_API_KEY = "VVWU73OPRWSIBGCH"
URL = "https://api.thingspeak.com/update"

PIR_PIN = 17

GPIO.setmode(GPIO.BCM)
GPIO.setup(PIR_PIN, GPIO.IN)

print("PIR Monitoring Started...")
time.sleep(2)

last_state = -1   # store previous state

def update_thingspeak(state):
    data = {
        "api_key": WRITE_API_KEY,
        "field1": state
    }
    try:
        requests.get(URL, params=data)
        print("Uploaded to ThingSpeak:", state)
    except:
        print("Upload Failed")

while True:
    motion = GPIO.input(PIR_PIN)

    if motion != last_state:   # send only if changed
        if motion == 1:
            print("Motion Detected")
            update_thingspeak(1)
        else:
            print("No Motion")
            update_thingspeak(0)

        last_state = motion
        time.sleep(15)  # ThingSpeak delay

    time.sleep(1)
```
## OUTPUT (Experiment 3B):
![WhatsApp Image 2026-02-16 at 11 26 36 AM](https://github.com/user-attachments/assets/7a2d7ce3-629a-4e01-8f9d-145ea29b0dcb)

![WhatsApp Image 2026-02-16 at 11 28 00 AM](https://github.com/user-attachments/assets/7293c8ae-e9e0-42dd-88e5-1e6b79139820)

![WhatsApp Image 2026-02-16 at 11 28 56 AM](https://github.com/user-attachments/assets/4f2b25f7-883b-4913-b176-a3c24ed7d9a0)

 ![WhatsApp Image 2026-02-16 at 11 29 07 AM](https://github.com/user-attachments/assets/c48a2c65-99ca-4b63-93ee-62eb65cf4f66)

## RESULTS
The Ultrasonic sensor and PIR sensor is connected to the Raspberry Pi 4 successfully and the distance and the motion detection is visualised in thingspeak confirming the proper interfacing of a digital output.
