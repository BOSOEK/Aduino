# 001. 2021 처음 하는 아두이노 프로젝트 : 보트

### 회로
![boat_Circuit](https://user-images.githubusercontent.com/68007145/109810384-d06c4200-7c6c-11eb-9c59-fd60fc7a085a.PNG)

### 코드

  #include <Servo.h>
  Servo servo;
  #include <IRremote.h>

  IRrecv irrecv(8);
  decode_results results;

  void setup()
  {
    servo.attach(9);

    irrecv.enableIRIn();
    pinMode(12, OUTPUT);
    pinMode(8, INPUT);

    Serial.begin(9600);
  }

  int motor = 0;

  void loop()
  {
    if(irrecv.decode(&results)) {
      Serial.println(results.value, HEX);

      if(results.value == 0xFD30CF && motor == 0) {
        digitalWrite(12, HIGH);
          motor = 1;
      }
      else if(results.value == 0xFD30CF && motor == 1) {
        digitalWrite(12, LOW);
          motor = 0;
      }

      if(results.value == 0xFD08F7) {
        servo.write(130);
          delay(50);
      }

      if(results.value == 0xFD8877) {
        servo.write(90);
          delay(50);
      }

      if(results.value == 0xFD48B7) {
        servo.write(50);
        delay(50);
      }


      delay(30);
      irrecv.resume();
    }
  }

