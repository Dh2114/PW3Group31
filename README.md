
const int leftMotorPWMPin = 4;  
const int rightMotorPWMPin = 15; 


int baseSpeed = 300; 
float Kp = 0.8;      
float Kd = 0.5;      


int previousError = 0; 

void setup() {
  Serial.begin(9600);
  
  
  pinMode(leftMotorPWMPin, OUTPUT);
  pinMode(rightMotorPWMPin, OUTPUT);
}


int calculateError() {
  int currentError = 0;.
  return currentError;
}

void loop() {
  int error = calculateError();
  
  int errorDerivative = error - previousError;
  
  int steeringAdjustment = (Kp * error) + (Kd * errorDerivative);
  
  previousError = error;
  
  int leftMotorSpeed = baseSpeed + steeringAdjustment;
  int rightMotorSpeed = baseSpeed - steeringAdjustment;
  
  leftMotorSpeed = constrain(leftMotorSpeed, 0, 255);
  rightMotorSpeed = constrain(rightMotorSpeed, 0, 255);
  
  analogWrite(leftMotorPWMPin, leftMotorSpeed);
  analogWrite(rightMotorPWMPin, rightMotorSpeed);
  

  delay(10); 
