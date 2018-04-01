---
layout: post
title: Sandisk Secure Digital Card
---



## 목차
<ul>
 <li>1. SD카드 소개</li>
 <li>2. 제픔 사양</li>
 <li>3. SD 카드 인터페이스 설명</li>
 <li>4. 보안 디지털(SD) 카드 프로토콜 설명</li>
 <li>5. SPI 프로토콜 정의
  <ul>
    <li>5.1. SPI 버스 프로토콜
     <ul>
      <li>5.1.1. 모드 선택</li>
      <li>5.1.2. 버스 전송 보호</li>
      <li>5.1.3. 데이터 읽기</li>
      <li>5.1.4. 데이터 쓰기</li>
      <li>5.1.5. 지우기 및 쓰기 보호 관리</li>
      <li>5.1.6. CID/CSD 레지스터 읽기</li>
      <li>5.1.7. 시퀀스 리셋</li>
      <li>5.1.8. 클록 제어</li>
      <li>5.1.9. 에러 검출
        <ul>
         <li>5.1.9.1. CRC 와 잘못된 명령(Illegal Commands)</li>    
         <li>5.1.9.2. 읽기, 쓰기 및 지우기 시간 초과 조건(Time-out Conditions)</li>    
        </ul>
       </li>
      <li>5.1.10. 메모리 배열 분활</li>
      <li>5.1.11. 카드 잠금/해제</li>
      <li>5.1.12. 응용 프로그램별 명령</li>
      <li>5.1.13. 저작권 보호 명령</li>
     </ul>
    </li>
    <ul>
     <li>5.2. SPI 명령어 집합
      <ul>
       <li>5.2.1. 명령어 형식</li>
       <li>5.2.2. 명령어 클래스</li>
       <li>5.2.3. 응답</li>
       <li>5.2.4. 데이터 토큰</li>
       <li>5.2.5. 데이터 오류 토큰</li>
       <li>5.2.6. Clearing Status Bits</li>
      </ul>
     </li>
     <li>5.3. 카드 레지스터</li>
     <li>5.4. SPI 버스 타이밍 다이아그램
      <ul>
       <li>5.4.1. 명령/응답</li>
       <li>5.4.2. 데이터 읽기</li>
       <li>5.4.3. 데이터 쓰기</li>
       <li>5.4.4. 타이밍 값</li>
      </ul>
     </li>
     <li>5.5. SPI 전기 인터페이(Electrical Interface)</li>
     <li>5.6. SPI 버스 작동 조건</li>
     <li>5.7. 버스 타이밍</li>
    </ul>
  </ul>
 </li>
 <li>부록 A. 애플리케이션 메모</li>
 <li>부록 B. 주문 정보</li>
 <li>부록 C. SnaDisk 전세계 영업소</li>
 <li>부록 D. 제한 보증</li>
 <li>부록 E. 책임 면책</li>
</ul>


## 4. 보안 디지털(SD)프토토콜 설명