---
type: source
title: "React Native in Action"
authors:
  - "Nader Dabit"
publisher: "Manning Publications"
year: 2019
category: "H. Information Systems/Mobile"
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/Mobile/2019-React Native in Action-Nader Dabit-Manning.pdf"
status: ingested
tags:
  - react-native
  - mobile-development
  - cross-platform
  - javascript
  - redux
  - styling
sources: ["[[2019-React Native in Action]]"]
created: 2026-10-18
updated: 2026-10-18
---

# React Native in Action (2019)

> **서지 정보**: Nader Dabit | Manning Publications (2019) | 321 Pages
> **분류**: `H. Information Systems / Mobile`
> **원천 PDF**: [2019-React Native in Action-Nader Dabit-Manning.pdf](file:///Users/yoshin/Documents/MarginNote4/H.%20Information%20Systems/Mobile/2019-React%20Native%20in%20Action-Nader%20Dabit-Manning.pdf)

---

## 핵심 요약 (Core Thesis)

웹뷰(WebView) 기반 하이브리드 앱(Cordova 등)의 성능 한계를 극복하고, 단일 JavaScript 코드베이스로 iOS(`UIView`)와 Android(`android.view`)의 **실제 네이티브 UI 위젯을 렌더링하는 [[React Native]]의 핵심 원리와 실전 앱 구축법**을 다루는 입문·실전서다.

저자 Nader Dabit은 React의 컴포넌트 생명주기와 상태(State)/속성(Props) 모델에서 출발하여, [[Mobile Layout and Styling with Flexbox|StyleSheet 및 Flexbox 스타일링]], [[React Navigation|네비게이션]], [[React Native Gesture and Animation System|Animated API 기반 애니메이션]], [[Offline-First and Mobile State Management|Redux 데이터 아키텍처]], 그리고 iOS/Android 플랫폼별 고유 API 연동까지 단계별로 설명한다. 2019년 출간작이므로 클래스 컴포넌트 비중이 높으나, React Native의 기본 철학과 코어 API 레퍼런스를 이해하는 데 탄탄한 기초를 제공한다 ([[React Native Learning Roadmap and Evolution (2019-2022)]] 참조).

---

## 챕터별 핵심 내용 요약

### Part 1: Getting Started with React Native (Ch 1 – Ch 3)
- **Ch 1. Getting started with React Native (p.27–50)**
  - **[[React Native]] 정의 및 등장 배경 (p.27–29)**: DOM이 아닌 네이티브 플랫폼 위젯으로 컴파일/브릿징되는 원리.
  - **[[React Native Architecture (Bridge to New Architecture)|Bridge 아키텍처]] 기초 (p.30–34)**: JavaScript 스레드와 네이티브 메인(UI) 스레드가 비동기 브릿지를 통해 통신하며 Virtual DOM diff 결과를 네이티브 뷰에 반영하는 메커니즘.
  - 코어 컴포넌트(`View`, `Text`, `TouchableHighlight` 등)와 프로젝트 구조(`index.js`, `AppRegistry`).
- **Ch 2. Understanding React (p.51–68)**
  - State와 Props를 통한 데이터 흐름 제어, React 컴포넌트 명세와 생명주기(`componentDidMount`, `shouldComponentUpdate` 등).
- **Ch 3. Building your first React Native app (p.69–96)**
  - Todo 애플리케이션을 처음부터 끝까지 구현하며 입력 처리(`TextInput`), 상태 변경, 탭 필터링 실습.

### Part 2: Developing Applications in React Native (Ch 4 – Ch 8)
- **Ch 4. Introduction to styling (p.99–138)**
  - 인라인 스타일 vs `StyleSheet.create`를 활용한 스타일 분리 및 조직화 ([[Mobile Layout and Styling with Flexbox]]).
  - 색상, 테두리(`borderRadius`), 마진/패딩, 그림자(`shadowPropTypesIOS` vs Android `elevation`), 텍스트 스타일링과 프로필 카드 컴포넌트 구현.
- **Ch 5. Styling in depth (p.139–168)**
  - 플랫폼별 크기/해상도 대응, 변환(`transform`: 이동, 회전, 스케일링).
  - **Flexbox 심화 (p.152–167)**: `flexDirection`, `justifyContent`, `alignItems`, `flexWrap`을 활용한 모바일 레이아웃 설계.
- **Ch 6. Navigation (p.169–185)**
  - 모바일 내비게이션의 핵심 패턴(Stack, Tab, Drawer)과 [[React Navigation]]을 활용한 여행 도시 가이드 앱 구현.
- **Ch 7. Animations (p.186–202)**
  - `Animated` API의 4가지 핵심 요소(`Animated.Value`, `Animated.timing`/`spring`, 애니메이션 가능 컴포넌트, `interpolate`) 및 시퀀스/병렬(`sequence`, `parallel`, `stagger`) 조합 ([[React Native Gesture and Animation System]]).
- **Ch 8. Using the Redux data architecture library (p.203–220)**
  - Context API의 한계와 Redux(Store, Action, Reducer, `connect`, `Provider`)를 결합한 전역 도서 목록 상태 관리 ([[Offline-First and Mobile State Management]]).

### Part 3: API Reference & Platform-Specifics (Ch 9 – Ch 11)
- **Ch 9. Implementing Cross-Platform APIs (p.223–245)**
  - `Alert`, `AppState`, `AsyncStorage`, `Clipboard`, `Dimensions`, `Geolocation`, `Keyboard`, `NetInfo`, `PanResponder` 등 크로스플랫폼 내장 API 실전 예제.
- **Ch 10. Implementing iOS-specific components and APIs (p.246–265)**
  - `Platform` 모듈과 `.ios.js`/`.android.js` 파일 확장자 분기, `DatePickerIOS`, `PickerIOS`, `ProgressViewIOS`, `SegmentedControlIOS`, `TabBarIOS`, `ActionSheetIOS`.
- **Ch 11. Implementing Android-specific components and APIs (p.266–284)**
  - `DrawerLayoutAndroid`, `ToolbarAndroid`, `ViewPagerAndroid`, `DatePickerAndroid`, `TimePickerAndroid`, `ToastAndroid`, `BackHandler`, `PermissionsAndroid`.

### Part 4: Bringing It All Together (Ch 12)
- **Ch 12. Building a Star Wars app using cross-platform components (p.287–308)**
  - 외부 REST API(SWAPI) 호출(`fetch`), `FlatList`, `Modal`, `ActivityIndicator`, [[React Navigation]]을 통합한 완성형 앱 구축.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[React Native Architecture (Bridge to New Architecture)]] — 초기 비동기 Bridge와 JS/UI 스레드 분리 원리 (Ch 1)
- [[Mobile Layout and Styling with Flexbox]] — `StyleSheet`와 Flexbox 레이아웃 기초 및 심화 (Ch 4–5)
- [[React Native Gesture and Animation System]] — 내장 `Animated` API와 `PanResponder` 활용 (Ch 7, Ch 9)
- [[Offline-First and Mobile State Management]] — Redux 아키텍처 및 `AsyncStorage`, `NetInfo` 활용 (Ch 8–9)

### Entities
- [[React Native]] — 코어 컴포넌트와 크로스플랫폼 및 플랫폼 특화 API 체계 (Ch 1, Ch 9–11)
- [[React Navigation]] — Stack, Tab, Drawer 내비게이션 구현 패턴 (Ch 6)

### Syntheses
- [[React Native Learning Roadmap and Evolution (2019-2022)]] — 2019년 클래스 컴포넌트/레거시 코어 API에서 2021–2022년 모던 생태계(Hooks, TypeScript, Community 패키지 분리, New Architecture)로의 변화 비교
