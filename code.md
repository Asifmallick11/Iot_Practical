# Practical 5 - Arduino Loops

- Loops Code

```cpp
void setup() { Serial.begin(9600);
Serial.println("*** This message will only be displayed on start or reset. ***"); delay(2000);
}
void loop() {
Serial.println("-- Arduino now at top of main loop. --"); Serial.println("	");
delay(2000);
Serial.println("Executing instructions in main loop."); delay(2000);
Serial.println("Arduino now at bottom of main loop.\r\n");
}
```

- Using a Variable 

```cpp
void setup() { int count;

Serial.begin(9600);
count = 0; Serial.println(count); count = 1; Serial.println(count); count = 2; Serial.println(count);
}

void loop() {
}
```

- Printing a Float Value

```cpp 
void setup() { float average;
Serial.begin(9600); average = 12.3299; Serial.println(average);
Serial.println(average, 4);
}
void loop() { }
```

---

# Practical 6 - Blinking Arduino Uno 3

```cpp
void setup ()
{
pinMode(12, OUTPUT);
}
void loop()
{
// turn the LED on (HIGH is the voltage level) 
digitalWrite(12, HIGH); 
delay(2000); 
// Wait for 1000 millisecond(s) // turn the LED off by making the voltage LOW 
digitalWrite(12, LOW); 
delay(2000); // Wait for 1000 millisecond(s)
}
```

- Connection 

LED Short Leg on GND 
LED Long Leg on 12

---

# Practical 7 - Blinking LED using Arduino Uno and BreadBoard

- Code 

```cpp
void setup ()
{
pinMode(13, OUTPUT); pinMode(12, OUTPUT);
}
void loop()
{
digital(13, HIGH); delay(2000); digital(12, HIGH); delay(3000); digital(13, LOW); delay(2000); digital(12, LOW); delay(3000);
}
```

- Connections 

|Arduino uno Pin |	Component Connection |
|----------------|-----------------------|
|D2 |   LED 1 Anode (long leg)  |
|D3	|   LED 2 Anode (long leg)  |
|GND Resistor | LED 1 Cathode (short leg) |
|GND Resistor | LED 2 Cathode (short leg) |

---

# Practical 8 - To Calculate Distance using Ultrasonic Sensor

- Code 

```cpp
#define TRIG_PIN 9
#define ECHO_PIN 10
#define LED_PIN 13 // Built-in LED pin on most Arduino boards
void setup() {
Serial.begin(9600); // Start serial communication 
pinMode(TRIG_PIN, OUTPUT); 
pinMode(ECHO_PIN, INPUT); 
pinMode(LED_PIN, OUTPUT); // LED output
}
void loop() {
// Clear the TRIG_PIN 
digitalWrite(TRIG_PIN, LOW); delayMicroseconds(2);
// Send 10 microsecond pulse 
digitalWrite(TRIG_PIN, HIGH); delayMicroseconds(10); digitalWrite(TRIG_PIN, LOW);
// Read the echo time 
long duration = pulseIn(ECHO_PIN, HIGH);
// Calculate distance in cm 
float distance = duration * 0.034 / 2;
// Print distance
Serial.print("Distance: ");
Serial.print(distance);
Serial.println(" cm");
// LED blinking logic: Blink when object is within 10 cm 
if (distance <= 10) {
digitalWrite(LED_PIN, HIGH); delay(100); digitalWrite(LED_PIN, LOW); delay(100);
} else { digitalWrite(LED_PIN, LOW);
}
delay(500); // Wait before next reading
}
```

- Connection Table 

Pin on Ultrasonic Pin on

|Component	  |  Sensor	  |  Arduino	|    Description    |
|-------------|-----------|-------------|-------------------|
|Ultrasonic Sensor	  |      VCC	|        5V	     |   Power supply to sensor|

|UltrasonicSensor	  |      GND	  |      GND	|        Ground connection|

|UltrasonicSensor	  |      TRIG	   | 9	      |  Trigger pin to send ultrasonic pulses|

| Ultrasonic Sensor	   |     ECHO	|    10	    |    Echo pin to receive reflected pulse|

|Pin on Ultrasonic  | Pin on Component  |  Description Sensor Arduino|
|-------------------|-------------------|-----------------------------|
|LED                | (Anode , long leg)|13 pin led control pin (through positive current limiting resistor eg 220ohm)
|LED                | Negative(Cathode,short leg)|GND pin gournd for LED|

