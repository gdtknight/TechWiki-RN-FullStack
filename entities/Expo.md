---
type: entity
title: "Expo"
entity_type: "framework"
category: "H. Information Systems/Mobile"
sources:
  - "[[2021-Do it! 리액트 네이티브 앱 프로그래밍]]"
  - "[[2022-React and React Native, Fourth Edition]]"
  - "[[2026-React Native Official Documentation]]"
  - "[[2026-Expo Official Documentation]]"
tags:
  - expo
  - react-native
  - expo-go
  - expo-cli
  - mobile-tooling
  - cng
created: 2026-10-18
updated: 2026-10-21
---

# Expo

## 개요

**Expo**는 [[React Native]] 위에 구축된 **유니버설 React 애플리케이션 프레임워크이자 플랫폼**으로, 복잡한 네이티브 빌드 도구(Xcode, Android Studio, Gradle, CocoaPods) 설정 없이도 단일 JavaScript/TypeScript 코드베이스만으로 iOS, Android, 웹 앱을 빠르게 개발·실기기 테스트·배포할 수 있게 해준다 ([[2021-Do it! 리액트 네이티브 앱 프로그래밍]], Ch 09 p.726; [[2022-React and React Native, Fourth Edition]], Ch 16 p.327–340).

---

## 핵심 구성요소 및 도구 생태계

[[2022-React and React Native, Fourth Edition]] (Ch 16, p.327–340)과 [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 09, p.726–750)에서 다루는 Expo의 핵심 도구:

1. **Expo CLI (`expo init` / `npx create-expo-app`)**: 프로젝트 스캐폴딩(TypeScript 템플릿, 탭 내비게이션 템플릿 등), 개발 서버 실행, 네이티브 호환 패키지 버전 맞춤 설치(`expo install`)를 수행하는 명령줄 도구 (p.328, p.365).
2. **Expo Go (모바일 클라이언트 앱)**: iOS App Store와 Google Play에서 설치하는 컨테이너 앱으로, 네이티브 컴파일이나 애플 개발자 계정 서명 없이도 **QR 코드 스캔 한 번으로 개발 중인 앱을 실제 스마트폰에서 즉시 실행**할 수 있다 (p.329–334).
3. **Expo Snack (`snack.expo.dev`)**: 로컬 환경에 아무것도 설치하지 않고 웹 브라우저 상에서 React Native 코드를 작성·공유하고 가상 기기 또는 실기기에서 즉시 미리 볼 수 있는 온라인 플레이그라운드 (p.335–340).
4. **Expo SDK**: 카메라(`expo-camera`), 위치 정보(`expo-location`), 파일 시스템, 이미지 피커(`expo-image-picker`), 암호화 등 검증된 크로스플랫폼 네이티브 API 모듈 세트 ([[2021-Do it! 리액트 네이티브 앱 프로그래밍]], p.797).

---

## 워크플로 비교: Managed Workflow vs Bare Workflow (React Native CLI)

