### Ultra module code
```
const int pingPin = 9; // Trig
const int echoPin = 8; // Echo 

void setup() {
   Serial.begin(9600); 
}

void loop() {
   long duration, cm;
   pinMode(pingPin, OUTPUT);
   digitalWrite(pingPin, LOW);
   delayMicroseconds(2);
   digitalWrite(pingPin, HIGH);
   delayMicroseconds(10);
   digitalWrite(pingPin, LOW);
   pinMode(echoPin, INPUT);
   duration = pulseIn(echoPin, HIGH);
   cm = microsecondsToCentimeters(duration);
   Serial.print(cm);
   Serial.print("cm");
   Serial.println();
   delay(1000);
}

long microsecondsToCentimeters(long microseconds) {
   return microseconds / 29 / 2;
}
```
