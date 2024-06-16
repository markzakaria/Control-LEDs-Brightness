# Control-LEDs-Brightness
#include <BluetoothSerial.h>

// LED pins

const int ledPin1 = 5;

const int ledPin2 = 4;

const int ledPin3 = 15;

BluetoothSerial SerialBT;

void setup() {

  // Initialize serial communication
  
  Serial.begin(9600);

  // Set the LED pins as outputs
  
  pinMode(ledPin1, OUTPUT);
  pinMode(ledPin2, OUTPUT);
  pinMode(ledPin3, OUTPUT);

  // Initialize Bluetooth serial communication
  
  SerialBT.begin("LED_Control");

  // Wait until Bluetooth is connected
  
  while (!SerialBT.connected()) {
  
   delay(100);
  }
}

void loop() {
  // Check if data is available from Bluetooth
  
  if (SerialBT.available()) {
  
  // Read the incoming brightness value
  
    int brightness = SerialBT.parseInt();

  // Control the LEDs based on brightness value
  
   if (brightness <= 250) {
      ledcWrite(ledPin1, brightness);
      analogWrite(ledPin2, 0);
      analogWrite(ledPin3, 0);
    } 
    
  else if (brightness <= 500) {
      analogWrite(ledPin1, 250);
      analogWrite(ledPin2, brightness - 250);
      analogWrite(ledPin3, 0);
    } 
    
  else {
      analogWrite(ledPin1, 250);
      analogWrite(ledPin2, 250);
      analogWrite(ledPin3, brightness - 500);
    }

  // Print the brightness value to the serial monitor
  
  Serial.println(brightness);
  }

  // Delay for stability
  delay(10);
}
