# AI_IoT_pet_project
좋습니다!
위에서 제안드린 구조를 토대로 **스마트 펫 피더 개발 계획서 초안**을 작성해 드릴게요.
(워드 문서에 그대로 넣을 수 있도록 깔끔하게 작성하겠습니다.)

---

# 📑 스마트 펫 피더 개발 계획서 (초안)

---

## 1. 프로젝트 개요

### 프로젝트명

> **스마트 펫 피더**

### 개발 목적

* 반려동물의 자동 사료 급여 및 건강 상태(백내장 등 안과 질환) 모니터링을 통해
  주인의 부재 시에도 건강한 생활을 유지할 수 있도록 지원

### 기대 효과

* 주인이 집에 없어도 자동으로 사료를 급여
* 반려동물 눈 질환(백내장 등) 조기 발견
* 모바일 앱에서 실시간 상태 및 진단 결과 확인

---

## 2. 개발 범위 및 주요 기능

### 📱 Flutter 앱

* **사료 주기 버튼**: 누르면 Firebase를 통해 사료 배급 명령 전달
* **사진 찍고 진단 버튼**: IoT 장치에 카메라 촬영 및 AI 진단 요청
* **장치 상태 표시**: 온라인/오프라인, 현재 동작 상태(급여 중 등)
* **최근 진단 결과 및 촬영 이미지 표시**

### 🔥 Firebase (Realtime Database)

* Flutter ↔ IoT 간 데이터 허브
* 실시간 데이터 동기화 및 이벤트 처리

### 🤖 IoT (Raspberry Pi)

* Firebase 명령을 수신(`commands/feed`, `commands/diagnose`)
* 사료 모터 구동 및 카메라 촬영
* 촬영 이미지 → TFLite 모델로 백내장 진단 → 결과 DB 기록

---

## 3. 시스템 구성도

```
Flutter 앱 <------> Firebase Realtime Database <------> 라즈베리파이 (IoT)
  ↑                      ↑                           ↑
  |                      |                           |
사료/진단 명령 set    상태 및 진단결과 onValue     명령 수신 & 처리 후 DB 업데이트
```

* **데이터 흐름**

  1. Flutter에서 `commands/feed` or `commands/diagnose` 명령 기록
  2. IoT 장치가 Firebase에서 변화를 감지 후 사료 투입 / 카메라 촬영
  3. IoT가 `device_status`, `diagnosis_log`를 업데이트
  4. Flutter 앱이 이를 실시간으로 구독하여 UI 표시

---

## 4. 사용 기술 스택

| 파트     | 주요 기술                                              |
| ------ | -------------------------------------------------- |
| 모바일    | Flutter (Dart), firebase\_core, firebase\_database |
| 서버(DB) | Firebase Realtime Database                         |
| IoT    | Raspberry Pi 4, Python 3, TFLite, OpenCV, GPIO     |
| AI 모델  | MobileNetV2 기반 안과질환 분류 모델 (tflite)                 |
| 통신     | Firebase SDK (python + flutterfire)                |

---

## 5. 데이터베이스 구조

* **/commands/**

  * `feed`: 사료 주기 명령 (timestamp)
  * `diagnose`: 카메라 촬영 및 AI 진단 명령 (timestamp)

* **/device\_status/**

  * `is_online`: 장치 온라인 여부
  * `last_seen`: 마지막 상태 보고 시각
  * `current_state`: feeding, diagnosing, idle 등

* **/diagnosis\_log/last\_result/**

  * `result`: 진단 결과 텍스트
  * `image_url`: 촬영 이미지 URL

---

## 6. 개발 일정 (예상)

| 주차  | 주요 작업 내용                     |
| --- | ---------------------------- |
| 1주차 | 요구사항 정의, 전체 구조 설계, DB 모델 설계  |
| 2주차 | Flutter 앱 UI 개발, Firebase 연동 |
| 3주차 | 라즈베리파이 GPIO + 카메라 테스트        |
| 4주차 | AI 진단(TFLite) 모델 적용 및 검증     |
| 5주차 | 시스템 통합 테스트, UI 디버깅           |
| 6주차 | 문서화 및 발표 자료 작성               |

---

## 7. 테스트 및 검증 계획

* **Flutter ↔ Firebase 연결 확인**

  * 버튼 클릭 시 Firebase `commands` 갱신
* **Firebase ↔ IoT 연동 검증**

  * IoT가 명령 수신 후 모터 동작 및 카메라 촬영 수행
* **AI 모델 검증**

  * 정상 이미지, 백내장 의심 이미지에 대한 TFLite 추론 결과 비교
* **종합 시나리오 테스트**

  * 앱 → Firebase → IoT → Firebase → 앱 의 전체 플로우 정상 동작

---

## 8. 추후 개선 및 확장 계획

* 진단 질환 추가 (피부병, 구강 상태 등)
* 사료 무게센서로 급여량 자동 계산
* Firebase Messaging을 통한 푸시 알림
* 데이터 장기 저장 (Firestore / MySQL) 및 그래프 리포트

---

### ✅ 결론

본 프로젝트를 통해 스마트 펫 피더를 구현하여
반려동물의 건강과 생활 리듬을 체계적으로 관리할 수 있으며,
향후 더 많은 질환과 기능을 연계하여 **지속 가능한 헬스케어 플랫폼**으로 발전시킬 수 있습니다.

---

## 📌 참고

필요하다면 이 내용을

* **MS Word(.docx) / PDF**
* **PPT(개발 발표용)**
* **Notion용 마크다운**

형식으로 바로 만들어 드릴 수 있습니다.

필요한 포맷을 말씀해주세요 — 즉시 만들어드릴게요 🚀.
(예: "PPT 5장으로 만들어줘" 또는 "Word 양식으로 해줘")
