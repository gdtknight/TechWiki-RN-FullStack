---
type: source
title: "React and React Native, Fourth Edition"
authors:
  - "Adam Boduch"
  - "Roy Derks"
  - "Mikhail Sakhniuk"
publisher: "Packt Publishing"
year: 2022
category: "E. Programming Languages/JavaScript-TypeScript"
pdf_path: "/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2022-React and React Native, Fourth Edition-Adam Boduch,Roy Derks 외-Packt Publishing.pdf"
status: ingested
tags:
  - react
  - react-18
  - react-native
  - new-architecture
  - jsi
  - fabric
  - turbomodules
  - hermes
  - reanimated
  - graphql
  - expo
sources: ["[[2022-React and React Native, Fourth Edition]]"]
created: 2026-10-18
updated: 2026-10-18
---

# React and React Native, Fourth Edition (2022)

> **서지 정보**: Adam Boduch, Roy Derks, Mikhail Sakhniuk | Packt Publishing (2022) | 606 Pages
> **분류**: `E. Programming Languages / JavaScript-TypeScript`
> **원천 PDF**: [2022-React and React Native, Fourth Edition-Adam Boduch,Roy Derks 외-Packt Publishing.pdf](file:///Users/yoshin/Documents/MarginNote4/E.%20Programming%20Languages/JavaScript-TypeScript/2022-React%20and%20React%20Native,%20Fourth%20Edition-Adam%20Boduch,Roy%20Derks%20%EC%99%B8-Packt%20Publishing.pdf)

---

## 핵심 요약 (Core Thesis)

**React 18 웹 개발(Part 1)** 과 **모던 [[React Native]] 모바일 개발(Part 2)**, 그리고 웹과 모바일을 아우르는 **상태 관리 및 GraphQL 데이터 아키텍처(Part 3)** 를 하나의 통합된 시각으로 다루는 종합 가이드다.

특히 이 책의 **Chapter 15 ("React Native under the Hood", PDF p.310–325)** 와 **Chapter 25 ("Using Animations", PDF p.484–495)** 는 기존 입문서들이 생략하거나 구형 아키텍처에 머물러 있던 한계를 넘어, **Hermes JS 엔진, JSI(JavaScript Interface), Fabric 렌더러, TurboModules, Codegen으로 구성된 [[React Native Architecture (Bridge to New Architecture)|React Native New Architecture]]** 와 UI 스레드 기반 애니메이션 라이브러리인 **[[React Native Gesture and Animation System|React Native Reanimated]]** 를 명확하게 해설한다는 점에서 가장 현대적인 아키텍처 가치를 지닌다.

---

## 챕터별 핵심 내용 요약

### Part 1: React (Web Foundation, Ch 1 – Ch 13, PDF p.24–301)
- **Ch 1 – Ch 4. React 18 기초, JSX, State/Context, Hooks (p.26–113)**
  - React 18의 동시성(Concurrency), 자동 배칭(Automatic Batching), 선언적 UI 패러다임과 Hooks(`useState`, `useEffect`, `useContext`, `useReducer`) 활용.
- **Ch 5 – Ch 10. 이벤트, 컴포넌트 리팩토링, 생명주기, 라우팅, 코드 스플리팅 (p.114–242)**
  - 모놀리식 컴포넌트를 재사용 가능한 Feature/Utility 컴포넌트로 분리(Ch 6), Error Boundaries(Ch 7), `React.lazy`와 `Suspense`를 이용한 코드 스플리팅(Ch 10).
- **Ch 11 – Ch 13. SSR, UI 프레임워크(MUI), 고성능 상태 업데이트 (p.244–301)**
  - 서버 사이드 렌더링(SSR), React 18의 `startTransition` 및 비동기 상태 우선순위 제어(Ch 13).

### Part 2: React Native (Mobile Development, Ch 14 – Ch 28, PDF p.302–545)
- **Ch 14. Why React Native? (p.304–309)**
  - 모바일 웹 앱(PWA/WebView) 대비 네이티브 렌더링의 UX 우위와 Android/iOS 크로스플랫폼 코드 공유의 경제성.
- **Ch 15. React Native under the Hood (p.310–325)**
  - **[[React Native Architecture (Bridge to New Architecture)]] 핵심 분석**:
    - **기존 아키텍처 (p.311–317)**: 3대 스레드(JS Thread, Yoga 엔진 기반 Shadow Thread, Native UI Thread), C++ 비동기 JSON 직렬화 **Bridge**의 동작 원리와 대량 스크롤(`onScroll`) 시 발생하는 브릿지 병목 현상.
    - **Hermes 엔진 (p.314)**: JavaScriptCore(JSC)를 대체하여 앱 초기 구동 속도 개선, 번들 크기 축소, 메모리 절감을 달성한 AOT 바이트코드 VM.
    - **New Architecture (p.318–319)**: 브릿지를 제거하고 C++ `HostObject`를 통해 JS와 Native가 동기적·직접적으로 참조하는 **JSI(JS Interface)**, C++ Shadow Tree를 직접 다루는 새 렌더러 **Fabric**, 필요한 시점에 지연 로딩(Lazy-loading)되는 **TurboModules**, 정적 타입 인터페이스를 자동 생성하는 **Codegen**.
  - 코어 패키지 경량화(Lean Core) 전략과 필수 서드파티 생태계(React Navigation, NativeBase, UI Kitten, React Native Paper, Bootsplash) 소개 (p.319–322).
- **Ch 16. Kick-Starting React Native Projects (p.326–341)**
  - React Native CLI vs [[Expo]] CLI, Expo Go 앱을 통한 실기기 즉시 테스트와 웹 기반 Expo Snack 활용.
- **Ch 17. Building Responsive Layouts with Flexbox (p.342–363)**
  - [[Mobile Layout and Styling with Flexbox]]: `StyleSheet`와 `styled-components/native`를 활용한 CSS-in-JS 스타일링, Flexbox 기반의 다단 그리드 및 리스트 레이아웃 설계.
- **Ch 18. Navigating Between Screens (p.364–385)**
  - [[React Navigation]] v6의 `@react-navigation/native-stack` (`createNativeStackNavigator`), 라우트 파라미터, 헤더 제어, Tab 및 Drawer 내비게이션.
- **Ch 19 – Ch 23. 리스트(FlatList), 진행률(Progress), 지도/위치(Maps), 사용자 입력, 모달 (p.386–465)**
  - `FlatList` 정렬·필터링·무한 스크롤(Lazy Loading)·Pull-to-Refresh(Ch 19), `react-native-maps`와 위치 정보 연동(Ch 21), 텍스트·피커·스위치·날짜/시간 입력(Ch 22), 확인 다이얼로그·토스트·액티비티 모달(Ch 23).
- **Ch 24 – Ch 25. 제스처와 애니메이션 (p.466–495)**
  - 터치 피드백(`Pressable`, `TouchableOpacity`)과 스와이프 가능 컴포넌트(Ch 24).
  - **[[React Native Gesture and Animation System|React Native Reanimated]] (Ch 25, p.484–495)**: 구형 `Animated` API가 브릿지 비동기 통신으로 인해 최소 1프레임(16ms) 이상 지연되는 문제를 해결하기 위해, New Architecture 기반의 UI 스레드에서 비즈니스 로직을 직접 처리하는 `react-native-reanimated` (`useSharedValue`, `useAnimatedStyle`, Layout Animations) 도입.
- **Ch 26 – Ch 28. 이미지 제어, 오프라인 지원, NativeBase UI (p.496–545)**
  - 이미지 리사이징, 지연 로딩, 벡터 아이콘(Ch 26).
  - **[[Offline-First and Mobile State Management|Going Offline]] (Ch 27, p.512–527)**: `@react-native-community/netinfo`를 통한 실시간 네트워크 연결 감지와 `@react-native-async-storage/async-storage` 기반 로컬 저장소 캐싱 및 재연결 시 상태 동기화 패턴.
  - NativeBase를 활용한 유니버설 디자인 시스템 구축(Ch 28).

### Part 3: React Architecture (State & GraphQL, Ch 29 – Ch 31, PDF p.546–589)
- **Ch 29. Handling Application State (p.548–563)**
  - 웹과 모바일 앱에서 상태를 계층화하는 전략(로컬 State, Context API, 외부 상태 관리 라이브러리).
- **Ch 30 – Ch 31. Why GraphQL? & Building a GraphQL React App (p.564–589)**
  - 선언적 데이터 페칭으로서의 GraphQL(Schema, Query, Mutation)과 **Apollo Client**(`useQuery`, `useMutation`, 인메모리 캐시)를 활용한 상태 관리 통합 ([[Offline-First and Mobile State Management]]).

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[React Native Architecture (Bridge to New Architecture)]] — 3대 스레드, Yoga, Hermes, Bridge 병목, JSI, Fabric, TurboModules, Codegen (Ch 15)
- [[Mobile Layout and Styling with Flexbox]] — `StyleSheet`, `styled-components/native`, Flexbox 반응형 레이아웃, NativeBase (Ch 17, Ch 28)
- [[React Native Gesture and Animation System]] — `Animated` API의 16ms 브릿지 지연 문제와 `React Native Reanimated` UI 스레드 애니메이션 (Ch 24–25)
- [[Offline-First and Mobile State Management]] — `NetInfo` + `AsyncStorage` 오프라인 동기화 아키텍처 및 Context / GraphQL(Apollo Client) 상태 관리 (Ch 27, Ch 29–31)

### Entities
- [[React Native]] — Lean Core 정책, 코어 컴포넌트(`FlatList`, `Modal`, `Pressable`) 및 서드파티 생태계 (Ch 14–15, Ch 19–26)
- [[Expo]] — Expo CLI, Expo Go, Expo Snack을 통한 빠른 모바일 개발 워크플로 (Ch 16)
- [[React Navigation]] — `@react-navigation/native-stack` 기반 네이티브 스택·탭·드로어 내비게이션 (Ch 18)

### Syntheses
- [[React Native Learning Roadmap and Evolution (2019-2022)]] — React 18 및 React Native New Architecture 시대의 모던 개발 패러다임 종합
