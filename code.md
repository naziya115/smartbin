#Done code
```
#include <Servo.h>

// notes for buzzer
#define NOTE_A3  220
#define NOTE_C4  262
#define NOTE_E4  330
#define NOTE_G4  392

// variables for joystick
#define GROUND_JOY_PIN A3           
#define VOUT_JOY_PIN A2              
#define XJOY_PIN A1 

Servo servo;

// servo pins
int trigPin = 9;
int echoPin = 8;
// buzzer pin
int piezoPin =5; 

long duration;
int distance;

// change this to make the song slower or faster
int tempo = 160;

int melody[] = {
    NOTE_C4,4, NOTE_E4,4, NOTE_G4,4, NOTE_E4,4, 
    NOTE_C4,4, NOTE_E4,8, NOTE_G4,-4, NOTE_E4,4,
    NOTE_A3,4, NOTE_C4,4, NOTE_E4,4, NOTE_C4,4,
  
};
int notes = sizeof(melody) / sizeof(melody[0]) / 2;
int wholenote = (60000 * 4) / tempo;
int divider = 0, noteDuration = 0;

void setup(){
  servo.attach(7);
  servo.write(0);
  delay(1000);

  // ultra module
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  //buzzer
  pinMode(piezoPin, OUTPUT);

  // joystick
  pinMode(VOUT_JOY_PIN, OUTPUT) ;  
  pinMode(GROUND_JOY_PIN, OUTPUT) ; 
  digitalWrite(VOUT_JOY_PIN, HIGH) ;
  digitalWrite(GROUND_JOY_PIN,LOW) ; 

  Serial.begin(9600); 
}
void loop(){
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH);
  distance= duration*0.034/2;
  Serial.print("Distance: ");
  Serial.println(distance);

  int joystickXVal = analogRead(XJOY_PIN) ; 
  Serial.print("joystick — ");  
  Serial.println((joystickXVal+520)/10); //print a from A1 calculated, scaled value

  
  
    if ( distance <= 25){
      servo.write(0);
      
      for (int thisNote = 0; thisNote < notes * 2; thisNote = thisNote + 2) {
        divider = melody[thisNote + 1];
        if (divider > 0) {
          noteDuration = (wholenote) / divider;
        }
        else if (divider < 0) {
          noteDuration = (wholenote) / abs(divider);
          noteDuration *= 1.5; 
        }
      tone(piezoPin, melody[thisNote], noteDuration * 0.9);
      delay(noteDuration);
      noTone(piezoPin); 
     }
    }else if(((joystickXVal+520)/10)<100){
     servo.write(0); 
    // tone(piezoPin, 1000); 
    // noTone(piezoPin); 
  }else if(((joystickXVal+520)/10)>100){
     servo.write(180);
     delay(500);
    // noTone(piezoPin);     
  //   delay(1000);        
  }0 else{
    noTone(piezoPin);
    servo.write(180);
    delay(500);
  }  
}
```


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
### Ultra module with servo and buzzer
```
#include <Servo.h>

// notes for buzzer
#define NOTE_A3  220
#define NOTE_C4  262
#define NOTE_E4  330
#define NOTE_G4  392

Servo servo;

// servo pins
int trigPin = 9;
int echoPin = 8;
// buzzer pin
int piezoPin = 5; 

long duration;
int distance;

// change this to make the song slower or faster
int tempo = 160;

int melody[] = {
    NOTE_C4,4, NOTE_E4,4, NOTE_G4,4, NOTE_E4,4, 
    NOTE_C4,4, NOTE_E4,8, NOTE_G4,-4, NOTE_E4,4,
    NOTE_A3,4, NOTE_C4,4, NOTE_E4,4, NOTE_C4,4,
  
};
int notes = sizeof(melody) / sizeof(melody[0]) / 2;
int wholenote = (60000 * 4) / tempo;
int divider = 0, noteDuration = 0;

void setup(){
  servo.attach(7);
  servo.write(0);
  delay(1000);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(piezoPin, OUTPUT);
  Serial.begin(9600); 
}
void loop(){
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH);
  distance= duration*0.034/2;
  Serial.print("Distance: ");
  Serial.println(distance);
    if ( distance <= 25){
      servo.write(0);
      
      for (int thisNote = 0; thisNote < notes * 2; thisNote = thisNote + 2) {
        divider = melody[thisNote + 1];
        if (divider > 0) {
          noteDuration = (wholenote) / divider;
        }
        else if (divider < 0) {
          noteDuration = (wholenote) / abs(divider);
          noteDuration *= 1.5; 
        }
      tone(piezoPin, melody[thisNote], noteDuration * 0.9);
      delay(noteDuration);
      noTone(piezoPin); 
     }
    }
   else{
    noTone(piezoPin);
    servo.write(180);
    delay(500);
  }
}
```

