---
type: entity
title: "React Navigation"
entity_type: "framework"
category: "H. Information Systems/Mobile"
sources:
  - "[[2019-React Native in Action]]"
  - "[[2021-Do it! 리액트 네이티브 앱 프로그래밍]]"
  - "[[2022-React and React Native, Fourth Edition]]"
tags:
  - react-navigation
  - react-native
  - routing
  - stack-navigator
  - tab-navigator
  - drawer-navigator
created: 2026-10-18
updated: 2026-10-18
---

# React Navigation

## 개요

**React Navigation (`@react-navigation/native`)** 은 브라우저 URL 히스토리(`window.history`)가 존재하지 않는 [[React Native]] 모바일 환경에서 화면 전환(Routing), 제스처 기반 뒤로가기, 상단 헤더, 하단 탭 바, 사이드 드로어를 구현하는 **사실상의 표준(De-facto Standard) 내비게이션 라이브러리**다 ([[2019-React Native in Action]], Ch 6 p.169; [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], Ch 07 p.521; [[2022-React and React Native, Fourth Edition]], Ch 15 p.320, Ch 18 p.364).

---

## 핵심 아키텍처 및 의존성 구조

[[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 07-1, p.522–526)과 [[2022-React and React Native, Fourth Edition]] (Ch 18, p.365)에 따르면 모던 React Navigation(v5/v6+)은 코어 컨테이너와 네이티브 최적화 하위 패키지들의 조합으로 동작한다:

- **`NavigationContainer`**: 앱 최상단에서 전체 내비게이션 트리 상태(Navigation State)와 딥링크(Deep Linking)를 관리하는 루트 컨텍스트 프로바이더.
- **필수 피어 의존성(Peer Dependencies)**:
  - `react-native-screens`: 화면 컴포넌트를 JS 뷰가 아닌 OS 네이티브 화면 컨테이너(`UIViewController`, `Fragment`)로 매핑하여 메모리 사용량과 전환 성능을 최적화.
  - `react-native-safe-area-context`: 노치 및 시스템 상태바/홈바 안전 영역 자동 계산.
  - `react-native-gesture-handler`: 네이티브 스와이프 뒤로가기 및 드로어 끌기 제스처 처리.

---

## 3대 내비게이터(Navigators) 비교

3권의 도서 모두 모바일 앱의 화면 전환 구조를 **Stack, Tab, Drawer** 3가지 기본 패턴과 이들의 **중첩(Nesting)** 으로 설명한다 ([[2019-React Native in Action]], p.170; [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], p.550–626; [[2022-React and React Native, Fourth Edition]], p.365–384):

| 내비게이터 유형 | 패키지 및 생성 함수 | 작동 원리 및 주요 특징 | 적합한 UI 유스케이스 |
|---|---|---|---|
| **Stack Navigator** (스택 내비게이션) | `@react-navigation/stack` (`createStackNavigator`) 또는 **`@react-navigation/native-stack` (`createNativeStackNavigator`)** | LIFO 스택 구조로 새 화면을 푸시(`navigation.navigate` / `push`)하고 뒤로가기(`goBack` / `pop`) 수행. 특히 `native-stack`은 iOS `UINavigationController`와 Android `Fragment`를 직접 사용해 네이티브 전환 성능을 제공 (`2022-RN 4e` p.365). | 목록 화면 → 상세 화면 진입, 회원가입 단계별 이동, 모달 화면 |
| **Tab Navigator** (탭 내비게이션) | `@react-navigation/bottom-tabs` (`createBottomTabNavigator`) | 화면 하단(또는 상단) 탭 바를 통해 대등한 위계의 주요 화면들을 즉시 전환. 아이콘 및 배지(`tabBarBadge`) 표시 지원. | 홈 / 검색 / 알림 / 내 정보 등 앱의 최상위 메인 메뉴 구성 |
| **Drawer Navigator** (드로어 내비게이션) | `@react-navigation/drawer` (`createDrawerNavigator`) | 화면 좌/우측 가장자리에서 스와이프하거나 햄버거 메뉴 버튼을 눌러 서랍처럼 열리는 사이드 메뉴. | 카테고리 수가 많거나 계정 전환·환경설정 메뉴를 별도 분리할 때 |

### 라우트 파라미터와 헤더 제어 (`navigation` & `route` Props)
- **화면 이동 및 데이터 전달**: `navigation.navigate('Details', { itemId: 42, title: 'RN' })`으로 이동하며, 수신 화면에서는 `route.params`로 파라미터를 읽는다 ([[2022-React and React Native, Fourth Edition]], p.370–374).
- **동적 헤더 갱신**: `<Stack.Screen options={({ route, navigation }) => ({ title: route.params.title, headerRight: ... })} />` 또는 컴포넌트 내부의 `navigation.setOptions()`를 통해 상단 헤더 타이틀과 우측 액션 버튼을 동적으로 제어한다 ([[2022-React and React Native, Fourth Edition]], p.375–379).

---

## 관련 링크

- **연관 개념**: [[Mobile Layout and Styling with Flexbox]], [[React Native Gesture and Animation System]]
- **연관 엔티티**: [[React Native]], [[Expo]]
- **출처 문헌**: [[2019-React Native in Action]] (Ch 6 p.169–185), [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 07 p.521–627), [[2022-React and React Native, Fourth Edition]] (Ch 18 p.364–385)
