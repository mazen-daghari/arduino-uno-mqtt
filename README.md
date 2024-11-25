# arduino-uno-mqtt
 creating a simple project where two Arduino Uno R3 boards communicate wirelessly using the nRF24L01 modules. 
Let's create a simple project where two Arduino Uno R3 boards communicate wirelessly using the nRF24L01 modules. One Arduino will act as the transmitter, sending data to the other Arduino, which will act as the receiver.

Components Needed:
2 x Arduino Uno R3

2 x nRF24L01 modules

Jumper wires

Breadboard (optional)

Wiring the nRF24L01 Modules to the Arduino Uno
Connect the nRF24L01 modules to both Arduinos as follows:

VCC to 3.3V

GND to GND

CE to digital pin 9

CSN to digital pin 10

SCK to digital pin 13

MOSI to digital pin 11

MISO to digital pin 12

Transmitter Code
This code will send a "Hello World" message from the transmitter Arduino to the receiver Arduino.

cpp
#include <SPI.h>
#include <RF24.h>

RF24 radio(9, 10); // CE, CSN pins
const byte address[6] = "00001";

void setup() {
  Serial.begin(9600);
  radio.begin();
  radio.openWritingPipe(address);
  radio.setPALevel(RF24_PA_MIN);
  radio.stopListening();
}

void loop() {
  const char text[] = "Hello World";
  radio.write(&text, sizeof(text));
  Serial.println("Sent: Hello World");
  delay(1000);
}
Receiver Code
This code will receive the "Hello World" message from the transmitter Arduino and print it to the Serial Monitor.

cpp
#include <SPI.h>
#include <RF24.h>

RF24 radio(9, 10); // CE, CSN pins
const byte address[6] = "00001";

void setup() {
  Serial.begin(9600);
  radio.begin();
  radio.openReadingPipe(0, address);
  radio.setPALevel(RF24_PA_MIN);
  radio.startListening();
}

void loop() {
  if (radio.available()) {
    char text[32] = "";
    radio.read(&text, sizeof(text));
    Serial.println(text);
  }
}
Explanation:
Transmitter Code:

Setup: Initializes the Serial Monitor, starts the nRF24L01 module, and sets it to writing mode.

Loop: Sends the "Hello World" message every second and prints a confirmation to the Serial Monitor.

Receiver Code:

Setup: Initializes the Serial Monitor, starts the nRF24L01 module, and sets it to listening mode.

Loop: Checks if data is available, reads the incoming message, and prints it to the Serial Monitor.

Uploading the Code:
Connect the first Arduino (transmitter) to your computer and upload the transmitter code.

Connect the second Arduino (receiver) to your computer and upload the receiver code.

Testing:
Open the Serial Monitor for both Arduinos.

You should see the "Hello World" message being sent from the transmitter and received by the receiver.

This setup demonstrates basic wireless communication between two Arduino Uno boards using the nRF24L01 modules