### Same code but just in case...
```
#include <Servo.h>

// notes for buzzer
#define NOTE_A3  220
#define NOTE_C4  262
#define NOTE_E4  330
#define NOTE_G4  392

Servo servo;

// servo pins
int trigPin = 9;
int echoPin = 8;
// buzzer pin
int piezoPin =5; 

long duration;
int distance;

// change this to make the song slower or faster
int tempo = 160;

int melody[] = {
    NOTE_C4,4, NOTE_E4,4, NOTE_G4,4, NOTE_E4,4, 
    NOTE_C4,4, NOTE_E4,8, NOTE_G4,-4, NOTE_E4,4,
    NOTE_A3,4, NOTE_C4,4, NOTE_E4,4, NOTE_C4,4,
  
};
int notes = sizeof(melody) / sizeof(melody[0]) / 2;
int wholenote = (60000 * 4) / tempo;
int divider = 0, noteDuration = 0;

void setup(){
  servo.attach(7);
  servo.write(0);
  delay(1000);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(piezoPin, OUTPUT);
  Serial.begin(9600); 
}
void loop(){
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH);
  distance= duration*0.034/2;
  Serial.print("Distance: ");
  Serial.println(distance);
    if ( distance <= 25){
      servo.write(0);
      
      for (int thisNote = 0; thisNote < notes * 2; thisNote = thisNote + 2) {
        divider = melody[thisNote + 1];
        if (divider > 0) {
          noteDuration = (wholenote) / divider;
        }
        else if (divider < 0) {
          noteDuration = (wholenote) / abs(divider);
          noteDuration *= 1.5; 
        }
      tone(piezoPin, melody[thisNote], noteDuration * 0.9);
      delay(noteDuration);
      noTone(piezoPin); 
     }
    }
   else{
    noTone(piezoPin);
    servo.write(180);
    delay(500);
  }
}
```


### ALmost implemented joystick to servo