---


# Practical 9 - Design and implement a digital lock system using a 4x4 keypad and Arduino

- Code

```cpp 
#include <Keypad.h>
// Keypad configuration 
const byte ROWS = 4; 
const byte COLS = 4;
char keys[ROWS][COLS] = {
{'1','2','3','A'}, 
{'4','5','6','B'},
{'7','8','9','C'},
{'*','0','#','D'}
};
byte rowPins[ROWS] = {9, 8, 7, 6};
 byte colPins[COLS] = {5, 4, 3, 2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

// Preset password
String presetPassword = "1234";
String enteredPassword = "";
// Output pin (LED or buzzer) 
const int lockPin = 13;
void setup() {
pinMode(lockPin, OUTPUT);
digitalWrite(lockPin, LOW);
Serial.begin(9600);
Serial.println("Enter 4-digit password:");
}
void loop() {
char key = keypad.getKey();
if (key) {
Serial.print(key);
if (key == '#') { // '#' acts as "Enter"
if (enteredPassword == presetPassword)
 { Serial.println(" -> Access Granted"); 
digitalWrite(lockPin, HIGH); // Turn on LED/buzzer
  delay(2000);  // Keep ON for 2 sec
digitalWrite(lockPin, LOW);
} else {
Serial.println(" -> Access Denied"); 
for (int i = 0; i < 3; i++) {
digitalWrite(lockPin, HIGH); 
delay(300); 
digitalWrite(lockPin, LOW);
 delay(300);
}
}
// Reset after each attempt enteredPassword = "";
Serial.println("Enter 4-digit password:");
}
else if (key == '*') { // '*' acts as reset/clear 
enteredPassword = "";
Serial.println(" -> Cleared");
}
else {
// Store pressed key
 if (enteredPassword.length() < 4) {
enteredPassword += key;
}
}
}
}
```

- Connection 

Connect from D2 to D9 to ardiuno from D to * key 

---

# Practical 10 - Control an LED using Arduino Uno and HC-05 Bluetooth Module with a Mobile App

- Code 

```cpp
#include <SoftwareSerial.h>

// SoftwareSerial pins for HC-05
SoftwareSerial BT(10, 11); // RX, TX  <-- must be on its own line!

int ledPin = 13;    // Built-in LED
String input = "";  // Buffer for incoming Bluetooth data

void setup() {
  pinMode(ledPin, OUTPUT);  // LED as output
  Serial.begin(9600);       // Serial Monitor
  BT.begin(9600);           // HC-05 default baud
  Serial.println("Bluetooth LED Control Ready");
}

void loop() {
  while (BT.available()) {        // Check if data is available
    char c = BT.read();

    if (c == '\n' || c == '\r') { // End of command
      input.trim();               // Remove spaces/newlines
      Serial.print("Command: ");
      Serial.println(input);

      // Control LED
      if (input == "1" || input == "ON") {
        digitalWrite(ledPin, HIGH);
        Serial.println("LED ON");
      }
      else if (input == "0" || input == "OFF") {
        digitalWrite(ledPin, LOW);
        Serial.println("LED OFF");
      }

      input = ""; // Clear buffer
    }
    else {
      input += c; // Build command string
    }
  }
}
```

- Connection 1 Table 

|HC-05 Pin	        | Arduino Connection	        | Notes HC-05 Pin |
|------------------|--------------------------------|-----------------|
|VCC	           |     5V	                    |    Power supply from VCC Arduino|

|GND	           |     GND	             |           Common ground	GND|

|TX	          |      Pin 10 (Arduino RX)	|        SoftwareSerial RX (direct TX connection)|

|RX	             |   Pin 11 (Arduino TX)	|        SoftwareSerial TX (direct connection , no resistor for RX hobby use)|

|HC-05 Pin	   |     Arduino Connection	   |     Notes HC-05 Pin|

|VCC	        |        5V	                |        Power supply from VCC Arduino|

- Connection 2 Table

Pin 13 Ardiuno to LED anode (Small pin)
Pin ground to LED cathode (Long pin)
Connect LED to Ardiuno pin 13 Long terminal of LED anode 

---

