---
layout: post
title: 아두이노 PWM의 비밀
---

# 아두이노 PWM의 비밀

작성자: Ken Shirrif
추가 편집: Paul Badger
(원본 문서)(http://www.righto.com/2009/07/secrets-of-arduino-pwm.html)
(추가 편집 문서)(https://www.arduino.cc/en/Tutorial/SecretsOfArduinoPWM)

펄스폭변조(PWM, Pulus-width modulation)는 여러 가지 방법으로 아두이노에서 구현 될 수 있습니다. 이 글에서는 간단한 PWM 기술과 듀티 사이클 및 주파수를보다 효과적으로 제어하기 위해 PWM 레지스터를 직접 사용하는 방법에 대해 설명합니다. 이 글에서는 ATmega168 또는 ATmega328을 사용하는 Arduino Diecimila 및 Duemilanove 모델에 대해 중점적으로 설명합니다.

간단히 말해, PWM 신호는 주파수가 일정한 디지털 사각파이지만 신호가 켜져있는 시간(듀티 사이클, duty cycle)은 0 ~ 100%까지 다양합니다.
![pwm dutycycle](https://www.arduino.cc/en/uploads/Tutorial/pwm1.gif)