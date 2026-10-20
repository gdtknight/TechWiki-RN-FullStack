---
type: concept
title: "TypeScript Core Syntax and Full-Stack Type Annotations"
category: "E. Programming Languages/JavaScript-TypeScript"
sources:
  - "[[2021-Mastering TypeScript Fourth Edition]]"
  - "[[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]]"
  - "[[2026-Full-Stack React TypeScript and Node 2nd Edition]]"
  - "[[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]]"
  - "[[2026-Clean Code with TypeScript]]"
tags:
  - typescript
  - syntax
  - type-annotations
  - interface
  - type-narrowing
  - utility-types
  - react-typescript
  - express-typescript
  - full-stack
created: 2026-10-19
updated: 2026-10-19
---

# TypeScript Core Syntax and Full-Stack Type Annotations

## 개요 (Overview)

[[JavaScript Core Syntax and Modern ES6+ Fundamentals|JavaScript 기초 문법]] 위에 **정적 타입 표기(`: Type`)** 를 결합한 [[TypeScript]]는 프론트엔드([[React]] / [[React Native]] 컴포넌트 Props, Hooks, 이벤트)와 백엔드(Node.js Express/Hono 요청·응답, Prisma DB 모델)를 하나의 타입 계약으로 묶어주는 풀스택 개발의 핵심 언어다.

이 문서는 [[2021-Mastering TypeScript Fourth Edition]] (Ch 1–4), [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]] (Ch 01–04), [[2026-Full-Stack React TypeScript and Node 2nd Edition]] (Ch 1–2, Ch 9–10) 및 **TypeScript 공식 핸드북(`typescriptlang.org/docs/handbook`)** 을 바탕으로, 낯선 개념 용어 풀이부터 기초 문법과 프론트엔드·백엔드 실전 타이핑 패턴까지 체계적으로 정리한다.

---

## 0. 타입스크립트 & 풀스택 필수 개념 용어 사전 (8선)

타입스크립트 책이나 강의를 볼 때 당연하다는 듯이 나오는 **8가지 핵심 용어**를 먼저 쉽게 풀어서 정리한다:

