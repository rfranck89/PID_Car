
# PID car
## Description
in this project, we were instructed to design, build, and program a device that uses PID control, with these requirements:

- work together in groups of two
- use only a metro and other standard components in the sigma lab
- use 4 or 6 AA batteries in a battery pack for power
- include a power switch and a LED power indicator
- ensure everything is securely mounted
Every new project:
To commit from VS Code:
If you get an error about user.name and user.email

## Planning 
early design
![early design](https://github.com/rfranck89/PID_Car/assets/71406948/59f59c81-4730-4484-8cd8-582610002258)


## Design

### Bill of Materials
     - 2 motors
     - circuitPython circuit
     - breadboard
     - sensor
     - H bridge
     - battery pack
     - 6 AA battery's
     - LED
     - resistor
     - 3D printer
     - 4 male-female wires
     - 6 male- male wires
     
     
### Wiring Diagram
![Screenshot 2023-05-26 1 13 11 PM](https://github.com/rfranck89/PID_Car/assets/71406948/6848e501-18e5-44a6-84b3-e9e936e7e482)


### Code 

```python
# Jabari Bright & Ryan Franck
# PID car
# The code makes the car move



from simple_pid import PID
import time
import board
import adafruit_hcsr04
sonar = adafruit_hcsr04.HCSR04(trigger_pin=board.D5, echo_pin=board.D6)

# Code to control Hbridge
from time import sleep
from digitalio import DigitalInOut, Direction, Pull
from pwmio import PWMOut
from adafruit_motor import motor as Motor
DEBUG = True  # mode of operation; False = normal, True = debug
OP_DURATION = 5  # operation duration in seconds
drv8833_ain1 = PWMOut(board.D9)
drv8833_ain2 = PWMOut(board.D10)
drv8833_bin1 = PWMOut(board.D11)
drv8833_bin2 = PWMOut(board.D12)
drv8833_sleep = DigitalInOut(board.D3)
motor_a = Motor.DCMotor(drv8833_ain1, drv8833_ain2)
motor_b = Motor.DCMotor(drv8833_bin1, drv8833_bin2)

# print status of motor
def print_motor_status(motor):
    if motor == motor_a:
        motor_name = "A"
    elif motor == motor_b:
        motor_name = "B"
    else:
        motor_name = "Unknown"
    print(f"Motor {motor_name} throttle is set to {motor.throttle}.")

# Basic control of motor
def basic_operations():
    # Drive forward at full throttle
    motor_a.throttle = 1.0
    if DEBUG: print_motor_status(motor_a)
    sleep(OP_DURATION)
    # Coast to a stop
    motor_a.throttle = None
    if DEBUG: print_motor_status(motor_a)
    sleep(OP_DURATION)
    # Drive backwards at 50% throttle
    motor_a.throttle = -0.5
    if DEBUG: print_motor_status(motor_a)
    sleep(OP_DURATION)
    # Brake to a stop
    motor_a.throttle = 0
    if DEBUG: print_motor_status(motor_a)
    sleep(OP_DURATION)
# Main
drv8833_sleep.direction = Direction.OUTPUT
drv8833_sleep.value = True  # enable (turn on) the motor driver
if DEBUG: print("Running in DEBUG mode.  Turn off for normal operation.")

# use this loop to test motor
# while True:
#     basic_operations()  # perform basic motor control operations on motor A

setpoint = 15

# Create PID object
pid = PID(0.2, 0.01, 0.01, setpoint=setpoint, output_limits=(-.5, .5))
dis = 0
prev_dis = 0

while True:
    
    # grabs the current distance
    try:
        dis = sonar.distance
        # print(dis)
    except RuntimeError:
        print("Retrying!")
    time.sleep(0.05)

      # Compute new output from the PID according to the systems current value
    print("Distance: ", dis)
    if dis == 0:
        dis = prev_dis
    else:
        prev_dis = dis

    control = pid(dis)
    print("Control: ", control)
    if abs(dis-setpoint) < .5:
        motor_a.throttle = control
        motor_b.throttle = control
        print("   Control is good!")
    else:
        # Feed the PID output to the system and get its current value
        motor_a.throttle = control
        motor_b.throttle = control

    # ask are we at the setpoint
    # if dis > setpoint:
    #     print('move foward')
    #     motor_a.throttle = 1.0
    #     motor_b.throttle = 1.0

    # elif dis < setpoint:
    #     print('move back')
    #     motor_a.throttle = -0.5
    #     motor_b.throttle = -0.5
    # else:
    #     print("stop")
    #     motor_a.throttle = 0
    #     motor_b.throttle = 0


```

### Build



![PIDcarpic1](https://github.com/rfranck89/PID_Car/assets/71406948/5adda2c4-c269-4d38-a6f6-b488ad919d38)
![PIDcarpic4](https://github.com/rfranck89/PID_Car/assets/71406948/32a47762-aff2-43fb-aa49-eb47a56330de)
![PIdcarpic2](https://github.com/rfranck89/PID_Car/assets/71406948/90ff50dc-0611-4202-b32a-dbba3af40828)
![PIDcarpic3](https://github.com/rfranck89/PID_Car/assets/71406948/8ffee07e-cab4-4491-ac7b-4cbc32062895)
![PIDcarpic5](https://github.com/rfranck89/PID_Car/assets/71406948/3ae7bbef-edcd-42e4-aefc-2ec961c772ff)
![PIDCARPIC6](https://github.com/rfranck89/PID_Car/assets/71406948/c74e6f53-e8a6-40f0-9267-58e1a18de034)



maybe include something about the wiring for the h bridge, with a link to the image on adafruit.

## Final Product
![IMG_4603](https://github.com/rfranck89/PID_Car/assets/71406948/fd39bc8b-bb34-404e-a5eb-b44212b3dbad)
![IMG_4601](https://github.com/rfranck89/PID_Car/assets/71406948/1cc91827-389f-4128-9847-b437e8f49bc5)
![IMG_4602](https://github.com/rfranck89/PID_Car/assets/71406948/3425c312-b369-4460-8f0f-74c04fdae559)
![IMG_4604](https://github.com/rfranck89/PID_Car/assets/71406948/4e628646-0b1b-4a46-8f44-344ec67814d5)
![IMG_4601](https://github.com/rfranck89/PID_Car/assets/71406948/11c7b6b0-4781-4854-a985-86b68ae3eefc)



https://github.com/rfranck89/PID_Car/assets/71406948/09cf27b8-cba5-4fd4-b060-db48174100f3







## Reflection

so the assignment was hard i asked my teachers for help so I recommend asking your teachers for help if you don't know what you're doing, because I kept messing with the PID and I couldnt get it to run smoothly so I asked the teacher for help, I also recommend working with a partner because this will take way too long for you to do it alone. also the car turns when it goes backward because of the front so if I could go back and redo this project i would make it a four wheeler instead of three.
