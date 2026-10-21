---
type: concept
title: "Mobile Layout and Styling with Flexbox"
category: "H. Information Systems/Mobile"
sources:
  - "[[2019-React Native in Action]]"
  - "[[2021-Do it! 리액트 네이티브 앱 프로그래밍]]"
  - "[[2022-React and React Native, Fourth Edition]]"
  - "[[2026-React Native Official Documentation]]"
tags:
  - react-native
  - flexbox
  - stylesheet
  - css-in-js
  - yoga
  - responsive-design
  - theming
created: 2026-10-18
updated: 2026-10-21
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
  - **RN 0.81 변경**: 코어 `<SafeAreaView>`는 사용 중단(deprecated)되었다. 이유는 "It is not compatible with edge-to-edge rendering on Android, and does not permit customization beyond padding."이며, 공식 권장은 `react-native-safe-area-context`다. Android 16은 edge-to-edge 표시를 강제한다 ([[2026-React Native Official Documentation]] 5절).
- **`Dimensions` / `useWindowDimensions` & `Platform.select`**: 기기의 논리적 가로/세로 크기 및 화면 회전(Orientation)을 감지하고, `Platform.OS` 또는 `Platform.select({ ios: ..., android: ... })`로 OS별 미세 스타일 차이를 분기한다 ([[2019-React Native in Action]], p.232, p.247; [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], p.116, p.158).
- **Context 기반 다크모드 테마**: `react-native-appearance` / `useColorScheme` 훅과 React Context API를 결합해 시스템 다크모드 설정에 따라 앱 전체 색상 팔레트를 동적으로 전환한다 ([[2019-React Native in Action]], Ch 5 p.139; [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], Ch 05-1 p.303).

---

## 4. 공식 문서(`reactnative.dev`) 기준 최신 보강: 웹 CSS 호환성 확장 (RN 0.71–0.88)

2019–2022년 도서 집필 이후, 과거 도서에서 "React Native에서 불가능하거나 플랫폼별 분기가 필요하다"고 설명했던 스타일 속성들이 하나씩 공식 지원으로 바뀌었다. 0.76 이후 추가된 속성은 대부분 **New Architecture 전용**이다. 0.82부터는 New Architecture로만 실행되므로 현재는 실질적 제약이 아니다 ([[2026-React Native Official Documentation]] 5절).

1. **Flexbox `gap` (RN 0.71+)**:
   - 0.71 릴리스 글: "0.71 adds initial support for gaps defined using pixel values." 현재 문서 기준으로 `gap`, `rowGap`, `columnGap`은 숫자와 퍼센트 문자열을 받으며, "Percentage values are supported on the New Architecture."
   - 자식 사이 여백을 `marginRight`/`marginBottom`으로 일일이 계산하던 구형 패턴 대신 부모 컨테이너에 `gap: 10`을 지정한다.
2. **통합 그림자 `boxShadow` (RN 0.76+)**:
   - **기존 도서의 한계**: iOS는 `shadowColor`, `shadowOffset`, `shadowOpacity`, `shadowRadius` 4개 속성을 조합하고, Android는 `elevation`만 작동하여 플랫폼별 그림자 모양이 달랐다 (`2019-RN in Action` p.135; `2021-Do it! RN` p.168).
   - **공식 문서**: "This is a spec-compliant implementation of the web style prop of the same name." 웹 문법 문자열이나 `BoxShadowValue` 객체 배열을 받고, 여러 그림자를 합성할 수 있다 ("a single `boxShadow` can be comprised of multiple different shadows").
   - **플랫폼 조건**: "Outset shadows are only supported on **Android 9+**. Inset shadows are only supported on **Android 10+**."
3. **그래픽 필터 `filter` (RN 0.76+)**: 지원 함수가 플랫폼마다 크게 다르다.
   - 공통: `brightness`, `opacity`. iOS는 이 둘만 지원한다 ("Due to issues with performance and spec compliance, these are the only two filter functions available on iOS.").
   - Android 전용: `blur`, `contrast`, `dropShadow`, `grayscale`, `hueRotate`, `invert`, `sepia`, `saturate`. 이 중 `blur`와 `dropShadow`는 Android 12 이상.
   - "`filter` implies `overflow: hidden`" 때문에 범위 밖으로 나간 자식은 잘린다.
   - 예: 크로스플랫폼으로 쓰려면 `filter: [{brightness: 0.8}, {opacity: 0.9}]`처럼 공통 함수만 쓴다.
4. **RN 0.77에 추가된 웹 CSS 속성** ("All these new features are available only for the New Architecture."):
   - **`display: 'contents'`**: 요소 자신은 레이아웃 박스를 만들지 않고 자식만 부모의 직계 자식처럼 배치한다. 이벤트를 처리해야 하는 래퍼 컴포넌트에 유용하다.
   - **`boxSizing`**: `border-box`와 `content-box`를 고른다. 기본값은 웹과 달리 `border-box`다 ("this is different from the default value on the web"). 하위 호환을 위해 그렇게 유지한다.
   - **`mixBlendMode`와 `isolation`**: 쌓임 맥락(stacking context) 안에서 색 혼합을 제어한다. 현재 문서 기준으로 Android 10 이상이 필요하다.
   - **outline 속성**: `outlineWidth`, `outlineStyle`, `outlineSpread`, `outlineColor`. border와 비슷하지만 border box 바깥에 그려지며 레이아웃에 영향을 주지 않는다.
5. **그라디언트 `backgroundImage`**:
   - 공식 문서: "`backgroundImage` provides the ability to draw a `linear-gradient()` and `radial-gradient()` using a web-like syntax." 예: `backgroundImage: 'linear-gradient(45deg, blue, red)'`.
   - 0.83 CHANGELOG에 네이티브 파서가 추가됐고("Background image native parser"), 0.87 CHANGELOG에서 정식 속성이 됐다("Remove `experimental_` prefix from `backgroundImage`"). `experimental_backgroundPosition`, `experimental_backgroundRepeat`, `experimental_backgroundSize`는 여전히 실험 단계다. 최초 도입 버전은 미확인이다 (확인 방법: `react/react-native` 저장소 `CHANGELOG.md`에서 `experimental_backgroundImage` 검색).
6. **가변 폰트 `fontVariationSettings` (RN 0.88+)**: "`fontVariationSettings` is now supported on both `<Text>` and `<TextInput>`, on Android and iOS."

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[React Native Architecture (Bridge to New Architecture)]], [[React Native Gesture and Animation System]], [[Web React vs React Native Shared Core and Differences]]
- **구현 기술/엔티티**: [[React Native]], [[Expo]]
- **출처 문헌**: [[2019-React Native in Action]] (Ch 4–5), [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 03, Ch 05), [[2022-React and React Native, Fourth Edition]] (Ch 15, Ch 17, Ch 28), React Native 공식 문서 (`reactnative.dev`)