| 핵심 용어 | 직관적 비유 및 쉬운 정의 | 풀스택 실무에서 왜 중요한가? |
|---|---|---|
| **① 컴파일 타임(Compile Time) vs 런타임(Runtime)** | - **컴파일 타임**: 내가 VS Code에서 코드를 짜고 빌드하는 **"요리 준비 시간"**.<br>- **런타임**: 완성된 코드가 사용자의 브라우저/스마트폰이나 Node.js 서버에서 실제로 돌아가는 **"식당 영업 시간"**. | TypeScript의 모든 타입 검사는 **컴파일 타임(요리 준비 시간)** 에만 작동해서 오타와 버그를 미리 잡아준다. 영업이 시작된 **런타임**에 터지는 에러보다 준비 시간에 잡는 에러가 100배 고치기 쉽다! |
| **② 트랜스파일(Transpile)과 타입 소거(Type Erasure)** | 브라우저나 JS 엔진은 `: string`, `interface User` 같은 TypeScript 문법을 읽지 못한다. 그래서 빌드할 때 **타입 표기들을 지우개로 싹 지워버리고(타입 소거) 순수 JavaScript 코드로 번역(트랜스파일)** 한다. | 런타임(실제 실행 중)에는 `interface User`가 흔적도 없이 사라진다! 그래서 외부 API에서 날아온 JSON 데이터가 진짜 `User` 모양인지는 `if`문이나 **Zod** 같은 도구로 런타임에 한 번 더 확인해야 한다 ([[Type-Safe Contract-First and Runtime Boundary Architecture]]). |
| **③ 정적 타이핑(Static) vs 동적 타이핑(Dynamic)** | - **동적 타이핑(JS)**: 변수라는 상자에 숫자도 넣었다가 문자열도 넣었다가 실행 중에 마음대로 바꿀 수 있음.<br>- **정적 타이핑(TS)**: 상자에 `"숫자 전용(: number)"` 라벨을 붙여서 다른 타입을 넣으려고 하면 실행 전에 빨간 줄(에러)을 띄움. | 백엔드 개발자가 API 필드명을 `userName`에서 `nickname`으로 바꾸었을 때, 정적 타이핑이 되어 있으면 프론트엔드 코드 중 고쳐야 할 모든 곳에 즉시 빨간 줄이 떠서 수정 누락이 0건이 된다. |
| **④ 구조적 타이핑(Structural Typing / 오리 타이핑)** | **"오리처럼 걷고 오리처럼 울면 오리다"**: 이름표(`class`나 `interface` 이름)가 달라도, 객체 안에 들어 있는 **속성(프로퍼티) 모양만 똑같으면 같은 타입으로 통과**시켜 주는 방식. | Java/C# 같은 언어(명목적 타이핑)와 달리, TS에서는 굳이 `implements User`를 안 적어줘도 `{ id: "1", email: "a@b.com" }` 모양만 맞으면 `User` 타입 자리에 바로 넣을 수 있어 매우 유연하다. |
| **⑤ 타입 추론(Inference) vs 타입 단언(Assertion, `as`)** | - **타입 추론**: 내가 `: number`를 안 적어도 `const a = 10`을 보고 TS가 알아서 `"아, a는 숫자구나"` 하고 맞추는 것.<br>- **타입 단언(`as`)**: TS가 잘 모르는 값을 개발자가 `"이건 무조건 User 맞아! 믿고 넘어가!"` 하고 강제로 우기는 것. | 타입 추론은 적극 활용해야 하지만, **타입 단언(`as`)은 거짓말이 될 위험**이 커서 남발하면 런타임 크래시의 주범이 된다. |
| **⑥ 타입 좁히기(Type Narrowing)와 타입 가드(Type Guard)** | `"문자열일 수도 있고 null일 수도 있는 값(string \| null)"`을 `if (val !== null)` 조건문으로 걸러내어, 조건문 안에서는 **"100% 문자열(string)"로 범위를 좁혀주는 과정**. | 이때 범위를 좁히기 위해 사용하는 조건식(`typeof`, `instanceof`, `in`)을 **타입 가드(Type Guard)** 라고 부른다. |
| **⑦ 제네릭(Generics, `<T>`)** | **"타입을 빈칸(`<T>`)으로 비워둔 만능 틀(붕어빵 기계)"**: 함수를 만들 때는 어떤 타입이 들어올지 모르니 `<T>`로 비워두고, 나중에 함수를 **호출할 때 `<User>`(팥)나 `<Product>`(슈크림)를 넣으면 그 모양대로 결과 타입이 찍혀 나오는 문법**. | `useState<User>()`나 `Promise<string>`, `Array<number>`에서 꺾쇠괄호 `<...>`가 바로 제네릭이다! 코드 중복 없이 재사용성과 타입 안전성을 동시에 잡는 핵심 문법이다. |
| **⑧ DTO (Data Transfer Object)** | **"택배 상자 규격"**: 프론트엔드와 백엔드가 데이터를 주고받을 때 사용하는 **요청(Request) 및 응답(Response) 데이터 구조 타입** (예: `LoginRequestDto`, `UserResponseDto`). | DB 테이블 원본(`User`)에는 비밀번호 해시(`passwordHash`) 같은 민감 정보가 들어 있으므로, 이를 그대로 내려주지 않고 `Omit<User, "passwordHash">`로 안전한 **DTO**를 만들어 통신한다. |

---

## 1. 기본 타입 표기법(Type Annotations)과 타입 추론(Type Inference)

### 1.1 원시 타입, 배열, 튜플(Tuple), 리터럴 타입
변수나 매개변수 이름 뒤에 **콜론(`: 타입`)** 을 붙여 타입을 명시한다 ([[2021-Mastering TypeScript Fourth Edition]], Ch 1–2 p.26–65):

```ts
// 1. 원시 타입 (소문자 사용 필수! String, Number 대문자 래퍼 객체 사용 금지)
const userName: string = "Kim";
const age: number = 29;
const isActive: boolean = true;

// 2. 배열 타입 (T[] 표기법 권장, 불변 배열은 readonly T[])
const tags: string[] = ["react", "typescript"];
const readonlyIds: readonly number[] = [1, 2, 3]; // push(), pop() 호출 시 컴파일 에러!

// 3. 튜플 (Tuple): 길이와 각 인덱스의 타입이 고정된 배열 (React useState 반환값의 구조!)
const coordinate: [number, number] = [37.5665, 126.9780];
const stateTuple: [string, (newValue: string) => void] = ["initial", (v) => {}];

// 4. 유니온(|) & 리터럴(Literal) 타입: 허용할 값의 집합을 정확히 제한
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";
type IdType = string | number;
```

