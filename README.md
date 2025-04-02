# Simple-mobile-controlled-arduino-car
This gives a glimpse of how to built a simple arduino car controlling it via bluetooth
# Arduino Bluetooth-Controlled Car  

Arduino Bluetooth-Controlled Car with Adafruit Motor Shield  

## 📌 Project Overview  
This project is a mobile-controlled car using an *Arduino Uno* and an *Adafruit Motor Shield. The car receives commands via **Bluetooth (HC-05)* and moves accordingly.  

## 🛠 Components Used  
- *Arduino Uno/Nano*  
- *Adafruit Motor Shield (V1)*  
- *HC-05 Bluetooth Module*  
- *4x DC Motors*  
- *12V or 9V Battery Pack*  
- *Chassis for Car*  
- *Jumper Wires*  

## 🔌 Wiring Connections  
### *1️⃣ Motor Connections (Motor Shield)*
| *Motor*  | *Motor Shield Terminal* |
|------------|--------------------------|
| *Front Left Motor*  | M1 (Motor 1) |
| *Front Right Motor* | M2 (Motor 2) |
| *Rear Left Motor*  | M3 (Motor 3) |
| *Rear Right Motor*  | M4 (Motor 4) |

### *2️⃣ Bluetooth Module Connections*
| *HC-05 Pin* | *Arduino Pin* |
|--------------|--------------|
| TX | RX (Pin 10) |
| RX | TX (Pin 11) |
| VCC | 5V |
| GND | GND |

---

## 📲 Mobile App Control  
Use a *Bluetooth Terminal App* to send commands:  
- F → Move *Forward*  
- B → Move *Backward*  
- L → Turn *Left*  
- R → Turn *Right*  
- S → *Stop*

## 🔧 Code (Arduino Sketch)  
```cpp
#include <AFMotor.h>
#include <SoftwareSerial.h>

// Define motors using the Adafruit Motor Shield
AF_DCMotor motor1(1); // Front Left Motor
AF_DCMotor motor2(2); // Front Right Motor
AF_DCMotor motor3(3); // Rear Left Motor
AF_DCMotor motor4(4); // Rear Right Motor

// Define Bluetooth module pins
SoftwareSerial BT(10, 11); // RX, TX for HC-05

void setup() {
    Serial.begin(9600);
    BT.begin(9600);
    
    // Set initial motor speed (0-255)
    motor1.setSpeed(200);
    motor2.setSpeed(200);
    motor3.setSpeed(200);
    motor4.setSpeed(200);
}

void loop() {
    if (BT.available()) {
        char command = BT.read();
        Serial.println(command);

        if (command == 'F') {  // Move Forward
            moveForward();
        } else if (command == 'B') {  // Move Backward
            moveBackward();
        } else if (command == 'L') {  // Turn Left
            turnLeft();
        } else if (command == 'R') {  // Turn Right
            turnRight();
        } else if (command == 'S') {  // Stop
            stopCar();
        }
    }
}

// Function to move the car forward
void moveForward() {
    motor1.run(FORWARD);
    motor2.run(FORWARD);
    motor3.run(FORWARD);
    motor4.run(FORWARD);
}

// Function to move the car backward
void moveBackward() {
    motor1.run(BACKWARD);
    motor2.run(BACKWARD);
    motor3.run(BACKWARD);
    motor4.run(BACKWARD);
}

// Function to turn left
void turnLeft() {
    motor1.run(BACKWARD);
    motor2.run(FORWARD);
    motor3.run(BACKWARD);
    motor4.run(FORWARD);
}

// Function to turn right
void turnRight() {
    motor1.run(FORWARD);
    motor2.run(BACKWARD);
    motor3.run(FORWARD);
    motor4.run(BACKWARD);
}

// Function to stop the car
void stopCar() {
    motor1.run(RELEASE);
    motor2.run(RELEASE);
    motor3.run(RELEASE);
    motor4.run(RELEASE);
}
