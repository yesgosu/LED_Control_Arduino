# LED Control Arduino (ESP8266 + Firebase)

ESP8266(NodeMCU)과 Firebase 실시간 데이터베이스를 이용한 IoT LED 원격 제어 시스템

## 📌 프로젝트 개요

이 프로젝트는 Firebase 실시간 데이터베이스를 통해 ESP8266에 연결된 LED를 원격으로 제어하는 IoT 애플리케이션입니다. 
모바일 앱이나 웹 인터페이스에서 Firebase DB 값을 변경하면, ESP8266이 이를 감지하여 LED를 ON/OFF 할 수 있습니다.

## 🛠️ 하드웨어 구성

- **마이크로컨트롤러**: ESP8266 (NodeMCU)
- **LED**: D7 핀에 연결
- **전원**: USB 5V

### 회로 연결
```
ESP8266 (NodeMCU)
├─ D7 → LED (+) → 저항(220Ω) → GND
└─ GND → LED (-)
```

## 📦 필요한 라이브러리

Arduino IDE의 라이브러리 매니저에서 설치:

1. **ESP8266 보드 패키지**: v2.7.4
   - `파일` → `환경설정` → `추가 보드 매니저 URLs`
   - `http://arduino.esp8266.com/stable/package_esp8266com_index.json` 추가
   - `도구` → `보드` → `보드 매니저`에서 "ESP8266" 검색 후 v2.7.4 설치

2. **Firebase Arduino Library**
   - `스케치` → `라이브러리 포함하기` → `라이브러리 관리`
   - "FirebaseArduino" 검색 후 설치

3. **ESP8266WiFi Library** (보드 패키지에 포함됨)

## ⚙️ 설정 방법

### 1. Firebase 프로젝트 설정

1. [Firebase Console](https://console.firebase.google.com/)에서 새 프로젝트 생성
2. `Realtime Database` 생성
3. 데이터베이스 URL 확인 (예: `xxxxx.firebaseio.com`)
4. `프로젝트 설정` → `서비스 계정` → `데이터베이스 비밀번호` 확인

### 2. 코드 설정

`ledweb.ino` 파일에서 다음 항목을 본인의 정보로 수정:
```cpp
#define FIREBASE_HOST "your-project.firebaseio.com"
#define FIREBASE_AUTH "your-firebase-secret-key"
#define WIFI_SSID "your-wifi-ssid"
#define WIFI_PASSWORD "your-wifi-password"
```

### 3. 업로드

1. Arduino IDE에서 보드 설정:
   - `도구` → `보드` → `NodeMCU 1.0 (ESP-12E Module)`
   - `도구` → `Upload Speed` → `115200`
2. COM 포트 선택
3. 업로드 버튼 클릭

## 🚀 사용 방법

### Firebase 데이터베이스 구조
```json
{
  "LED_STATUS": "OFF"
}
```

### LED 제어

1. **Firebase Console에서 직접 제어**:
   - Firebase Console → Realtime Database
   - `LED_STATUS` 값을 `"ON"` 또는 `"OFF"`로 변경

2. **웹/앱에서 제어** (별도 구현 필요):
   - Firebase SDK를 사용하여 웹 또는 모바일 앱 개발
   - `LED_STATUS` 값을 업데이트하는 버튼 구현

### Serial Monitor 출력
```
Connecting to your-wifi-ssid...
Connected to your-wifi-ssid
Led Turned ON
Led Turned OFF
```

## 📊 동작 원리
```
사용자 → Firebase DB 값 변경 → ESP8266이 감지 (500ms 폴링) → LED 제어
```

1. ESP8266이 WiFi에 연결
2. Firebase 실시간 데이터베이스에 연결
3. 500ms마다 `LED_STATUS` 값 확인
4. 값에 따라 LED ON/OFF

1. **별도 설정 파일 사용**:
```cpp
// config.h 파일 생성 (이 파일은 .gitignore에 추가)
#ifndef CONFIG_H
#define CONFIG_H

#define FIREBASE_HOST "your-project.firebaseio.com"
#define FIREBASE_AUTH "your-secret-key"
#define WIFI_SSID "your-ssid"
#define WIFI_PASSWORD "your-password"

#endif
```

2. **.gitignore 파일 생성**:
```
config.h
*.bak
*.tmp
```

3. **Firebase 보안 규칙 설정**:
```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

## 🐛 문제 해결

### WiFi 연결 실패
- SSID와 비밀번호가 정확한지 확인
- ESP8266이 2.4GHz WiFi만 지원함 (5GHz 불가)

### Firebase 연결 실패
- Firebase URL에 `https://` 제외하고 입력
- Firebase Auth 키가 정확한지 확인
- Firebase 보안 규칙 확인

### LED가 동작하지 않음
- LED 극성 확인 (긴 다리가 +)
- 저항 연결 확인 (220Ω 권장)
- D7 핀 연결 확인

## 🔧 개선 가능 사항

- [ ] Firebase Stream 방식으로 변경 (폴링 → 실시간)
- [ ] WiFi 재연결 로직 추가
- [ ] 여러 개의 LED 제어
- [ ] PWM을 이용한 밝기 조절
- [ ] RGB LED 색상 제어
- [ ] 웹/모바일 제어 앱 개발

## 📝 라이선스

MIT License


# OS
.DS_Store
Thumbs.db