[[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 01 p.15, Ch 09 p.726–740)과 [[2022-React and React Native, Fourth Edition]] (Ch 16 p.327–328)의 비교 분석:

| 항목 | Expo Managed Workflow | Bare Workflow / React Native CLI (`npx react-native init`) |
|---|---|---|
| **네이티브 폴더 노출** | `ios/`, `android/` 디렉터리가 숨겨져 있으며 Expo가 전체 네이티브 빌드를 대행 관리 | `ios/`(Podfile), `android/`(build.gradle)가 프로젝트에 직접 노출됨 (`2021-Do it! RN` Ch 02) |
| **초기 진입 장벽** | **매우 낮음** (Node.js와 스마트폰의 Expo Go 앱만 있으면 Windows에서도 iOS 앱 테스트 가능) | **높음** (macOS 필수, Xcode, Android Studio, JDK, CocoaPods 직접 설정 필요) |
| **커스텀 네이티브 모듈** | Expo SDK가 지원하는 모듈 중심 (필요 시 `expo eject` / Prebuild로 Bare 전환 가능) | C++/Objective-C/Swift/Java/Kotlin으로 작성된 모든 서드파티 네이티브 모듈 자유롭게 링크 가능 |
| **권장 선택 기준** | 빠른 프로토타이핑, 입문 학습, 표준 네이티브 기능(카메라·위치·푸시) 위주 프로젝트 | 특수 블루투스/하드웨어 SDK 연동, [[React Native Architecture (Bridge to New Architecture)\|네이티브 빌드 파이프라인]]을 밑바닥부터 제어해야 하는 경우 |

---

## 관점 비교 / 출처별 차이 (Lint 2026-10-19)

두 원천 도서(2021–2022)의 "Managed vs Bare(CLI)" 이분법은 현재 공식 문서 기준으로 상당 부분 **구식(Outdated)** 이다.

| 항목 | 도서 서술 ([[2021-Do it! 리액트 네이티브 앱 프로그래밍]] Ch 09, [[2022-React and React Native, Fourth Edition]] Ch 16) | 현재 공식 문서 기준 (`reactnative.dev` Get Started, `docs.expo.dev`) |
|---|---|---|
| **프로젝트 시작** | `expo init` 또는 `npx react-native init`(CLI)를 동등한 선택지로 제시 | `expo init`은 폐기되고 `npx create-expo-app`으로 대체됨. React Native 공식 문서는 신규 앱에 **Expo 같은 프레임워크 사용을 권장**한다. 프레임워크 없는 방식도 허용하지만, 특수한 제약이 있거나 핵심 기능을 직접 해결하려는 경우로 안내한다 |
| **네이티브 코드 접근** | `expo eject`로 Bare 전환 (일방향, 이후 `ios/`·`android/` 수동 유지보수) | **Continuous Native Generation (CNG)**: `app.json`/`app.config.js` + Config Plugins를 근거로 `npx expo prebuild`가 네이티브 폴더를 **빌드 산출물처럼 언제든 재생성**함. `eject` 개념은 사라짐 |
| **커스텀 네이티브 모듈** | Expo Go에 포함된 SDK 모듈로 제한됨 | **Development Build**(`expo-dev-client`) + Expo Modules API로 임의의 네이티브 모듈 사용 가능 → 사실상 "Managed라서 생기는 제약"은 사라짐 |
| **빌드/배포** | 로컬 Xcode/Gradle 빌드 | 로컬 빌드 또는 클라우드 빌드·제출(EAS Build / EAS Submit) 선택 |
| **런타임 아키텍처** | 구형 Bridge 기반 | SDK 52는 신규 프로젝트만 New Architecture 기본, SDK 53은 전체 기본, SDK 54가 Legacy 지원 마지막, **SDK 55부터 New Architecture 전용** (`newArchEnabled` 옵션 삭제) |

### SDK 버전과 React Native 대응 (2026-10-21 기준, `expo.dev/changelog`)
| SDK | 52 | 53 | 54 | 55 | 56 | 57 | 58 |
|---|---|---|---|---|---|---|---|
| **React Native** | 0.76 | 0.79 | 0.81 | 0.83 | 0.85 | 0.86 | 0.88 |

- 최신 SDK 58 (2026-10-15): Expo Router 데이터 로더·서버 렌더링·미들웨어·네이티브 탭 안정화, Expo Modules 2.0 베타, Android 홈 화면 위젯. 새 Expo Go가 스토어에 올라오기 전까지 `create-expo-app`은 SDK 57 프로젝트를 만든다.
- React Compiler는 SDK 54부터 기본으로 켜져 있다 (React 블로그 "React Compiler v1.0").
- 상세 이력: [[2026-Expo Official Documentation]] 3–4절.

> 학습 시에는 도서의 Expo Go·SDK 실습은 그대로 유효하지만, 프로젝트 생성·eject 관련 명령은 위 최신 방식으로 바꿔서 따라 한다. 관련 맥락: [[React Native Learning Roadmap and Evolution (2019-2022)]] §2 (모순 및 Deprecated 항목).

---

## 관련 링크

- **연관 개념**: [[React Native Architecture (Bridge to New Architecture)]]
- **연관 엔티티**: [[React Native]], [[React Navigation]]
- **출처 문헌**: [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 01, Ch 09 p.725–846), [[2022-React and React Native, Fourth Edition]] (Ch 16 p.326–340, Ch 21 p.421), [[2026-React Native Official Documentation]] (Get Started Framework Guide), [[2026-Expo Official Documentation]] (CNG & Prebuild, Development Builds)
