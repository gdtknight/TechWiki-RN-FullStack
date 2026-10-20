---
type: concept
title: "Mobile Layout and Styling with Flexbox"
category: "H. Information Systems/Mobile"
sources:
  - "[[2019-React Native in Action]]"
  - "[[2021-Do it! 리액트 네이티브 앱 프로그래밍]]"
  - "[[2022-React and React Native, Fourth Edition]]"
tags:
  - react-native
  - flexbox
  - stylesheet
  - css-in-js
  - yoga
  - responsive-design
  - theming
created: 2026-10-18
updated: 2026-10-18
---

# Mobile Layout and Styling with Flexbox

## 정의 (Definition)

[[React Native]]는 웹 브라우저의 CSS 파일 대신 **JavaScript 객체 기반의 CSS-in-JS 스타일링(`StyleSheet` API)** 과 **Flexbox 레이아웃 모델**을 사용하여 다양한 화면 크기의 iOS 및 Android 기기에서 일관된 반응형 UI를 구성한다 ([[2019-React Native in Action]], p.99, p.152; [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], p.104–118; [[2022-React and React Native, Fourth Edition]], p.317, p.343).

작성된 스타일 객체는 [[React Native Architecture (Bridge to New Architecture)|Shadow Thread의 Yoga 엔진]]을 통해 각 OS의 네이티브 레이아웃 제약 조건으로 변환된다 ([[2022-React and React Native, Fourth Edition]], p.317).

---

## 1. 웹 CSS와 React Native 스타일링의 핵심 차이

3권의 도서가 공통으로 강조하는 웹 CSS 대비 React Native 스타일링의 차이점은 다음과 같다:

| 구분 | 웹 CSS (Browser DOM) | React Native (`StyleSheet` & Yoga) | 출처 |
|---|---|---|---|
| **문법 표기법** | 케밥 케이스 (`background-color`) | 카멜 케이스 (`backgroundColor`) JavaScript 객체 | `2019-RN in Action` p.100 |
| **기본 `flexDirection`** | `row` (좌→우 가로 배치) | **`column` (위→아래 세로 배치)** — 모바일 세로 화면에 최적화 | `2021-Do it! RN` p.152; `2022-RN 4e` p.343 |
| **크기 단위** | `px`, `em`, `rem`, `%`, `vw/vh` | **단위 없는 숫자(논리적 픽셀/밀도 독립 픽셀 `dp`/`pt`)** 및 문자열 `'50%'`만 허용 | `2019-RN in Action` p.111; `2021-Do it! RN` p.116 |
| **스타일 상속 (Inheritance)** | 부모 요소의 폰트/색상이 모든 자식에 자동 상속됨 | **원칙적으로 스타일 상속 없음** (단, `<Text>` 내부의 중첩 `<Text>` 간에만 텍스트 스타일 상속 허용) | `2019-RN in Action` p.126 |
| **그림자 (Shadow) 처리** | `box-shadow` 하나로 통합 | iOS는 `shadowColor`/`shadowOffset`/`shadowOpacity`/`shadowRadius`, Android는 **`elevation`** 속성 사용 | `2019-RN in Action` p.135; `2021-Do it! RN` p.168 |

---

## 2. Flexbox 레이아웃 핵심 축과 속성

[[2019-React Native in Action]] (Ch 5, p.152–165), [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 03-4, p.149–169), [[2022-React and React Native, Fourth Edition]] (Ch 17, p.343–361)에서 정리하는 Flexbox 제어 모델:

- **`flex` 속성**: 웹의 `flex-grow`처럼 부모 컨테이너의 남은 공간을 자식들이 어떤 비율로 나눠 가질지 결정한다 (`flex: 1`이면 가용 공간 전체 차지).
- **주축(Main Axis) 결정 (`flexDirection`)**:
  - `column` (기본값): 주축이 수직(위→아래), 교차축(Cross Axis)이 수평(좌→우).
  - `row`: 주축이 수평(좌→우), 교차축이 수직(위→아래).
- **주축 정렬 (`justifyContent`)**: `flex-start`, `center`, `flex-end`, `space-between` (양 끝 밀착 후 균등 배분), `space-around`, `space-evenly`.
- **교차축 정렬 (`alignItems` & `alignSelf`)**: `stretch` (기본값, 교차축 방향으로 늘어남), `flex-start`, `center`, `flex-end`, `baseline`. 특정 자식만 예외 정렬할 때 `alignSelf` 사용.

---

## 3. 스타일 조직화 및 테마(다크모드) 설계 패턴

