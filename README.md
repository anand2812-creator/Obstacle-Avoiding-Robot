#include <Servo.h>

// Pin Definitions
#define trigPin 9
#define echoPin 10

#define IN1 5
#define IN2 6
#define IN3 7
#define IN4 8

#define enA 3
#define enB 11

Servo myServo;

long duration;
int distance;
int leftDistance;
int rightDistance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  pinMode(enA, OUTPUT);
  pinMode(enB, OUTPUT);

  myServo.attach(4);
  myServo.write(90);

  analogWrite(enA, 150);  // Motor speed
  analogWrite(enB, 150);
}

void loop() {

  distance = getDistance();

  if (distance > 15) {
    moveForward();
  } else {
    stopMotors();
    delay(300);

    myServo.write(0);      // Check left
    delay(500);
    leftDistance = getDistance();

    myServo.write(180);    // Check right
    delay(500);
    rightDistance = getDistance();

    myServo.write(90);     // Center
    delay(300);

    if (leftDistance > rightDistance) {
      turnLeft();
      delay(500);
    } else {
      turnRight();
      delay(500);
    }
  }
}

// Function to measure distance
int getDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  return distance;
}

// Movement Functions
void moveForward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void turnLeft() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void turnRight() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void stopMotors() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}


