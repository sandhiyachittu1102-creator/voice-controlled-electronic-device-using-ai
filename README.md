# voice-controlled-electronic-device-using-ai
AI-based voice control for electronic devices using Python and Arduino/ESP32.

# ==========================================
# VOICE-CONTROLLED ELECTRONIC DEVICE USING AI
# ==========================================

# PART 1: PYTHON CODE
# Save this section as main.py

import speech_recognition as sr
import serial
import time

# Change COM3 to your Arduino port
arduino = serial.Serial('COM3', 9600, timeout=1)
time.sleep(2)

recognizer = sr.Recognizer()

print("AI Voice Control System Started!")
print("Say: Light On, Light Off, Fan On, Fan Off, or Exit")

try:
    while True:
        try:
            with sr.Microphone() as source:
                print("Listening...")
                recognizer.adjust_for_ambient_noise(
                    source, duration=0.5
                )
                audio = recognizer.listen(source, timeout=5)

            command = recognizer.recognize_google(audio).lower()
            print("Recognized:", command)

            if "light on" in command:
                arduino.write(b'1')
                print("Light ON")

            elif "light off" in command:
                arduino.write(b'2')
                print("Light OFF")

            elif "fan on" in command:
                arduino.write(b'3')
                print("Fan ON")

            elif "fan off" in command:
                arduino.write(b'4')
                print("Fan OFF")

            elif "exit" in command:
                print("System stopped.")
                break

            else:
                print("Command not recognized. Try again.")

        except sr.UnknownValueError:
            print("Sorry, I could not understand.")
        except sr.RequestError:
            print("Speech recognition service unavailable.")
        except sr.WaitTimeoutError:
            print("No speech detected. Try again.")

except KeyboardInterrupt:
    print("\nSystem stopped by user.")

finally:
    arduino.close()


# ==========================================
# PART 2: ARDUINO CODE
# ==========================================
# Copy this section separately into Arduino IDE
# Save as voice_control.ino

/*
void setup() {
  Serial.begin(9600);

  pinMode(8, OUTPUT);  // Light control
  pinMode(9, OUTPUT);  // Fan control

  digitalWrite(8, LOW);
  digitalWrite(9, LOW);
}

void loop() {
  if (Serial.available() > 0) {
    char command = Serial.read();

    if (command == '1') {
      digitalWrite(8, HIGH);
    }
    else if (command == '2') {
      digitalWrite(8, LOW);
    }
    else if (command == '3') {
      digitalWrite(9, HIGH);
    }
    else if (command == '4') {
      digitalWrite(9, LOW);
    }
  }
}
*/