### 3.1 `StyleSheet.create` vs `styled-components` vs UI Kit
- **`StyleSheet.create` (기본 내장)**: 인라인 객체 전달 시 렌더링마다 새 객체가 생성되는 오버헤드를 막고, IDE 자동완성 및 정적 타입 검증을 제공한다 ([[2021-Do it! 리액트 네이티브 앱 프로그래밍]], p.108). 배열 구문(`style={[styles.base, styles.active]}`)을 통해 조건부 스타일을 합성한다.
- **`styled-components/native`**: 웹 React 개발자에게 친숙한 CSS 문법으로 스타일이 내장된 컴포넌트를 선언할 수 있다 ([[2022-React and React Native, Fourth Edition]], Ch 17 p.347).
- **유니버설 UI 컴포넌트 라이브러리**: 원자 컴포넌트를 직접 스타일링하는 대신 **NativeBase**(Ch 28, p.528), **React Native Paper**(Material Design), **UI Kitten**(Eva Design System), **react-native-elements** 등을 도입해 일관된 디자인 시스템을 빠르게 구축할 수 있다 ([[2022-React and React Native, Fourth Edition]], p.320, p.528).

### 3.2 플랫폼·화면 크기 대응 및 다크모드
- **`SafeAreaView` & `react-native-safe-area-context`**: 아이폰 노치(Notch)나 홈 인디케이터 영역을 침범하지 않도록 안전 영역 내부에 콘텐츠를 배치한다 ([[2021-Do it! 리액트 네이티브 앱 프로그래밍]], p.114).
- **`Dimensions` / `useWindowDimensions` & `Platform.select`**: 기기의 논리적 가로/세로 크기 및 화면 회전(Orientation)을 감지하고, `Platform.OS` 또는 `Platform.select({ ios: ..., android: ... })`로 OS별 미세 스타일 차이를 분기한다 ([[2019-React Native in Action]], p.232, p.247; [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], p.116, p.158).
- **Context 기반 다크모드 테마**: `react-native-appearance` / `useColorScheme` 훅과 React Context API를 결합해 시스템 다크모드 설정에 따라 앱 전체 색상 팔레트를 동적으로 전환한다 ([[2019-React Native in Action]], Ch 5 p.139; [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], Ch 05-1 p.303).

---

## 4. 공식 문서(`reactnative.dev`) 기준 최신 보강: 웹 CSS 호환성 대폭 확장 (RN 0.71–0.77+)

2019–2022년 도서 집필 이후 **Yoga 3.0 엔진과 New Architecture(RN 0.76–0.77+)** 가 도입되면서, 과거 도서에서 "React Native에서 불가능하거나 플랫폼별 분기가 필요하다"고 설명했던 핵심 스타일 속성들이 공식 지원으로 전환되었다:

1. **통합 그림자 속성 (`boxShadow`, RN 0.76+)**:
   - **기존 도서의 한계**: iOS는 `shadowColor`, `shadowOffset`, `shadowOpacity`, `shadowRadius` 4개 속성을 조합하고, Android는 `elevation`만 작동하여 플랫폼별 그림자 모양이 달랐다 (`2019-RN in Action` p.135; `2021-Do it! RN` p.168).
   - **공식 문서 최신 표준**: New Architecture 환경에서 웹 CSS와 동일한 단일 **`boxShadow: '0px 4px 8px rgba(0, 0, 0, 0.2)'`**(내부 그림자 `inset` 및 다중 그림자 포함)를 iOS와 Android 양쪽에서 동일하게 사용할 수 있다.
2. **그래픽 필터 효과 (`filter` & `mixBlendMode`, RN 0.76–0.77+)**:
   - 별도 네이티브 이미지 처리 라이브러리 없이 뷰 자체에 **`filter: 'blur(4px) brightness(0.9) saturate(1.2)'`** 및 `mixBlendMode`를 선언적으로 적용할 수 있다.
3. **Flexbox `gap`, `rowGap`, `columnGap` 및 `display: 'contents'` (RN 0.71–0.77+)**:
   - 자식 요소 사이에 여백을 주기 위해 일일이 `marginRight`/`marginBottom`을 계산하던 구형 패턴 대신, 웹과 동일하게 부모 컨테이너에 **`gap: 12`** 를 지정하는 것이 현대 표준이다.
   - 또한 RN 0.77부터 `boxSizing`(`border-box` vs `content-box`), `outline`, `display: 'contents'`가 추가되어 웹([[React]])과 [[React Native]] 간 스타일 코드 공유율이 한층 높아졌다.

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[React Native Architecture (Bridge to New Architecture)]], [[React Native Gesture and Animation System]], [[Web React vs React Native Shared Core and Differences]]
- **구현 기술/엔티티**: [[React Native]], [[Expo]]
- **출처 문헌**: [[2019-React Native in Action]] (Ch 4–5), [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 03, Ch 05), [[2022-React and React Native, Fourth Edition]] (Ch 15, Ch 17, Ch 28), React Native 공식 문서 (`reactnative.dev`)
