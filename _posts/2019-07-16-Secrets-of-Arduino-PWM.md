---
layout: post
title: 아두이노 PWM의 비밀
---

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

원하는 시간 동안 핀을 반본적으로 켜고 끄는 방식으로 모든 핀에 PWM을 "수동으로" 구현할 수 있습니다.
``` cpp
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

모든 핀을 수동으로 PWM하는 정교한 예제가 [여기](http://www.arduino.cc/playground/Main/PWMallPins)에 있습니다.

# ATmega PWM 레지스터 직접 사용

ATmega168P/328P 칩에는 6개의 PWM 출력을 제어하는 3개의 PWM 타이머가 있습니다. 타이머 레지스터를 직접 조작하여 analogWrite를 사용하는 것보다 더 많은 제어를 얻을 수 있습니다.

AVR ATmega328P 데이터 시트는 PVWM 타이머에 대한 자세한 설명을 제공하만 타이머의 다양한 제어 및 출력 모드로 인해 이해하기 어려울 수 있습니다.

Arduino 언어와 데이터 시트 간의 관계에 대한 적합한 정보가 여기에 있을 수 있습니다

__Atmega 168/328 타이머들__

ATmega328은 Timer 0, Timer 1, Tumer 2로 알려진 3개의 타이머가 있습니다. 각 타이머에는 타이머의 두 출력에 대한 PWM 폭을 제어하는 두 개의 출력 비교 레지스터(OCR, Output Compare Register)가 있습니다. 타이머가 비교 레지스터 값에 도달하면 해당 출력이 토글됩니다. 각 타이머의 두 개의 출력 레지스터는 일반적으로 동일한 주파수를 갖지만 출력 비교 레지스터에 따라 다른 듀티 사이클을 가질수 있습니다.

각 타이머에는 시스템 클럭을 1, 8, 64, 256, 1024와 같은 프리스케일 계수로 타이머 클럭을 생성하는 프리스케일러가 있습니다. Arduino는 16MHz의 시스템 클럭을 가지고 있으며 타이머 클럭 주파수는 시스템 클럭 주파수를 프리스케일 계수로 나눈 값이 됩니다.
> 타이머 2에는 다른 타이머와 같지 않은 사전 설정 값이 있습니다.

타이머는 여러가지 다른 모드로 복잡하게 되어있습니다. 주요 PWM 모드는 "Fast PWM", "Phase-correct PWM"이며 아래에서 설명합니다. 타이머는 0부터 255이나 0부터 고정된 값까지 실행할 수 있습니다. (16비트 타이머 1은 16 비트까지 타이머 값을 지원하는 추가 모드가 있습니다.) 각 출력은 반전시킬수도 있습니다.

또한 오버플로에서 인터럽트를 생성하거나 두 출력 비교 레지스터에 대해 일치시킬 수 있지만이 기사의 범위를 벗어납니다. Timer Register Serveral 레지스터는 각 타이머를 제어하는 데 사용됩니다. Timer/Counter Control Registers TCCRnA, TCCRnB는 타이머의 주요 제어 비트를 유지합니다. (TCCRnA와 TCCRnB는 출력 A와 B에 해당하지 않습니다.) 이 레지스터는 여러 비트 그룹을 유지합니다.

- 파형 셍성 모드(Waveorm Generation Mode, WGM) 비트: 타이머의 전체 모드를 제어합니다.
  (이 비트들은 TCCRnA와 TCCRnB로 나뉘어져 있습니다.)
- 클럭 선택 (Clcok Select, CS) 비트: 클럭의 프리스케일러를 제어합니다.
- 출력 일치 비교 A 모드(Compare Mathc Ouput A Mode, COMnA) 비트: 출력 A를 활성화/비활성화/반전 합니다.
- 출력 일치 비교 B 모드(Compare Mathc Ouput B Mode, COMnB) 비트: 출력 B를 활성화/비활성화/반전 합니다.

출력 비교 레지스터 OCRnA, OCRnB는 출력 A, B가 영향 받는 레벨을 설정합니다. 타이머 값이 레지스터 값과 일치하면 해당 출력이 모드에서 지정한대로 수정됩니다.