### 1.2 언제 타입을 직접 적고, 언제 생략(추론)하는가?
- **생략 권장 (타입 추론 활용)**: `const count = 0;`이나 `const items = ["a", "b"];`처럼 선언과 동시에 초기값이 주어지면 TypeScript가 자동으로 타입을 추론하므로 굳이 `: number`를 중복해서 적지 않는다 ([[2026-Clean Code with TypeScript]], p.52).
- **명시 필수 3대 상황**:
  1. **함수의 매개변수(Parameters)** 와 공개 API 함수의 **반환 타입(Return Type)**
  2. **빈 배열이나 `null`로 초기화되는 상태**: `const [user, setUser] = useState<User | null>(null);`, `const list: Product[] = [];`
  3. **객체 리터럴이 특정 규격(DTO/Props)을 준수해야 할 때**: `const payload: CreateUserRequest = { ... }` (또는 `satisfies CreateUserRequest`)

---

## 2. 객체 타이핑: `interface` vs `type` (타입 별칭)

객체 구조를 정의하는 두 키워드의 문법과 풀스택 실무 선택 기준이다 ([[2026-Full-Stack React TypeScript and Node 2nd Edition]], Ch 1 p.38–48):

```ts
// 1. interface 선언 및 extends 확장
interface BaseEntity {
  readonly id: string;      // readonly: 생성 후 수정 불가
  createdAt: Date;
}

interface User extends BaseEntity {
  email: string;
  nickname?: string;        // ?: 옵셔널 속성 (string | undefined)
}

// 2. type (Type Alias) 선언 및 & (교차 타입, Intersection) 확장
type ApiSuccess<T> = { status: "ok"; data: T };
type ApiError = { status: "error"; message: string };
type ApiResponse<T> = ApiSuccess<T> | ApiError; // 유니온(|)은 type만 가능!
```

| 비교 항목 | `interface` | `type` (타입 별칭) | 풀스택 실무 권장 기준 |
|---|---|---|---|
| **대상** | 객체(Object), 클래스, 함수 구조만 가능 | 객체뿐만 아니라 **원시값, 유니온(`\|`), 튜플, 매핑 타입** 모두 가능 | **유니온(`A \| B`)이나 유틸리티 조합은 무조건 `type` 사용** |
| **확장 문법** | `interface B extends A { ... }` (컴파일러 캐싱 성능 우수) | `type B = A & { ... }` (교차 타입) | 컴포넌트 Props나 도메인 모델 객체는 `interface` 또는 `type` 중 팀 컨벤션 통일 |
| **선언 병합 (Declaration Merging)** | **동일 이름으로 여러 번 선언 시 속성이 자동 합쳐짐** | 불가 (중복 선언 에러) | **Express의 `req.user` 확장이나 전역 `Window`/`ProcessEnv` 타입 확장 시 반드시 `interface` 사용!** |

```ts
// [풀스택 백엔드 실전] Express의 Request 인터페이스에 선언 병합으로 인증된 user 속성 추가
declare global {
  namespace Express {
    interface Request {
      user?: { id: string; role: "USER" | "ADMIN" };
    }
  }
}
```

---

## 3. 함수 타이핑 완벽 정리 (매개변수, 반환값, 콜백, `async`)

([[2021-Mastering TypeScript Fourth Edition]], Ch 2 p.72–88; [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]], Ch 02)