# Practical 11 - Develop a system where the IR sensor detects objects passing in front of it (like counting people entering a room). The Arduino should increment a counter and send the updated count to a smartphone via Bluetooth . connection details for interfacing the Arduino, IR Sensor, and HC-05 Bluetooth module

- Code 

```cpp
#include <SoftwareSerial.h>
// Define Bluetooth module pins
SoftwareSerial BT(10, 11); // RX | TX
// IR sensor pin const 
int IR_PIN = 2;
// Variables for counting volatile
 int count = 0; 
int lastSensorState = HIGH;
 // assuming HIGH when no object
void setup() {
pinMode(IR_PIN, INPUT);
BT.begin(9600);	// Bluetooth default baud rate
Serial.begin(9600);	// For debugging (optional)
BT.println("People Counter Ready!");
Serial.println("System Started. Waiting for objects...");
}
void loop() {
int currentState = digitalRead(IR_PIN);
// Detect falling edge: Object passed
if (lastSensorState == HIGH && currentState == LOW) {
Count++;
 sendCount();
 delay(300);
 // small debounce delay to avoid multiple counts for one pass
 }
lastSensorState = currentState;
}
void sendCount() {
BT.print("Count: ");
BT.println(count);
Serial.print("Count updated: ")
; Serial.println(count);
}
```

- Connection 

1. IR Sensor to Arduino Connection

|   IR Sensor Pin	        | Arduino Connection |
|---------------------------|---------------------|
| VCC	                    | 5V                   |
|GND   	                    |     GND             |
| OUT	                    | Digital Pin 2         |

The OUT pin sends HIGH/LOW signals when an object is detected.

2. HC-05 Bluetooth Module to Arduino

|HC-05 Pin	     |       Arduino Connection|
|----------------|-------------------------|
|VCC	           |         5V|
|GND	          |          GND|
|TXD	      |              Pin 10 (Arduino RX via SoftwareSerial)|
|RXD	         |           Pin 11 (Arduino TX via SoftwareSerial, use voltage divider)|

Important Note (Very Important)
1) The HC-05 RX pin works on 3.3V, but Arduino gives 5V output.
2) So, use a voltage divider between Arduino Pin 11 → HC-05 RX.

Voltage Divider Setup:
1) Arduino Pin 11 → 1kΩ resistor → HC-05 RX
2) HC-05 RX → 2kΩ resistor → GND

---

# Practical 12 - Digital Temperature Display Using Arduino, Thermistor, and TM1637 4-Digit Display and Display using counter

- Code

```cpp
#include <TM1637Display.h>
#include <math.h>

// TM1637 Pins
#define CLK 4
#define DIO 3

TM1637Display display(CLK, DIO);

// Thermistor Pin
#define THERMISTOR_PIN A0

// Thermistor Constants
#define SERIES_RESISTOR 10000
#define NOMINAL_RESISTANCE 10000
#define NOMINAL_TEMPERATURE 25
#define B_COEFFICIENT 3950

// Counter Variables
int counter = 0;
bool thresholdCrossed = false;

void setup() {
 Serial.begin(9600);
 display.setBrightness(0x0f);  // Max brightness
 Serial.println("Temperature System Started");
}

void loop() {
 int adcValue = analogRead(THERMISTOR_PIN);

 // Avoid division by zero
 if (adcValue == 0) return;

 // Convert ADC value to resistance
 float resistance = SERIES_RESISTOR / (1023.0 / adcValue - 1);

 // Apply Steinhart-Hart Equation
 float steinhart;
 steinhart = resistance / NOMINAL_RESISTANCE;
 steinhart = log(steinhart);
 steinhart /= B_COEFFICIENT;
 steinhart += 1.0 / (NOMINAL_TEMPERATURE + 273.15);
 steinhart = 1.0 / steinhart;
 steinhart -= 273.15;

 float temperature = steinhart;

 // Print to Serial Monitor
 Serial.print("Temperature: ");
 Serial.print(temperature);
 Serial.println(" °C");

 // Display temperature (integer)
 display.showNumberDec((int)temperature);

 // Counter Logic (if temp > 30°C)
 if (temperature > 30 && !thresholdCrossed) {
   counter++;
   thresholdCrossed = true;

   Serial.print("Counter: ");
   Serial.println(counter);
 }

 if (temperature <= 30) {
   thresholdCrossed = false;
 }

 delay(1000);
}

```

- Connections 

