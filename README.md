# 🌊 AI 기반 선박 제어실 보조 On-Device 시스템
## 🏆 **인텔 엣지 AI 실무 프로젝트 경진대회 최우수상 수상**

<img src="docs/resources/video_gif/01_Dehazing.gif" alt="01_Dehazing.gif" width="800"/>
<img src="docs/resources/video_gif/03_AD_Dehazing.gif" alt="03_AD_Dehazing.gif" width="800"/>
<img src="docs/resources/video_gif/04_PE1.gif" alt="04_PE1.gif" width="800"/>
<img src="docs/resources/video_gif/06_Server_SystemLog.gif" alt="06_Server_SystemLog.gif" height="480"/>

## 💡 1. 프로젝트 개요

본 프로젝트는 **AI 기반 컴퓨터 비전 기술**을 활용하여 해상 환경에서 선박 항해 안전성과 효율 향상을 위한 **선박 제어실 보조 On-Device 시스템**입니다.
이 시스템은 **안개 속 객체를 실시간으로 탐지**하고, **이상을 감지**하며, **객체 추적, 자세 추정**기술로 선체 안전과 선원 보호를 도모합니다. 또한, **LLM**을 활용하여 **자동 항해 일지 작성과 브리핑**을 제공합니다.
시스템의 핵심 기능으로는 **안개 제거**, **이상 감지**, **낙상 감지**, **자동 항해 일지 작성** 등이 포합됩니다.