```ts
// 1. 기본 매개변수, 옵셔널 매개변수(?), 반환 타입(: number)
// 주의: 옵셔널 매개변수(discount?)는 필수 매개변수 뒤에 위치해야 함
function calculateTotal(price: number, quantity = 1, discount?: number): number {
  const raw = price * quantity;
  return discount ? raw - discount : raw;
}

// 2. 콜백 함수 타입 표기: (매개변수: 타입) => 반환타입
// 반환값을 사용하지 않는 이벤트 핸들러/콜백은 반환 타입을 void로 지정
type OnSubmitCallback = (formData: { email: string }) => void;

// 3. 비동기(async) 함수의 반환 타입은 항상 Promise<T>로 감싸야 함!
async function fetchUserProfile(userId: string): Promise<User> {
  const res = await fetch(`/api/users/${userId}`);
  if (!res.ok) throw new Error("Failed to fetch user");
  return (await res.json()) as User;
}
```

---

## 4. 타입 좁히기(Type Narrowing) 5대 기법과 `as` 남용 주의

유니온 타입(`string | null`, `ApiSuccess | ApiError`)이나 `unknown` 값을 안전하게 다루기 위해 조건문으로 타입을 좁히는 5가지 표준 기법이다 ([[2021-Mastering TypeScript Fourth Edition]], p.68–80; [[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]], p.45–52):

```ts
function processInput(val: string | string[] | null | Date) {
  // ① Truthiness 가드: null, undefined, "" 제외
  if (!val) return;

  // ② typeof 가드: 원시 타입("string", "number", "boolean", "function") 판별
  if (typeof val === "string") {
    return val.toUpperCase(); // 이 블록 안에서 val은 100% string
  }

  // ③ instanceof 가드: 클래스 인스턴스(Date, Error 등) 판별
  if (val instanceof Date) {
    return val.toISOString(); // 이 블록 안에서 val은 100% Date
  }

  // ④ Array.isArray 가드: 여기까지 왔다면 val은 자동으로 string[]로 좁혀짐
  return val.join(", ");
}

// ⑤ 'in' 연산자 & 공통 리터럴 판별자(Discriminated Union) 가드
function handleResponse(res: ApiResponse<User>) {
  if (res.status === "ok") {
    console.log(res.data.email); // ✅ res가 ApiSuccess<User>로 자동 정제됨
  } else {
    console.error(res.message);  // ✅ res가 ApiError로 자동 정제됨
  }
}
```

> [!CAUTION]
> **초보자가 가장 많이 범하는 실수: `as`(타입 단언)와 `!`(Non-null 단언) 남발**
> - `const user = data as User;`나 `user!.email`은 컴파일러에게 **"내가 책임질 테니 타입 에러를 숨겨라"** 고 명령할 뿐, 실제 런타임 데이터를 변환하거나 검증하지 않는다 ([[Type-Safe Contract-First and Runtime Boundary Architecture]] 참조).
> - 실제 서버 응답에 `email`이 누락되어 있으면 런타임에 즉시 앱이 뻗으므로, 외부 입력(`req.body`, `res.json()`, `JSON.parse`)은 반드시 타입 가드나 **Zod 스키마**로 검증해야 한다.

---

## 5. 풀스택 개발자를 위한 필수 내장 유틸리티 타입 8선

기존에 정의한 도메인 모델(`User`)을 재사용하여 프론트엔드 폼 상태나 백엔드 API 요청/응답 DTO를 파생시킬 때 사용하는 필수 도구다 ([[2026-Clean Code with TypeScript]], Ch 9 p.291–310):

```ts
interface User {
  id: string;
  email: string;
  name: string;
  passwordHash: string;
  role: "USER" | "ADMIN";
}

// 1. Omit<T, K>: 특정 민감 필드만 제거 (백엔드 응답에서 비밀번호 해시 제거 필수!)
type PublicUserDto = Omit<User, "passwordHash">;

// 2. Pick<T, K>: 필요한 속성만 추출 (로그인 요청 바디, 카드 요약 뷰)
type LoginRequestDto = Pick<User, "email"> & { password: string };

// 3. Partial<T>: 모든 속성을 선택적(?)으로 변환 (PATCH 일부 수정 API 바디)
type UpdateUserDto = Partial<Pick<User, "name" | "role">>;

// 4. Required<T> & Readonly<T>: 모든 속성을 필수로 만들거나 수정 불가능하게 동결
type StrictUserConfig = Readonly<Required<UpdateUserDto>>;

// 5. Record<K, V>: 키 타입 K와 값 타입 V를 가진 사전(Map) 객체 타입 정의
const ROLE_LABELS: Record<User["role"], string> = {
  USER: "일반 회원",
  ADMIN: "시스템 관리자",
};

// 6. ReturnType<typeof fn> & Awaited<T>: 함수의 반환 타입이나 Promise 내부 결과 타입 추출
type FetchProfileResult = Awaited<ReturnType<typeof fetchUserProfile>>; // User
```

