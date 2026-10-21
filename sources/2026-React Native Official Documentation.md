---
type: source
source_type: official-docs
title: "React Native Official Documentation"
authors: ["React Native Team"]
publisher: "React Foundation (Linux Foundation)"
year: 2026
version: "0.76–0.88"
category: "Official Documentation/Mobile"
url: "https://reactnative.dev"
status: ingested
tags:
  - react-native
  - new-architecture
  - jsi
  - fabric
  - turbomodules
  - bridgeless
  - hermes
  - yoga
  - expo
  - official-docs
sources: ["[[2026-React Native Official Documentation]]"]
created: 2026-10-21
updated: 2026-10-21
---

# React Native Official Documentation

> **출처**: [reactnative.dev](https://reactnative.dev) (GitHub: [`react/react-native-website`](https://github.com/react/react-native-website). 0.86 릴리스 때 `facebook` 조직에서 `react` 조직으로 이전)
> **주관/게시**: React Native Team. 2026-02-24부터 React Foundation(Linux Foundation 산하)이 소유하며, 그 전에는 Meta가 소유했다 ([[2026-React Official Documentation]] 4절)
> **대상 버전**: `0.76–0.88` (0.76 New Architecture 기본화 → 0.82 New Architecture 전용 → 0.88 최신, 2026-10-19 기준)
> **분류**: `Official Documentation / Mobile`

---

## 1. 개요 및 핵심 테제 (Core Thesis)

**React Native 공식 문서(`reactnative.dev`)** 는 단일 JavaScript/TypeScript 코드베이스로 iOS와 Android의 순정 네이티브 애플리케이션을 구축하기 위한 크로스플랫폼 프레임워크인 [[React Native]]의 표준 명세이자 최신 공식 레퍼런스이다.

React Native의 핵심 비전은 **"Learn once, write anywhere(한 번 배워서 어디서나 작성한다)"** 이다. 웹과 동일한 선언형 React 컴포넌트 모델과 JavaScript 생태계를 누리면서도, 웹뷰(WebView) 에뮬레이션이 아닌 **실제 플랫폼의 네이티브 위젯(iOS의 `UIView`, Android의 `android.view`)** 을 직접 구동한다. 특히 **0.76에서 New Architecture를 기본값(Default)으로 전환**했고("Starting from React Native 0.76, the New Architecture is enabled by default in your projects."), 이때는 옵트아웃과 구형 모듈용 interop layer가 남아 있었다. **0.82부터 New Architecture로만 실행**되며("the first React Native that runs entirely on the New Architecture"), **0.84부터 Legacy Architecture 코드를 단계적으로 제거**하고 있다 (상세 이력은 5절).

---

## 2. New Architecture & Bridgeless Mode 공식 명세 (RN 0.76+ Default)

공식 문서(`reactnative.dev/architecture/overview`)가 정의하는 모던 React Native의 5대 핵심 기둥:

### 2.1 JSI (JavaScript Interface)
- 특정 JS 엔진(JSC, V8, Hermes)에 구애받지 않는 C++ 추상화 레이어로, **C++ 객체(`jsi::HostObject`)를 JavaScript 런타임에 직접 바인딩**한다.
- 과거 Bridge처럼 모든 데이터를 문자열 JSON으로 직렬화/역직렬화할 필요 없이, 메모리 주소 참조를 통해 $O(1)$의 비용으로 네이티브 메서드를 동기적(Synchronous) 또는 비동기적으로 직접 호출한다.

### 2.2 Fabric Renderer (신규 렌더링 엔진)
- 기존 브릿지 기반의 UI Manager를 완전히 대체하는 C++ 기반 렌더러.
- React의 재조정 파이프라인과 직접 통합되어, **JS 힙이 아닌 C++ 힙 메모리에 불변(Immutable) `ShadowNode` 트리를 직접 구축**한다.
- Meta가 개발한 크로스플랫폼 레이아웃 엔진인 **Yoga(C++)** 와 동일한 C++ 메모리 공간에서 직접 통신하여 레이아웃을 계산하고, 이전 트리와 새 트리의 차이(Diff)만을 **Mounting Transaction**으로 묶어 네이티브 UI 메인 스레드에 원자적으로 적용한다.

### 2.3 TurboModules & Codegen
- **TurboModules**: 네이티브 OS API(카메라, 위치, 파일시스템 등)를 JSI로 직접 바인딩하며, 앱 시작 시 일괄 초기화되던 기존 네이티브 모듈과 달리 **JS에서 실제로 모듈을 호출하는 시점에 지연 로딩(Lazy Initialization)** 된다.
- **Codegen**: Flow 또는 TypeScript로 작성된 인터페이스 명세 파일로부터 C++, Java(JNI), Objective-C 헤더와 래퍼 코드를 빌드 타임에 자동 생성하여, 런타임 타입 불일치로 인한 크래시를 원천 차단한다.

### 2.4 Hermes JS Engine 기본 채택
- 모바일 OS의 메모리 제약과 앱 시동 속도에 특화된 경량 JS 엔진.
- 코드를 빌드 타임에 **HBC(Hermes Bytecode)** 로 사전 컴파일(AOT)하여 배포함으로써, 앱 실행 시 파싱 및 컴파일 단계를 생략하여 TTI(Time to Interactive)를 극대화한다.
- 0.70부터 기본 엔진이며, **0.84부터 Hermes V1이 iOS·Android 기본 엔진**이다: "Hermes V1 is now the default JavaScript engine for React Native on both iOS and Android, following the initial experimental opt-in in React Native 0.82." 기존 Hermes 사용자는 자동으로 V1을 받는다 ("If you're already using Hermes (the default since 0.70), you'll automatically get Hermes V1.").
- JavaScriptCore(JSC)는 0.79부터 커뮤니티 패키지 `@react-native-community/javascriptcore`로 이관이 진행되었고, 0.81 릴리스 글은 "support for the JavaScriptCore (JSC) engine has moved to a community-maintained package"라고 밝힌다.

### 2.5 Bridge 의존성 제거 (Bridgeless)
- 공식 블로그 "The New Architecture is Here"(2024-10-23)는 "In the New Architecture, we've also fully removed React Native's dependency on the bridge, replacing it with direct, efficient communication between JavaScript and native code using JSI"라고 설명하고, 효과로 "Removing the bridge improves startup time by avoiding bridge initialization."을 든다.
- 같은 글은 "The bridge remains for backward compatibility to support gradual migration to the New Architecture."라고도 밝힌다. 0.76 시점의 Bridgeless는 새 아키텍처의 경로가 Bridge에 의존하지 않는다는 뜻이며, Bridge 코드 자체가 사라진 것은 아니다.
- 구형 라이브러리는 자동 interop layer로 동작한다. 단 "it does not support accessing custom Shadow Nodes or concurrent features"라는 제약이 있다. 0.84에서도 "the Interop Layer code required for compatibility remains in place."

---

## 3. 웹 표준 정렬 및 레이아웃/스타일링 진화

### 3.1 웹 표준 정렬 Event Loop & `useLayoutEffect` 동기 레이아웃
- React Native의 JS Event Loop가 웹 브라우저의 Microtask 처리 표준과 일치하도록 개편되어, React 18/19의 **동시성 기능(`Suspense`, `useTransition`, Automatic Batching)** 이 모바일 네이티브 위젯에서도 매끄럽게 구동된다.
- 구형 브릿지에서는 불가능했던 **동기식 레이아웃 측정(`useLayoutEffect`)** 이 Fabric에서 공식 지원되어, 화면이 페인트되기 전에 네이티브 뷰의 실제 위치·크기를 읽고 보정할 수 있게 되어 화면 깜빡임(Flicker)이 근본적으로 해결되었다.

### 3.2 최신 CSS 스타일링 속성 표준 지원
- **`boxShadow`**: 기존의 iOS 전용(`shadowOffset`/`shadowColor`) 및 Android 전용(`elevation`)으로 분리되어 있던 그림자 속성이 표준 웹 CSS 문법(`boxShadow: '0 4px 6px rgba(0,0,0,0.1)'`)으로 통합 지원된다.
- **`filter`**: 블러, 밝기 등 시각 효과 표준화.
- **Flexbox `gap` (rowGap, columnGap)**: 마진 연쇄 계산 없이 아이템 간 여백 제어.
- **`display: 'contents'`**: 레이아웃 래퍼 없이 자식 노드만 직렬 배치.

---

## 4. 환경 설정 표준의 대전환: "Use a Framework" (Expo 공식 기본 권장)

공식 문서의 시작 가이드(`reactnative.dev/docs/environment-setup`)는 현대 React Native 생태계의 가장 큰 정책 변화를 보여준다:

1. **프레임워크(Framework) 사용의 기본화**:
   - 과거에는 네이티브 CLI(`npx react-native init`)가 1순위였으나, 현재 공식 문서는 **[[Expo]]와 같은 프로덕션 프레임워크를 사용할 것을 기본으로 권장**한다: "However, if you're building a new app with React Native, we recommend using a Framework."
   - 프레임워크 없이 시작하는 것도 공식적으로 허용된다. 다만 핵심 기능을 직접 구현하거나 라이브러리를 조합해야 한다: "If your app has unusual constraints that are not served well by a Framework, or you prefer to solve these problems yourself, you can make a React Native app without a Framework using Android Studio, Xcode."
2. **`expo eject`의 폐기와 Continuous Native Generation (CNG)**:
   - 과거 Expo의 치명적 한계였던 `expo eject`(일방향 이탈 후 네이티브 폴더 수동 관리)는 완전히 폐기되었다.
   - **`npx expo prebuild` (CNG)** 에 의해 `ios/`와 `android/` 디렉터리가 `node_modules`와 같은 **재생성 가능한 빌드 산출물**로 취급되며, Config Plugins를 통해 임의의 서드파티 네이티브 모듈을 Managed 상태 그대로 완벽히 통합할 수 있다.

---

## 5. 릴리스 이력: 0.79–0.88 (공식 블로그 `reactnative.dev/blog` 기준)

| 버전 (발표일) | 핵심 변경 | 원문 근거 |
|---|---|---|
| **0.79** (2025-04-08) | Metro 0.82 (`package.json` `exports` 해석), JSC 커뮤니티 패키지 이관 시작, Chrome 원격 JS 디버깅 제거 | "we're removing Remote JS Debugging via Chrome." |
| **0.80** (2025-06-12) | React 19.1.0, Legacy Architecture 동결, JS deep import 사용 중단(deprecation), iOS 의존성 사전 빌드(실험) | "We won't be developing new bugfixes or features in the Legacy Architecture anymore" |
| **0.81** (2025-08-12) | Android 16(API 36) 기본 타깃, `<SafeAreaView>` 사용 중단(`react-native-safe-area-context` 권장), `edgeToEdgeEnabled` Gradle 속성, 사전 컴파일 iOS 빌드(실험) | "the legacy `<SafeAreaView>` component is deprecated" |
| **0.82** (2025-10-08) | **New Architecture 전용**. `newArchEnabled=false`·`RCT_NEW_ARCH_ENABLED=0` 무시, interop layer는 유지, Hermes V1 실험 옵트인 | "the first React Native that runs entirely on the New Architecture" |
| **0.83** (2025-12-10) | React 19.2 (`<Activity>`, `useEffectEvent`), DevTools 기능 추가, Web Performance API 안정화, IntersectionObserver(Canary) | "bringing the new `<Activity>` and `useEffectEvent` APIs to React Native" |
| **0.84** (2026-02-11) | **Hermes V1 기본**, iOS 사전 컴파일 바이너리 기본, Legacy Architecture 코드 제거 진행(iOS 빌드에서 제외), Node.js 22.11 이상 필요 | "React Native 0.84 requires Node.js v22.11 or later." |
| **0.85** (2026-04-07) | Software Mansion과 만든 Shared Animation Backend(실험, 0.85.1부터), Metro TLS, Jest 프리셋이 `@react-native/jest-preset`으로 분리(Breaking) | "React Native 0.85 introduces the new Shared Animation Backend, built in collaboration with Software Mansion." |
| **0.86** (2026-06-11) | 저장소를 `react` GitHub 조직으로 이전, Android 15+ edge-to-edge 수정, DevTools 라이트·다크 모드 에뮬레이션 | "has moved from the `facebook` GitHub organization to the `react` organization." |
| **0.87** (2026-08-11) | Strict TypeScript API가 기본 공개 API가 됨, Metro 0.87, Swift Package Manager 실험 지원, AGP 9 | "React Native's public JavaScript API is now the Strict TypeScript API." |
| **0.88** (2026-10-19) | 이벤트 디스패치를 W3C `EventTarget` 기반으로 전환, `fontVariationSettings`(가변 폰트), iOS UIScene(SceneDelegate) 생명주기 지원, Java TurboModules `ArrayBuffer`, React 19.3.0 | "React Native now dispatches events through the W3C `EventTarget` API by default" |

- 0.88의 SceneDelegate 지원 배경: "Starting with the iOS 27 SDK, Apple requires the UIScene lifecycle: UIKit apps built with Xcode 27 will fail to launch."
- 레거시 제거의 단계별 의미는 [[React Native Architecture (Bridge to New Architecture)]] 4절의 레거시 제거 타임라인을 참고한다.

---

## 6. 기존 도서 서술과의 차이 및 주의점 (Version Drift)

| 항목 | 기존 도서 서술 (2019–2022년 출간 도서들) | RN 0.76+ 공식 문서 기준 (`reactnative.dev`) |
|---|---|---|
| **JS 엔진** | 초기에는 JSCore만 지원, 0.60부터 Hermes 사용 시작, 0.64부터 iOS도 지원 ([[2022-React and React Native, Fourth Edition]] PDF p.314, 인쇄 p.291) | Hermes 기본 (0.70+, 0.84 릴리스 글 기준) → **Hermes V1 기본 (0.84+)**, JSC는 커뮤니티 패키지로 이관 |
| **Safe Area 처리** | 코어 `<SafeAreaView>` (iOS 전용) | 0.81부터 사용 중단, `react-native-safe-area-context` 권장 |
| **통신 아키텍처** | 비동기 JSON Bridge (`MessageQueue.js`) ([[2019-React Native in Action]], [[2021-Do it! 리액트 네이티브 앱 프로그래밍]]) | **JSI 기반 직접 메모리 바인딩 & Bridgeless Mode 기본화** |
| **렌더링 엔진** | 레거시 UI Manager (JS가 JSON 명령 전송) | **Fabric Renderer** (C++ Shadow Tree + Yoga C++ 직접 레이아웃) |
| **모듈 호출** | 시작 시 모든 Native Modules 일괄 초기화 | **TurboModules & Codegen** (정적 타입 보장 및 필요 시 지연 로딩) |
| **레이아웃 측정** | `onLayout` 비동기 측정으로 깜빡임 유발 | **`useLayoutEffect` 동기 섀도 트리 측정** 지원 |
| **시작 도구** | `react-native init` 또는 `expo init` | **`npx create-expo-app` (Expo 프레임워크 공식 권장)** |
| **네이티브 확장** | `expo eject`로 Bare 전환 필수 ([[2022-React and React Native, Fourth Edition]]) | **Continuous Native Generation (`npx expo prebuild`)** |
| **코어 내장 모듈** | `AsyncStorage`, `NetInfo` 코어 내장 | **Lean Core 정책**: 전량 커뮤니티 패키지로 분리 |

---

## 7. 파생 및 연관 위키 페이지

- **코어 개념**: [[React Native Architecture (Bridge to New Architecture)]], [[Mobile Layout and Styling with Flexbox]], [[React Native Gesture and Animation System]], [[Offline-First and Mobile State Management]]
- **엔티티**: [[React Native]], [[Expo]], [[React Navigation]], [[React]]
- **관련 공식 소스**: [[2026-Expo Official Documentation]]
- **종합 분석**: [[React and React Native Internal Implementation Deep Dive]], [[Web React vs React Native Shared Core and Differences]], [[React Native Learning Roadmap and Evolution (2019-2022)]]