### 📍 전체 시스템 구상도
![Image](https://github.com/user-attachments/assets/aa3c5641-43c6-497d-b5ac-7b89fd1d8878)

---

## 🛠️ 2. 기술 스택

![RaspberryPi](https://img.shields.io/badge/Hardware-RaspberryPi5-A22846?style=for-the-badge&logo=raspberrypi&logoColor=white)
![JetsonNano](https://img.shields.io/badge/Hardware-Jetson%20Nano-76B041?style=for-the-badge&logo=nvidia&logoColor=white)
![IMU Sensor](https://img.shields.io/badge/Hardware-IMU%20Sensor-FF9900?style=for-the-badge&logo=generic&logoColor=white)

![Python](https://img.shields.io/badge/Language-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenVINO](https://img.shields.io/badge/Framework-OpenVINO-0078D4?style=for-the-badge&logo=intel&logoColor=white)
![PyTorch](https://img.shields.io/badge/Framework-PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![YOLO](https://img.shields.io/badge/Model-YOLOv8-FF2B2B?style=for-the-badge&logo=github&logoColor=white)
![EfficientNet](https://img.shields.io/badge/EfficientNet-B3-FF6F00?style=for-the-badge&logo=google&logoColor=white)
![MoveNet](https://img.shields.io/badge/Model-MoveNet-03A9F4?style=for-the-badge&logo=google&logoColor=white)
![MQTT](https://img.shields.io/badge/Protocol-MQTT-00B5A1?style=for-the-badge&logo=cloudsmith&logoColor=white)
![OpenCV](https://img.shields.io/badge/Library-OpenCV-5C3A00?style=for-the-badge&logo=opencv&logoColor=white)
![MariaDB](https://img.shields.io/badge/Database-MariaDB-003B57?style=for-the-badge&logo=mariadb&logoColor=white)
![PyQt6](https://img.shields.io/badge/Framework-PyQt6-41C1C1?style=for-the-badge&logo=python&logoColor=white)

---

## 🎯 3. 핵심 기능

- **안개 제거 및 이상 감지**: 
   - **CLAHE 기법**과 **DCP(Dark Channel Prior)** 기법을 사용하여 안개를 제거
   - **YOLOX-S**와 **EfficientNet-B3** 모델을 활용해 **이상 객체 탐지** 및 **라벨링** 실시간 수행

- **낙상 감지 및 경고 시스템**: 
   - **YOLOv8n** 모델로 **갑판 위 선원** 객체 탐지
   - **MoveNet Lightning** 모델로 **선원의 자세를 추정**, **fall down 상태**가 감지되면 서버로 **위험 알림** 전송
   - 특정 영역을 위험 구역으로 설정하고, 해당 구역에 사람이 있을 경우 **경고** 발송

- **자동 항해 일지 작성 및 브리핑**:
   - **LLM (대형 언어 모델)**을 활용
   - **STT (음성 인식)**와 **TTS (음성 합성)**로 선원 명령 처리와 **자동 항해 일지 작성** 및 **브리핑** 제공

- **실시간 데이터 처리**: 
   - 각 모듈에서 **MQTT**를 통해 데이터를 실시간으로 서버로 전송
   - **이상 감지**, **낙상 감지**, **위험 객체 감지** 등의 데이터 처리 및 저장
   - 실시간으로 처리된 데이터는 **모니터링 UI**를 통해 상황실에서 즉각 확인 가능 

---

## 📘 4. 기술 구현

### 1) **CLAHE (Contrast Limited Adaptive Histogram Equalization)** 기법
   - **목표**: 저조도 환경에서 이미지를 향상시켜 **선명한 시야** 확보
   - **기법**: 각 이미지 영역에 대해 **대비 향상**을 수행하여 안개 상태에서도 선명한 이미지 제공

### 2) **DCP (Dark Channel Prior)** 기법
   - **목표**: 이미지에서 **안개**를 제거하여 더욱 명확한 시야 제공
   - **기법**: 이미지의 **어두운 채널**을 사용해 안개 농도를 추정하고 이미지 복원

### 3) **YOLOX-S & EfficientNet-B3** 모델을 활용한 **이상 감지**
   - **목표**: 실시간으로 **이상 객체** 탐지
   - **기법**: **YOLOX-S** 모델을 사용하여 객체를 탐지하고, **EfficientNet-B3** 모델로 객체를 라벨링

### 4) **YOLOv8n 모델**을 활용한 **낙상 감지**
   - **목표**: 갑판 위 **선원 객체** 감지 및 **낙상** 상태 감지
   - **기법**: **YOLOv8n** 모델로 선원을 객체 탐지하고, **MoveNet Lightning**으로 자세를 추정하여 낙상 감지

### 5) **LLM**, **STT**, **TTS**를 활용한 **자동 항해 일지 작성 및 브리핑**
   - **목표**: **항해 일지** 자동 작성 및 **브리핑** 제공
   - **기법**: **STT**로 음성 인식, **TTS**로 실시간 브리핑 음성 제공, **LLM**으로 항해 일지 자동 작성

---

## 👨‍💻 5. 역할 및 기여

- **MQTT 기반 실시간 통신 구조 설계 및 구현**
  - 선박 단말(On-Device) ↔ 서버 간 이벤트/상태 데이터 송수신
  - 이상 감지, 낙상 감지, 위험 이벤트를 topic 기반으로 분리 설계

- **MariaDB 기반 서버 DB 설계 및 연동**
  - 이상 감지 로그, 낙상 이벤트, 시스템 상태 로그 저장
  - 자동 항해 일지 작성을 위한 데이터 구조 설계
  - 서버–DB 간 데이터 흐름 관리

- **PyQt6 기반 상황실(Control Room) GUI 구현**
  - 실시간 로그 모니터링 UI 구성
  - 이벤트 발생 시 시각적 알림 및 상태 표시
  - MQTT 수신 데이터 기반 UI 동적 갱신

- **LLM 기반 자동 항해 일지 작성 시스템 구현**
  - 수집된 이벤트/상태 로그를 구조화하여 LLM 입력 프롬프트 설계
  - 항해 중 발생한 주요 이벤트를 자연어 항해 일지로 자동 생성

- **STT / TTS 기반 음성 인터페이스 연동**
  - STT를 통한 음성 명령 처리 및 상황 보고 입력
  - TTS를 활용한 항해 요약 및 브리핑 음성 출력
  - GUI 및 서버 로직과 연동하여 실시간 음성 피드백 제공

---

## 🐞 6. 트러블슈팅

### 1) PyQt에서 실시간 이미지 표시 시 프레임 저하 문제
- 문제: 서버에서 수신한 이미지 프레임을 PyQt GUI에 실시간으로 출력할 때 FPS 저하 및 화면 끊김 발생
- 원인: 
  - MQTT는 경량 메시지 전송에 최적화된 프로토콜로, 영상/이미지 스트리밍에 구조적으로 부적합
  - 이미지 데이터를 Base64 문자열로 전송·복원하면서 CPU 부하 증가
  - PyQt GUI에서 다수의 이미지 디코딩·렌더링을 동시에 처리하며 렌더링 병목 발생
- 해결:
  - 이미지 해상도 축소 및 포맷 단순화로 전송 데이터 최소화
  - MQTT 환경에서 바이너리 스트리밍이 어려워 Base64 전송 방식을 유지하되,
    이미지 해상도 축소 및 UI 갱신 최소화로 병목을 완화
  - UI 갱신 주기 조절 및 불필요한 전체 화면 갱신 제거
- 결과: 실시간 영상 품질에는 한계가 있었으나, 상황 인지용 모니터링이 가능한 수준까지 성능 개선

### 2) PyQt GUI 실시간 업데이트 시 UI 멈춤 현상
- 문제: MQTT 메시지를 직접 UI 스레드에서 처리하며 화면이 일시적으로 멈춤
- 원인: 네트워크 I/O와 GUI 렌더링을 동일 스레드에서 처리
- 해결: MQTT 수신 로직을 별도 스레드로 분리하고 signal-slot 구조로 UI 갱신
- 결과: 실시간 로그 출력 시에도 GUI 응답성 유지

### 3) LLM 자동 항해 일지 생성 품질 불안정
- 문제: 동일한 이벤트 로그라도 항해 일지 문장이 일관되지 않거나 핵심 정보 누락
- 원인: 입력 로그 포맷이 비정형적이며 프롬프트 구조가 불명확
- 해결: 이벤트 로그를 시간순·유형별로 정리한 프롬프트 템플릿 설계
- 결과: 항해 일지의 일관성 및 가독성 향상

---

## 📚 7. 배운 점

- **OpenCV**, **PyTorch**, **YOLO** 등 AI 관련 다양한 지식 함양
- 다양한 **이미지 처리 기법**(CLAHE, DCP 등)을 활용하여 **저조도 및 안개 상태**에서도 시야를 확보 로직 구현 경험
- **MQTT**를 사용한 **실시간 데이터 전송** 및 **경고 발송 시스템** 구현을 보조하며 새로운 통신 방식에 대한 지식 습득
- **STT**와 **TTS**를 활용한 **자동 항해 일지 작성 및 브리핑** 시스템을 통해 **음성 인식 및 합성**의 유용성을 실감
- 충분한 성능 확보를 위한 **실시간 시스템 최적화**의 중요함 학습
---

### 🚢 Team

# CTRL SEA CTRL VISION

## 1. 프로젝트 소개
> ### **AI 기반 선박 제어실 보조 On-Device 시스템**

### 주요 기능
- **안개 너머 객체 탐지 및 이상 감지**  
- **선원 안전 확보**  
- **자동 항해 일지 작성 및 브리핑**

**개발 기간**: 2025.09.26 ~ 2025.10.22  
**개발 환경**: Jetson Nano / Raspberry Pi 5 / Python / MQTT  
**발표 자료 다운로드**: [[Ctrl + Click Here]](https://drive.google.com/drive/folders/1VzminDn5eenhiwE3JjTkIos7xjNJQT3j?usp=sharing)

## 2. 안개 제거 Dehazing

> ### **이미지 향상(Image Enhancement) 및 복원(Image Restoration)을 통한 시야 확보**

<img src="docs/resources/screenshot/01_Dehazing.png" alt="01_Dehazing.png" width="800"/>
<img src="docs/resources/screenshot/02_Dehazing.png" alt="02_Dehazing.png" width="800"/>
<img src="docs/resources/screenshot/03_Dehazing.png" alt="03_Dehazing.png" width="800"/>
<img src="docs/resources/video_gif/01_Dehazing.gif" alt="01_Dehazing.gif" width="800"/>

## 3. 이상 감지 Anomaly Detection

### 🛰 이상 감지 학습 방식
<img src="docs/resources/screenshot/04_AD.png" alt="04_AD.png" width="800"/>
<img src="docs/resources/screenshot/05_AD.png" alt="05_AD.png" width="800"/>
<img src="docs/resources/video_gif/02_AD_No_Dehazing.gif" alt="02_AD_No_Dehazing.gif" width="800"/>
<img src="docs/resources/video_gif/03_AD_Dehazing.gif" alt="03_AD_Dehazing.gif" width="800"/>

## 4. 낙상 감지 Fall Detection

<img src="docs/resources/screenshot/06_PE.png" alt="06_PE.png" width="800"/>
<img src="https://github.com/user-attachments/assets/ac1ceadf-53a7-4eb9-8f55-9cbd8d159dfe" width="800"/>  
<img src="https://github.com/user-attachments/assets/58486cd9-d9b5-46f6-bcaf-c36f92431969" width="800"/>  
<img src="docs/resources/video_gif/04_PE1.gif" alt="04_PE1.gif" width="800"/>
<img src="docs/resources/video_gif/05_PE2.gif" alt="05_PE2.gif" width="800"/>
 


## 5. 상황실 Ctrl Room

### 🛰 MQTT 통신 구조

<img src="docs/resources/screenshot/07_Server.png" alt="07_Server.png" width="800"/>
<img src="docs/resources/screenshot/08_Server.png" alt="08_Server.png" width="800"/>
<img src="docs/resources/video_gif/06_Server_SystemLog.gif" alt="06_Server_SystemLog.gif" height="480"/>
<img src="docs/resources/video_gif/07_Server_Logbook.gif" alt="07_Server_Logbook.gif" height="480"/>

## 6. 팀원 소개
| 이름 | 담당 |
|------|------|
| **문두르** | PM |
| **류균봉** | Image Enhancement / Dehazing |
| **나지훈** | Server / MQTT / GUI / LLM / STT / TTS |
| **김찬미** | Pose Estimation / Fall Detection |
| **이환중** | Object Detection / Anomaly Detection |