---

## 6. 프론트엔드(React / RN) & 백엔드(Express / Prisma) 실전 치트시트

### 6.1 React & React Native 컴포넌트/훅 타이핑 패턴
([[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]], Ch 03–04; [[2024-React in Depth]], Ch 5–6)

```tsx
import { useState, useRef, type ReactNode, type ChangeEvent, type FormEvent } from "react";

// 1. 컴포넌트 Props 정의 (React 19 기준 ref도 일반 Prop으로 포함 가능)
interface ProfileCardProps {
  user: PublicUserDto;
  isHighlighted?: boolean;
  onSelect: (userId: string) => void;
  children?: ReactNode; // 텍스트, JSX 엘리먼트, 배열, null 등을 모두 수용하는 표준 자식 타입
}

export function ProfileCard({ user, isHighlighted = false, onSelect, children }: ProfileCardProps) {
  // 2. useState 타이핑: 초기값이 null이거나 빈 배열일 때 제네릭 <T> 명시
  const [selectedTab, setSelectedTab] = useState<"posts" | "comments">("posts");
  const [items, setItems] = useState<string[]>([]);

  // 3. DOM / Native View 참조를 위한 useRef 타이핑 (초기값 null 필수)
  const inputRef = useRef<HTMLInputElement>(null);

  // 4. 웹 React 폼/입력 이벤트 핸들러 타이핑
  const handleInputChange = (e: ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
  };
  const handleFormSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    inputRef.current?.focus(); // 옵셔널 체이닝으로 안전하게 DOM 메서드 호출
  };

  return (
    <div onClick={() => onSelect(user.id)}>
      <h3>{user.name}</h3>
      {children}
    </div>
  );
}
```

### 6.2 Node.js Express 라우트 핸들러 & Prisma ORM 타이핑 패턴
([[2026-Full-Stack React TypeScript and Node 2nd Edition]], Ch 9–10 p.305–385)

```ts
import type { Request, Response, NextFunction } from "express";
import { Prisma } from "@prisma/client";

// 1. Express Request 제네릭 4개 순서: Request<Params, ResBody, ReqBody, ReqQuery>
interface UserParams { id: string }
interface UpdateBody { name?: string }
interface ListQuery { page?: string }

export async function updateUserHandler(
  req: Request<UserParams, PublicUserDto | { error: string }, UpdateBody, ListQuery>,
  res: Response<PublicUserDto | { error: string }>,
  next: NextFunction
) {
  try {
    const userId = req.params.id;     // string으로 정확히 추론됨
    const newName = req.body.name;    // string | undefined로 정확히 추론됨
    // ... DB 업데이트 후 응답
  } catch (err) {
    next(err);
  }
}

// 2. Prisma 스키마로부터 관계(JOIN/include)가 포함된 정확한 반환 타입 자동 추출
type UserWithPosts = Prisma.UserGetPayload<{
  include: { posts: { select: { id: true; title: true } } };
}>;
```

---

## 관련 개념 및 엔티티

- **선행/기초 문법**: [[JavaScript Core Syntax and Modern ES6+ Fundamentals]]
- **심화 타입 아키텍처**: [[TypeScript Type System and Advanced Generics]], [[Type-Safe Contract-First and Runtime Boundary Architecture]], [[Full-Stack Authentication, Token Refresh, and Observability]]
- **구현 기술/엔티티**: [[TypeScript]], [[JavaScript (ECMAScript)]], [[React]], [[React Native]]
- **출처 문헌**: [[2021-Mastering TypeScript Fourth Edition]] (Ch 1–4), [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]] (Ch 01–04), [[2026-Full-Stack React TypeScript and Node 2nd Edition]] (Ch 1–2, Ch 9–10), [[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]] (Ch 1–2), [[2026-Clean Code with TypeScript]] (Ch 1, Ch 9), TypeScript 공식 문서 (`typescriptlang.org`)
