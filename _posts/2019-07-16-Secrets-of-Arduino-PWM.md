---
layout: post
title: 아두이노 PWM의 비밀
---

# 아두이노 PWM의 비밀

[원본 문서](http://www.righto.com/2009/07/secrets-of-arduino-pwm.html) by Ken Shirrif 
[추가 편집](https://www.arduino.cc/en/Tutorial/SecretsOfArduinoPWM) by Paul Badger 

펄스 폭 변조(PWM, Pulus-width modulation)는 여러 가지 방법으로 아두이노에서 구현 될 수 있습니다. 이 글에서는 간단한 PWM 기술과 듀티 사이클 및 주파수를보다 효과적으로 제어하기 위해 PWM 레지스터를 직접 사용하는 방법에 대해 설명합니다. ATmega168, ATmega328을 사용하는 Arduino Diecimila, Duemilanove 모델에 대해 중점적으로 설명합니다.

PWM 신호는 주파수가 일정한 디지털 사각파이지만 신호가 켜져있는 시간(듀티 사이클, duty cycle)은 0 ~ 100%까지 다양합니다.
![pwm dutycycle](https://www.arduino.cc/en/uploads/Tutorial/pwm1.gif)

# PWM 예제

PWM에는 몇 가지 용도가 있습니다.

- 디밍 LED
- 아날로그 출력 제공(디지털 출력이 필터링 되면 0 ~ 100% 사이의 아날로그 전압을 제공합니다.)
- 오디오 신호 생성
- 모터의 가변 속도 제어 제공
- 변조 신호 생성(예: 원격 제어용 적외선 LED 작동)

__analogWrite로 간단한 펄스폭 변조__
Arduino의 프로그래밍 언어에서 간단히 PWM을 사용하기 위해서는 analogWrite(pin, dutyCycle)를 호출합니다. pin은 PWM 핀(3, 5, 6, 9, 10, 11) 중 하나이고 dutyCycle은 0~255 사이의 값입니다. analogWrite 함수는 하드웨어 PWM에 대한 간단한 인터페이스를 제공하지 주파수에 대한 어떠한 제어도 제공하지 않습니다.

아마 99%의 독자들은 여기서 멈추고 analgoWrite를 사용하면 되지만 더 많은 유연성을 제공하는 다른 옵션이 있습니다.

# 비트-뱅잉(Bit-banging) 펄스 폭 변조

원하는 시간 동안 핀을 반본적으로 켜고 끄는 방식으로 모든 핀에 PWM을 "수동으로" 구현할 수 있습니다. 예:
``` arduino
void setup(){
    pinMode(13, OUTPUT);
}

void loop()
{
  digitalWrite(13, HIGH);
  delayMicroseconds(100); // Approximately 10% duty cycle @ 1KHz
  digitalWrite(13, LOW);
  delayMicroseconds(1000 - 100);
}
```
이 기법은 모든 디지털 출력 핀을 사용할 수 있다는 장점이 있습니다. 주요한 단점들로 첫번째 단점은 모든 인터럽트가 타이밍에 영향을 미치므로 인터럽터를 비활성화하지 않으면 상당한 지터가 발생할 수 있다는 것입니다. 두 번째 단점은 프로세서가 다른 작업을 하는동안 출력을 실행 할 수 없다는 점입니다. 마지막으로 오실로스코프를 보면서 사이클을 신중하게 계산하거나 값을 조정하지 않으면 특정 듀티 사이클 및 주파수에 적합한 상수를 결정하기가 어렵습니다.

모든 핀을 수동으로 PWM하는 정교한 예제가[여기](http://www.arduino.cc/playground/Main/PWMallPins)에 있습니다.
