# Practical 5 - Arduino Loops

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