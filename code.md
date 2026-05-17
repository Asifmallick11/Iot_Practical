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

D2	LED 1 Anode (long leg)
D3	LED 2 Anode (long leg)
GND Resistor → LED 1 Cathode (short leg) 
GND Resistor → LED 2 Cathode (short leg)

---