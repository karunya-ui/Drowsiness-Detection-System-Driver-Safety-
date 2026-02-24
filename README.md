# Drowsiness-Detection-System-Driver-Safety-

#include <Arduino.h>
#include "config.h"

unsigned long eyeClosedStart = 0;
bool eyeClosed = false;

void setup() {
    Serial.begin(115200);

    pinMode(IR_SENSOR_PIN, INPUT);
    pinMode(BUZZER_PIN, OUTPUT);

    digitalWrite(BUZZER_PIN, LOW);
    Serial.println("Drowsiness Detection System Started...");
}

void loop() {
    int sensorValue = digitalRead(IR_SENSOR_PIN);

    if (sensorValue == LOW) {  // Eye Closed
        if (!eyeClosed) {
            eyeClosed = true;
            eyeClosedStart = millis();
        }

        if (millis() - eyeClosedStart > CLOSED_TIME_THRESHOLD) {
            digitalWrite(BUZZER_PIN, HIGH);
            Serial.println("DROWSINESS DETECTED! Wake up!");
        }
    }
    else {  // Eye Open
        eyeClosed = false;
        digitalWrite(BUZZER_PIN, LOW);
    }

    delay(100);
}