Thermistor Connection (Voltage Divider)

|Component	         |           Connection |
|--------------------|----------------------|
|Thermistor one leg	 |           5V         |
|Thermistor other leg|	        A0 + one side of 10k resistor |
|10k resistor other side|	        GND         |

👉 A0 reads voltage to calculate temperature.

TM1637 Display Connection

|TM1637 Pin         |	    Arduino     |
|-------------------|-------------------|
|VCC                |       5V          |
|GND                |       GND         |
|DIO                |       Pin 3       |
|CLK	            |       Pin 4       |

Working Concept

●Thermistor senses temperature
●Arduino converts analog value → temperature (°C)
●TM1637 displays temperature
●Counter increments every time temperature crosses a threshold (e.g., 30°C) 

---

# Practical 13 - To interface a TM1637 4-digit display with Arduino and display numbers (like a counter).

- Code

```cpp
#include <TM1637Display.h>

// Define pins
#define CLK 4
#define DIO 3

TM1637Display display(CLK, DIO);

int count = 0;

void setup() {
 display.setBrightness(0x0f); // Set brightness
}

void loop() {
 display.showNumberDec(count); // Display number

 count++; // Increment counter

 if (count > 9999) {
   count = 0; // Reset after 9999
 }

 delay(1000); // 1 second delay
}
```

- Connections

|TM1637 Pin	         |   Arduino|
|------------------|--------------|
|VCC	          |          5V|
|GND	           |         GND|
|DIO	           |         Pin 3|
|CLK	            |        Pin 4|

---

# Practical 14 - Digital Temperature Display Using Arduino, Thermistor, and TM1637 4-Digit Display and Display using counter

- Code

```cpp
#include <TM1637Display.h>
#include <math.h>

// TM1637 Pins
#define CLK 4
#define DIO 3

TM1637Display display(CLK, DIO);

// Thermistor Pin
#define THERMISTOR_PIN A0

// Thermistor Constants
#define SERIES_RESISTOR 10000
#define NOMINAL_RESISTANCE 10000
#define NOMINAL_TEMPERATURE 25
#define B_COEFFICIENT 3950

// Counter Variables
int counter = 0;
bool thresholdCrossed = false;

void setup() {
 Serial.begin(9600);
 display.setBrightness(0x0f);  // Max brightness
 Serial.println("Temperature System Started");
}

void loop() {
 int adcValue = analogRead(THERMISTOR_PIN);

 // Avoid division by zero
 if (adcValue == 0) return;

 // Convert ADC value to resistance
 float resistance = SERIES_RESISTOR / (1023.0 / adcValue - 1);

 // Apply Steinhart-Hart Equation
 float steinhart;
 steinhart = resistance / NOMINAL_RESISTANCE;
 steinhart = log(steinhart);
 steinhart /= B_COEFFICIENT;
 steinhart += 1.0 / (NOMINAL_TEMPERATURE + 273.15);
 steinhart = 1.0 / steinhart;
 steinhart -= 273.15;

 float temperature = steinhart;

 // Print to Serial Monitor
 Serial.print("Temperature: ");
 Serial.print(temperature);
 Serial.println(" °C");

 // Display temperature (integer)
 display.showNumberDec((int)temperature);

 // Counter Logic (if temp > 30°C)
 if (temperature > 30 && !thresholdCrossed) {
   counter++;
   thresholdCrossed = true;

   Serial.print("Counter: ");
   Serial.println(counter);
 }

 if (temperature <= 30) {
   thresholdCrossed = false;
 }

 delay(1000);
}

```

- Connections 

Thermistor Connection (Voltage Divider)

|Component	         |           Connection |
|--------------------|----------------------|
|Thermistor one leg	 |           5V         |
|Thermistor other leg|	        A0 + one side of 10k resistor |
|10k resistor other side|	        GND         |

👉 A0 reads voltage to calculate temperature.

TM1637 Display Connection

|TM1637 Pin         |	    Arduino     |
|-------------------|-------------------|
|VCC                |       5V          |
|GND                |       GND         |
|DIO                |       Pin 3       |
|CLK	            |       Pin 4       |


Working Concept

●Thermistor senses temperature
●Arduino converts analog value → temperature (°C)
●TM1637 displays temperature
●Counter increments every time temperature crosses a threshold (e.g., 30°C) 

---