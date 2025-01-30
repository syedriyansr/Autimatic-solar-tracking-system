# Autimatic-solar-tracking-system
Mini project based on Arduino, Esp8266 and esp32Program
#include <Servo.h>

Servo myServo;  // Create Servo object

int ldrLeft = A0;  // LDR 1 (Left)
int ldrRight = A1; // LDR 2 (Right)
int servoPin = 9;  // Servo motor connected to pin 9

int pos = 90; // Initial servo position at center (90 degrees)
int threshold = 10; // Threshold difference to move servo

void setup() {
    myServo.attach(servoPin);  // Attach the servo
    myServo.write(pos);  // Set initial position
    pinMode(ldrLeft, INPUT);
    pinMode(ldrRight, INPUT);
    Serial.begin(9600);
}

void loop() {
    int leftValue = analogRead(ldrLeft);  // Read LDR 1
    int rightValue = analogRead(ldrRight); // Read LDR 2

    int diff = leftValue - rightValue; // Calculate difference

    Serial.print("Left: "); Serial.print(leftValue);
    Serial.print("  Right: "); Serial.print(rightValue);
    Serial.print("  Diff: "); Serial.println(diff);

    if (diff > threshold) { // If left side is brighter
        if (pos < 180) {
            pos += 2;  // Move servo right
        }
    } 
    else if (diff < -threshold) { // If right side is brighter
        if (pos > 0) {
            pos -= 2;  // Move servo left
        }
    }

    myServo.write(pos);  // Move servo to new position
    delay(100); // Small delay for stability
}
