---
type: source
source_type: official-docs
title: "Expo Official Documentation"
authors: ["Expo Team"]
publisher: "650 Industries (Expo)"
year: 2026
version: "Expo SDK 52–53+"
category: "Official Documentation/Mobile"
url: "https://docs.expo.dev"
status: ingested
tags:
  - expo
  - react-native
  - cng
  - prebuild
  - expo-router
  - eas
  - development-builds
  - official-docs
sources: ["[[2026-Expo Official Documentation]]"]
created: 2026-10-21
updated: 2026-10-21
---

# Expo Official Documentation

> **출처**: [docs.expo.dev](https://docs.expo.dev) (GitHub: [`expo/expo`](https://github.com/expo/expo))
> **주관/게시**: Expo Team (650 Industries, 2026)
> **대상 버전**: `Expo SDK 52–53+` (React Native New Architecture & React 19 지원)
> **분류**: `Official Documentation / Mobile`

---

## 1. 개요 및 핵심 테제 (Core Thesis)

**Expo 공식 문서(`docs.expo.dev`)** 는 Android, iOS, 웹 전반에 걸쳐 유니버설 React 애플리케이션을 개발·빌드·배포하기 위한 오픈소스 프레임워크이자 클라우드 플랫폼인 [[Expo]]의 표준 명세서이다.

React Native 공식 문서(`reactnative.dev`)가 신규 앱 개발의 기본 표준으로 **"Use a Framework (Expo 권장)"** 을 선언함에 따라, 모던 React Native 생태계에서 Expo는 단순한 초보자용 툴킷을 넘어 **업계 표준 프로덕션 인프라**로 자리 잡았다. 현대 Expo의 핵심 철학은 **"네이티브 코드의 선언적 생성과 관리"** 이며, 과거의 취약점이었던 네이티브 제약(`expo eject`)을 **Continuous Native Generation (CNG)** 과 **Development Builds**로 완전히 극복했다.

---

## 2. 현대 Expo 아키텍처의 5대 기둥

### 2.1 Continuous Native Generation (CNG) & `npx expo prebuild`
- **개념**: `ios/` 및 `android/` 네이티브 프로젝트 디렉터리를 개발자가 Git으로 영구 추적·수동 수정하지 않고, **`node_modules`나 `.next` 디렉터리처럼 언제든 완전히 지우고 다시 생성할 수 있는 빌드 산출물(Ephemeral Artifacts)** 로 취급하는 패러다임이다.
- **Config Plugins**: 앱 아이콘, 스플래시 화면, 권한(카메라, 위치), 서드파티 SDK 설정(Google Maps, Firebase 등)을 `app.json` 또는 `app.config.js`에 플러그인 형태로 선언한다.
- **동작 방식**: `npx expo prebuild`를 실행하면 Expo CLI가 순정 네이티브 템플릿 위에 Config Plugins를 적용하여 완전히 구성된 최신 `ios/`, `android/` 폴더를 즉시 합성해 낸다. 업그레이드 시 복잡한 네이티브 마이그레이션 충돌 없이 설정 파일만 갱신하고 다시 prebuild하면 끝난다.

### 2.2 Development Builds (`expo-dev-client`)
- **과거의 한계**: 구형 Expo Go 앱은 사전에 번들링된 Expo SDK 모듈만 실행할 수 있어서, 커스텀 블루투스나 C++ 라이브러리 등 외부 네이티브 모듈을 링크할 수 없었다.
- **해결책**: `expo-dev-client` 라이브러리를 추가하여 **내 프로젝트 전용 커스텀 디버그 빌드(Development Build)** 를 생성한다. 이를 통해 Expo Go의 빠른 JS 새로고침(Fast Refresh) 경험을 100% 누리면서도, Swift, Kotlin, C++로 작성된 **모든 임의의 네이티브 모듈을 제한 없이 사용**할 수 있다.

### 2.3 Expo Router (파일 기반 범용 라우팅)
- **개념**: Next.js의 App Router 모델을 모바일로 가져온 **파일 시스템 기반 라우터(File-based Router)** 이다 (`app/` 디렉터리 기반).
- **특징**:
  - `app/index.tsx`, `app/profile/[id].tsx` 구조만으로 화면 라우팅과 동적 URL 파라미터가 자동 구성된다.
  - 모바일 순정 [[React Navigation]] 스택(`Stack`), 탭(`Tabs`), 드로어(`Drawer`)를 선언적 레이아웃 파일(`_layout.tsx`)로 매핑한다.
  - **딥 링킹(Deep Linking)** 과 웹 표준 URL이 기본 지원되어 단일 라우팅 코드로 웹 브라우저 URL 동기화와 모바일 앱 화면 전환을 완벽히 일치시킨다.

### 2.4 EAS (Expo Application Services)
클라우드 상에서 네이티브 앱의 생명주기 전체를 자동화하는 프로덕션 엔터프라이즈 인프라:
1. **EAS Build**: 로컬에 macOS(Xcode)나 복잡한 Android 빌드 환경이 없어도 클라우드 인프라에서 순정 `.ipa` / `.aab` 바이너리를 병렬 빌드.
2. **EAS Submit**: 빌드된 바이너리를 Apple App Store와 Google Play Store에 명령줄 하나(`eas submit`)로 자동 업로드.
3. **EAS Update (OTA)**: 앱 스토어 심사 대기 없이 버그 수정 JS 번들과 에셋을 사용자 기기에 실시간 무선 업데이트(Over-The-Air) 배포.

### 2.5 Expo Modules API (모던 네이티브 모듈 작성 DSL)
- 과거 Objective-C 매크로(`RCT_EXPORT_MODULE`)나 복잡한 Java JNI 코드로 작성하던 네이티브 모듈을 **Swift 및 Kotlin의 최신 언어 기능을 활용한 선언적 DSL(Domain Specific Language)** 로 작성할 수 있게 해주는 차세대 C++/JSI 통합 API이다.
- 비동기 함수, 동기 함수, 이벤트, 뷰 컴포넌트를 최소한의 보일러플레이트로 빠르게 구현할 수 있다.

---

## 3. React Native New Architecture와의 완벽한 통합

- Expo SDK 52+부터 **React Native New Architecture(Fabric, TurboModules, JSI, Bridgeless Mode)가 기본(Default)** 으로 지원된다.
- Expo SDK의 모든 코어 모듈(카메라, 파일시스템, 미디어 등)이 C++ JSI 및 TurboModules 사양으로 전면 재작성되어, 구형 Bridge 없이 네이티브 UI 스레드와 동기적으로 통신한다.

---

## 4. 기존 도서 서술과의 차이 및 주의점 (Version Drift)

| 항목 | 기존 도서 서술 (2021–2022년 출간 도서들) | Expo 공식 문서 기준 (`docs.expo.dev`) |
|---|---|---|
| **프로젝트 초기화** | `expo init` 또는 `npx react-native init` | **`npx create-expo-app` (프레임워크 사용이 기본 표준)** |
| **네이티브 코드 접근** | `expo eject`로 영구 분리 ([[2022-React and React Native, Fourth Edition]]) | **`expo eject` 완전 폐기** → **Continuous Native Generation (`npx expo prebuild`)** |
| **커스텀 네이티브 모듈** | 사용 불가 (Bare 전환 필수) | **Development Builds (`expo-dev-client`)** 로 제한 없이 사용 가능 |
| **라우팅 모델** | `react-navigation` 명령형 컴포넌트 수동 조립 | **Expo Router** 파일 기반 선언적 라우팅 권장 |
| **클라우드 파이프라인** | 레거시 Expo Build (`expo build:ios/android`) | **EAS (Expo Application Services: Build, Submit, Update)** |
| **런타임 아키텍처** | 구형 비동기 JSON Bridge 기반 | **RN New Architecture & Bridgeless Mode 완벽 통합 지원** |

---

## 5. 파생 및 연관 위키 페이지

- **엔티티**: [[Expo]], [[React Native]], [[React Navigation]]
- **코어 개념**: [[React Native Architecture (Bridge to New Architecture)]], [[Mobile Layout and Styling with Flexbox]]
- **종합 분석**: [[React and React Native Internal Implementation Deep Dive]], [[React Native Learning Roadmap and Evolution (2019-2022)]]
- **관련 소스**: [[2026-React Native Official Documentation]], [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], [[2022-React and React Native, Fourth Edition]]
