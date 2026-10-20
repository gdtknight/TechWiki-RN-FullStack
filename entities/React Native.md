---
type: entity
title: "React Native"
entity_type: "framework"
category: "H. Information Systems/Mobile"
sources:
  - "[[2019-React Native in Action]]"
  - "[[2021-Do it! 리액트 네이티브 앱 프로그래밍]]"
  - "[[2022-React and React Native, Fourth Edition]]"
  - "[[2026-React Native Official Documentation]]"
tags:
  - react-native
  - meta
  - cross-platform
  - mobile-framework
  - ios
  - android
created: 2026-10-18
updated: 2026-10-18
---

# React Native

## 개요

**React Native**는 Meta(구 Facebook)가 오픈소스로 공개한 **크로스플랫폼 모바일 애플리케이션 프레임워크**로, React의 선언적 컴포넌트 모델과 JavaScript/TypeScript를 사용해 iOS와 Android의 **실제 네이티브 UI 위젯**을 렌더링한다 ([[2019-React Native in Action]], p.27; [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], p.14; [[2022-React and React Native, Fourth Edition]], p.305).

웹뷰(WebView) 안에서 HTML/CSS를 흉내 내는 하이브리드 프레임워크와 달리, [[React Native Architecture (Bridge to New Architecture)|Bridge 또는 JSI(Fabric)]]를 통해 OS 순정 UI 컴포넌트를 직접 구동하므로 네이티브 수준의 UX와 반응성을 달성한다.

---

## 핵심 코어 컴포넌트와 네이티브 매핑

[[2019-React Native in Action]] (Ch 1 p.29–30), [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 02-3 p.73), [[2022-React and React Native, Fourth Edition]] (Ch 14 p.306, Ch 19–26)에 정리된 핵심 코어 컴포넌트 매핑:

| React Native 컴포넌트 | 웹(HTML) 대응 개념 | iOS 네이티브 뷰 | Android 네이티브 뷰 | 주요 역할 및 특징 |
|---|---|---|---|---|
| `<View>` | `<div>` (비스크롤 컨테이너) | `UIView` | `android.view.ViewGroup` | [[Mobile Layout and Styling with Flexbox\|Flexbox]] 레이아웃, 터치 핸들링, 접근성 컨테이너 |
| `<Text>` | `<p>`, `<span>` | `UITextView` | `android.widget.TextView` | 문자열 표시 전용 (RN에서 모든 텍스트는 반드시 `<Text>` 안에 위치해야 함) |
| `<Image>` | `<img>` | `UIImageView` | `android.widget.ImageView` | 로컬 정적 이미지(`require`) 및 원격 네트워크 이미지(`uri`), 리사이징/지연 로딩 |
| `<TextInput>` | `<input type="text">` | `UITextField` | `android.widget.EditText` | 가상 키보드 연동 텍스트 입력 (`onChangeText`, `secureTextEntry` 등) |
| `<ScrollView>` | `overflow: scroll` `<div>` | `UIScrollView` | `HorizontalScrollView` / `ScrollView` | 모든 자식 요소를 한 번에 메모리에 렌더링하는 스크롤 컨테이너 (소량 콘텐츠용) |
| `<FlatList>` / `<SectionList>` | 가상화 리스트 (Virtualized List) | `UITableView` / `UICollectionView` | `RecyclerView` | 화면에 보이는 영역만 지연 렌더링(Lazy Loading)하고 풀투리프레시(`onRefresh`), 무한 스크롤(`onEndReached`) 지원 (`2022-RN 4e` Ch 19) |
| `<Modal>` | `<dialog>` | `UIViewController` (Modal) | `android.app.Dialog` | 현재 화면 위에 오버레이로 표시되는 확인 창·바텀시트·로딩 화면 (`2022-RN 4e` Ch 23) |

---

## 대안 모바일 개발 방식과의 비교

[[2019-React Native in Action]] (p.27–29)과 [[2022-React and React Native, Fourth Edition]] (Ch 14 p.305–309)의 논의를 종합한 모바일 앱 개발 접근법 비교:

| 항목 | 순수 네이티브 (Swift / Kotlin) | **[[React Native]]** | WebView 하이브리드 / PWA (Cordova, Ionic) |
|---|---|---|---|
| **렌더링 대상** | OS 네이티브 UI 위젯 | **OS 네이티브 UI 위젯 (`UIView` / `android.view`)** | 모바일 브라우저 엔진 내 HTML DOM |
| **코드 공유율** | 0% (iOS/Android 별도 개발 팀 필요) | **85–95%+** (단일 JS/TS 코드베이스 + 웹 React와 로직 공유 가능) | 약 100% |
| **UI 성능 및 제스처** | 최상 | **상** (특히 [[React Native Architecture (Bridge to New Architecture)\|New Architecture]] + Reanimated 적용 시 네이티브에 준함) | 중–하 (복잡한 스크롤·제스처에서 이질감 발생) |
| **개발 생산성** | 재컴파일 대기 시간 존재 | **Fast Refresh (Hot Reloading)** 및 [[Expo]] 생태계로 즉각 피드백 | 웹 개발과 동일 |

---

## 관련 링크

- **연관 개념**: [[React Native Architecture (Bridge to New Architecture)]], [[Mobile Layout and Styling with Flexbox]], [[React Native Gesture and Animation System]], [[Offline-First and Mobile State Management]]
- **연관 엔티티**: [[Expo]], [[React Navigation]]
- **종합 로드맵**: [[React Native Learning Roadmap and Evolution (2019-2022)]]
- **출처 문헌**: [[2019-React Native in Action]], [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], [[2022-React and React Native, Fourth Edition]], [[2026-React Native Official Documentation]]
