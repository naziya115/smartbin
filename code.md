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
### Ultra module with servo
```
//define Pins
#include <Servo.h>

Servo servo;

int trigPin = 9;
int echoPin = 8;

// defines variables
long duration;
int distance;

void setup() 
{
  servo.attach(7);
  servo.write(0);
 delay(1000);
  
// Sets the trigPin as an Output
pinMode(trigPin, OUTPUT);
// Sets the echoPin as an Input 
pinMode(echoPin, INPUT);
// Starts the serial communication 
Serial.begin(9600); 
}
void loop() 
{
// Clears the trigPin
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
// Sets the trigPin on HIGH state for 10 micro seconds
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);
// Reads the echoPin, returns the sound wave travel time in microseconds
duration = pulseIn(echoPin, HIGH);
// Calculating the distance
distance= duration*0.034/2;
// Prints the distance on the Serial Monitor
Serial.print("Distance: ");
Serial.println(distance);
if ( distance <= 25   ) // Change Distance according to Ultrasonic Sensor Placement
 {

servo.write(0);
delay(1000);
 } 
else 
{
servo.write(180);

 }

}
```
