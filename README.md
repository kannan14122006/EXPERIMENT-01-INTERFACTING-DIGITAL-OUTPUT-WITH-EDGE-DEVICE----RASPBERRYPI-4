# EXPERIMENT-01-INTERFACTING-DIGITAL-OUTPUT-WITH-EDGE-DEVICE---(RASPBERRYPI-PI4)
### NAME : KANNAN r
### DEPARTMENT : AIML
### REG NO : 212224240072
### DATE OF EXPERIMENT : 04.02.2026

### AIM
To interface a digital output device (LED) with the Raspberry Pi 4 and control it using Python.

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


## Working Principle:
Experiment 1A
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to GP15 via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).

Experiment 1B
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The IR sensor is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF based on the IR sensor.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to any one GPIO via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).
Connect the IR sensor Vcc to any +5V.
Connect the IR sensor GND to any GND.
Connect the IR sensor OUT to any one GPIO. 

## Experiment 1A – LED Blinking using Raspberry Pi 4

## PROGRAM(python):
```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "VVWU73OPRWSIBGCH"
CHANNEL_ID = 3249431
THINGSPEAK_URL = "https://api.thingspeak.com/update"

# Set GPIO numbering mode
GPIO.setmode(GPIO.BCM)

# Define LED pin
LED_PIN = 18

# Set GPIO18 as output
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=VVWU73OPRWSIBGCH&field1={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)

try:
    while True:
        # LED ON
        GPIO.output(LED_PIN, GPIO.HIGH)
        print("LED ON")
        send_to_thingspeak(1)
        time.sleep(15)

        # LED OFF
        GPIO.output(LED_PIN, GPIO.LOW)
        print("LED OFF")
        send_to_thingspeak(0)
        time.sleep(15)

except KeyboardInterrupt:
    print("Program stopped")

finally:
    GPIO.cleanup()
```
## OUTPUT 1A:
## LED OFF:
![WhatsApp Image 2026-02-04 at 11 23 14 AM](https://github.com/user-attachments/assets/39ed0413-0320-48b4-985b-ca1726bcdd36)

![WhatsApp Image 2026-02-04 at 11 24 50 AM](https://github.com/user-attachments/assets/55259153-4def-43e9-97b4-23e6dd560203)

<img width="1919" height="901" alt="Screenshot 2026-02-04 111336" src="https://github.com/user-attachments/assets/a87e6882-d87f-42dd-8052-da32d7e590f0" />

# LED ON:

![WhatsApp Image 2026-02-04 at 11 23 15 AM](https://github.com/user-attachments/assets/b54ae90b-d261-41f5-adff-c19d0119c4cf)

![WhatsApp Image 2026-02-04 at 11 24 54 AM](https://github.com/user-attachments/assets/de70ddd6-2eed-45bb-abe7-7b4588910071)

<img width="1911" height="901" alt="Screenshot 2026-02-04 111351" src="https://github.com/user-attachments/assets/cb92bc54-04d6-4db1-acb6-fc15a1e6d2c4" />

## Experiment 1B – LED Control using IR Sensor

## PROGRAM(python):

```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "VVWU73OPRWSIBGCH"
CHANNEL_ID = 3249431
THINGSPEAK_URL = "https://api.thingspeak.com/update"



# Pin setup
SENSOR_PIN = 23   # Input from sensor
LED_PIN = 18      # Output to LED

# GPIO mode
GPIO.setmode(GPIO.BCM)

# Setup pins
GPIO.setup(SENSOR_PIN, GPIO.IN)
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=VVWU73OPRWSIBGCH&field2={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)


print("Sensor + LED system running...")

try:
    while True:
        sensor_value = GPIO.input(SENSOR_PIN)

        if sensor_value == 0:   # Many IR sensors give LOW when object detected
            print("Object Detected! LED ON")
            GPIO.output(LED_PIN, GPIO.HIGH)
            send_to_thingspeak(1)

            time.sleep(15)
        else:
            print("No Object. LED OFF")
            GPIO.output(LED_PIN, GPIO.LOW)
            send_to_thingspeak(0)

            time.sleep(15)

        time.sleep(0.1)

except KeyboardInterrupt:
    print("Stopped by user")

finally:
    GPIO.cleanup()
```
## OUTPUT 1B:
# LED OFF(NO OBJECT DETECTED):

![WhatsApp Image 2026-02-04 at 11 56 09 AM](https://github.com/user-attachments/assets/3c727e0f-66b5-4dc0-99b0-7fa4f795ab66)

 ![WhatsApp Image 2026-02-04 at 11 56 08 AM](https://github.com/user-attachments/assets/42fe85a3-b871-4291-a2b7-94d7f95469e7)

<img width="1916" height="912" alt="Screenshot 2026-02-04 115536" src="https://github.com/user-attachments/assets/ed1c21b8-4986-4ef0-8b1d-e9cdac60f444" />

 # LED ON(OBJECT DETECTED):

 ![WhatsApp Image 2026-02-04 at 11 56 09 AM (1)](https://github.com/user-attachments/assets/9b484547-9f88-4b49-b5db-582d4abc516e)
 ![WhatsApp Image 2026-02-04 at 11 56 09 AM (2)](https://github.com/user-attachments/assets/669da119-13ec-49d9-bfcd-0e5ac3836c99)
 <img width="1914" height="913" alt="Screenshot 2026-02-04 115522" src="https://github.com/user-attachments/assets/0bd19e1e-6a4e-4665-a221-9bd6f8b6e0ad" />


## RESULTS
The LED connected to the Raspberry Pi 4 successfully turns ON and OFF at  user defined time  confirming the proper interfacing of a digital output.
