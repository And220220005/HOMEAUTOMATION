# HOMEAUTOMATION
Home Automation with Bluetooth using Adriuno
#include <Servo.h> #include <SoftwareSerial.h>

// Bluetooth module SoftwareSerial BT(10, 11); // RX, TX

// Sensors and Actuators const int pirPin = 2; const int ldrPin = A0; const int trigPin = 3; const int echoPin = 4; const int relay1 = 5; const int relay2 = 6; const int buzzer = 7; Servo doorServo;

void setup() { pinMode(pirPin, INPUT); pinMode(ldrPin, INPUT); pinMode(trigPin, OUTPUT); pinMode(echoPin, INPUT); pinMode(relay1, OUTPUT); pinMode(relay2, OUTPUT); pinMode(buzzer, OUTPUT); doorServo.attach(9);

BT.begin(9600);
Serial.begin(9600);

}

void loop() { if (BT.available()) { char command = BT.read(); Serial.println(command);

if (command == '1') {
        digitalWrite(relay1, HIGH);
    } else if (command == '0') {
        digitalWrite(relay1, LOW);
    } else if (command == '2') {
        digitalWrite(relay2, HIGH);
    } else if (command == '3') {
        digitalWrite(relay2, LOW);
    } else if (command == 'O') {
        doorServo.write(90); // Open door
    } else if (command == 'C') {
        doorServo.write(0); // Close door
    } else if (command == 'B') {
        digitalWrite(buzzer, HIGH);
        delay(500);
        digitalWrite(buzzer, LOW);
    }
}

int ldrValue = analogRead(ldrPin);
if (ldrValue < 500) {
    digitalWrite(relay1, HIGH); // Turn on light
} else {
    digitalWrite(relay1, LOW);
}

int pirValue = digitalRead(pirPin);
if (pirValue == HIGH) {
    digitalWrite(buzzer, HIGH);
    delay(1000);
    digitalWrite(buzzer, LOW);
}

long duration;
int distance;
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);
duration = pulseIn(echoPin, HIGH);
distance = duration * 0.034 / 2;

if (distance < 10) {
    doorServo.write(90);
}
delay(500);

}
