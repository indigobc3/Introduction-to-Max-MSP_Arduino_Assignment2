(ReadMe Code)

Arduino Code below connected to Arduino Uno Microcontroller, and Max Msp software. Code controls interactive system between Max and Arduino. Information is sent from Arduino to Max, and Max to Arduino. 



# Introduction-to-Max-MSP_Arduino_Assignment2
Assignment2_Max MSP_Arduino_

//Piezo de Resistance


// define variables

const int pot1DelayChord = A0;     

const int pot2DrumLPF = A1; 

const int pot3HR = A2;    

const int sensorFSR = A3;

const int pot4Metro = A4;

const int photoTransistorCrush = A5;

const int drumButton = 9;

const int chordButton = 8;

const int melodyButton = 7;

const int piezoButton = 6;

const int trigPin = 3;

const int echoPin = 4;

long duration;

int distance;

int pitch;

int piezoPlay;



void setup() {

  Serial.begin(9600); // opens Serial Communication
  
  pinMode(5, OUTPUT); // sets piezo as Output
  
  pinMode(trigPin, OUTPUT); // sets the trigPin as Output
  
  pinMode(echoPin, INPUT); // sets the echoPin as Input
  
}



void loop() {
  // ARDUINO TO MAX
  
  Serial.print(analogRead(pot1DelayChord)); // sends pot1 information to Serial

  Serial.print(","); // inlcudes space between data

  Serial.print(analogRead(pot2DrumLPF)); // sends pot2 information to Serial

  Serial.print(",");

  Serial.print(analogRead(pot3HR)); // sends pot3 information to Serial

  Serial.print(",");
  
  Serial.print(analogRead(sensorFSR)); //Sends FSR information to Serial

  Serial.print(",");

  Serial.print(analogRead(pot4Metro)); //sends pot4 information to Serial

  Serial.print(",");
   
  digitalWrite(trigPin, LOW);   // clears the trigPin for Ultrasonic Sensor
  
  delayMicroseconds(2);   // sets the trigPin on HIGH state for 10 micro seconds

  digitalWrite(trigPin, HIGH);
  
  delayMicroseconds(10);
  
  digitalWrite(trigPin, LOW);   // reads the echoPin, returns the sound wave travel time in microseconds

  duration = pulseIn(echoPin, HIGH);
  
  distance = duration * 0.034 / 2; // calculating the distance

  Serial.print(distance); // prints the distance on the Serial Monitor

  Serial.print(",");

  Serial.print(digitalRead(drumButton));

  Serial.print(",");

  Serial.print(digitalRead(chordButton));

  Serial.print(",");

  Serial.print(digitalRead(melodyButton));

  Serial.print(",");

  Serial.print(digitalRead(piezoButton));

  Serial.print(",");

  Serial.println(analogRead(photoTransistorCrush));
  

 // MAX TO ARDUINO
  
  piezoPlay = Serial.read(); // reads information sent from Max to Serial
  
   
  pitch = map(piezoPlay,0,127, 50, 6000); // maps midi information to frequency
  
  
  tone(5,pitch,40); // piezo plays pitch frequency for 40 ms 

  
  delay(100); // delay
  
}
  

 
