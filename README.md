const int leftCoilPin = A5;
const int rightCoilPin = A2;

const int leftMotorPWMPin = 5;
const int rightMotorPWMPin = 3;

int baseSpeed = 80;
float Kp = 0.2;
float Kd = 0.05;

int sensorOffset = 0;
int previousError = 0;


int deadband = 0; 

void setup() {
  Serial.begin(9600);
  
  pinMode(leftMotorPWMPin, OUTPUT);
  pinMode(rightMotorPWMPin, OUTPUT);
  
  delay(3000);
}

int calculateError() {
  int leftSignal = analogRead(leftCoilPin);
  int rightSignal = analogRead(rightCoilPin);
  
  int currentError = (leftSignal - rightSignal) + sensorOffset;
  
  Serial.print("Left: "); Serial.print(leftSignal); Serial.print(" | Right: "); Serial.print(rightSignal); Serial.print(" | Error: "); Serial.println(currentError);
  
  return currentError;
}

void loop() {
  int error = calculateError();
  

  if (error > deadband) {
    error = error - deadband;       
  } else if (error < -deadband) {
    error = error + deadband;       
  } else {
    error = 0;                     
  }
  
  int errorDerivative = error + previousError;
  int steeringAdjustment = (Kp * error) - (Kd * errorDerivative);
  
  previousError = error;
  
  int leftMotorSpeed = baseSpeed + steeringAdjustment;
  int rightMotorSpeed = baseSpeed - steeringAdjustment;
  
  leftMotorSpeed = constrain(leftMotorSpeed, 0, 255);
  rightMotorSpeed = constrain(rightMotorSpeed, 0, 255);
  
  analogWrite(leftMotorPWMPin, leftMotorSpeed);
  analogWrite(rightMotorPWMPin, rightMotorSpeed);
  
  delay(10);
}