```
#include <Servo.h>

// notes for buzzer
#define NOTE_A3  220
#define NOTE_C4  262
#define NOTE_E4  330
#define NOTE_G4  392
#define SERVO_PIN 9
#define GROUND_JOY_PIN A3           
#define VOUT_JOY_PIN A2              
#define XJOY_PIN A1 

Servo servo;

// servo pins
int trigPin = 9;
int echoPin = 8;
// buzzer pin
int piezoPin =5; 

long duration;
int distance;

// change this to make the song slower or faster
int tempo = 160;

int melody[] = {
    NOTE_C4,4, NOTE_E4,4, NOTE_G4,4, NOTE_E4,4, 
    NOTE_C4,4, NOTE_E4,8, NOTE_G4,-4, NOTE_E4,4,
    NOTE_A3,4, NOTE_C4,4, NOTE_E4,4, NOTE_C4,4,
  
};
int notes = sizeof(melody) / sizeof(melody[0]) / 2;
int wholenote = (60000 * 4) / tempo;
int divider = 0, noteDuration = 0;

void setup(){
  servo.attach(9);
  servo.write(0);
  delay(1000);

  // ultra module
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(piezoPin, OUTPUT);

  // joystick
  pinMode(VOUT_JOY_PIN, OUTPUT) ;  
  pinMode(GROUND_JOY_PIN, OUTPUT) ; 
  digitalWrite(VOUT_JOY_PIN, HIGH) ;
  digitalWrite(GROUND_JOY_PIN,LOW) ; 

  Serial.begin(9600); 
}
void loop(){
  int joystickXVal = analogRead(XJOY_PIN) ; 
  Serial.print("joystick — ");  
  delay(500);
  Serial.println((joystickXVal+520)/10); //print a from A1 calculated, scaled value

 /* digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);*/
  
  /*duration = pulseIn(echoPin, HIGH);
  distance= duration*0.034/2;
  Serial.print("Distance: ");
  Serial.println(distance);*/

  if(((joystickXVal+520)/10)<100){
     servo.write(0); 
  }else {
     servo.write(180); 
  }
  Serial.print("servo — ");      
  Serial.println(servo.read()); 
  delay(500);

 /* if ( distance <= 25){
     servo.write(0);
      
  for (int thisNote = 0; thisNote < notes * 2; thisNote = thisNote + 2) {
    divider = melody[thisNote + 1];
    if (divider > 0) {
      noteDuration = (wholenote) / divider;
    }
    else if (divider < 0) {
      noteDuration = (wholenote) / abs(divider);
      noteDuration *= 1.5; 
    }
      tone(piezoPin, melody[thisNote], noteDuration * 0.9);
      delay(noteDuration);
      noTone(piezoPin); 
     }
    }
   else{
    noTone(piezoPin);
    servo.write(180);
    delay(500);
  }*/
}
```
### Control servo and a buzzer with a joystick
```
#include <Servo.h>

// notes for buzzer
#define NOTE_A3  220
#define NOTE_C4  262
#define NOTE_E4  330
#define NOTE_G4  392
#define SERVO_PIN 9
#define GROUND_JOY_PIN A3           
#define VOUT_JOY_PIN A2              
#define XJOY_PIN A1 

Servo servo;

// servo pins
int trigPin = 9;
int echoPin = 8;
// buzzer pin
int piezoPin =5; 

long duration;
int distance;

// change this to make the song slower or faster
int tempo = 160;

int melody[] = {
    NOTE_C4,4, NOTE_E4,4, NOTE_G4,4, NOTE_E4,4, 
    NOTE_C4,4, NOTE_E4,8, NOTE_G4,-4, NOTE_E4,4,
    NOTE_A3,4, NOTE_C4,4, NOTE_E4,4, NOTE_C4,4,
  
};
int notes = sizeof(melody) / sizeof(melody[0]) / 2;
int wholenote = (60000 * 4) / tempo;
int divider = 0, noteDuration = 0;

void setup(){
  servo.attach(9);
  servo.write(0);
  delay(1000);

  // ultra module
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(piezoPin, OUTPUT);

  // joystick
  pinMode(VOUT_JOY_PIN, OUTPUT) ;  
  pinMode(GROUND_JOY_PIN, OUTPUT) ; 
  digitalWrite(VOUT_JOY_PIN, HIGH) ;
  digitalWrite(GROUND_JOY_PIN,LOW) ; 

  Serial.begin(9600); 
}
void loop(){
  int joystickXVal = analogRead(XJOY_PIN) ; 
  Serial.print("joystick — ");  
  Serial.println((joystickXVal+520)/10); //print a from A1 calculated, scaled value

  if(((joystickXVal+520)/10)<100){
     servo.write(0); 
     tone(piezoPin, 1000); 
    // noTone(piezoPin); 
  }else {
     servo.write(180); 
     noTone(piezoPin);     
  //   delay(1000);        
  }
  Serial.print("servo — ");      
  Serial.println(servo.read()); 
 // delay(500);
}
```
