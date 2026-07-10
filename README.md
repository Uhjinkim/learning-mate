# Learning Mate

AI와 함께 개발하면서 웹 개발을 배우기 위한 교육용 프로젝트입니다.

수업에서 배운 내용을 간단한 폼으로 기록하고, 다시 복습하며, Notion에 정리할 수 있도록 Markdown으로 내보내는 학습 기록 웹앱을 만듭니다.

## 프로젝트 목적

**AI를 활용해 개발하되, 결과물이 아니라 과정을 이해하는 것을 목표로 한다.**

## 프로젝트 원칙

1. AI는 적극 활용한다.
2. 하지만 붙여넣기 전에 이해한다.
3. 최소 한 가지는 설명할 수 있어야 한다.
4. 배운 개념을 프로젝트에 바로 적용한다.
5. 완벽한 서비스보다 완성된 프로젝트를 목표로 한다.

---

## 1. 프로젝트 목표

### 만드는 과정에서 학습
- HTML 폼 작성
- CSS로 화면 꾸미기
- JavaScript 이벤트 처리
- 객체와 배열 사용
- localStorage 저장
- GitHub 협업
- AI에게 기능 단위로 코드 요청하기
- AI가 만든 코드의 동작 설명하기

### 완성 후에도 학습
- 수업 내용을 매일 기록
- 이전 기록 검색 및 복습
- 어려웠던 내용 다시 확인
- Notion용 Markdown 생성 및 옮겨서 장기 정리

---

## 2. 핵심 사용자 흐름

```
새 학습 기록 작성
      ↓
저장 버튼 클릭
      ↓
브라우저에 기록 저장
      ↓
기록 목록에서 다시 확인
      ↓
필요한 기록 선택
      ↓
Markdown 미리보기
      ↓
복사하여 Notion에 붙여넣기
```

---

## 3. 첫 달 MVP

### 데이터 구조 (필드 확정)

| 필드 | 설명 | 비고 |
|---|---|---|
| id | 고유 식별자 | `Date.now()` |
| title | 제목 | 필수 |
| date | 학습 날짜 | **자동 생성** (입력받지 않음) |
| category | 카테고리 | select |
| learned | 오늘 배운 내용 | textarea |
| difficult | 어려웠던 점 | textarea |
| code | 예제 코드 | textarea |
| review | 다음에 복습할 내용 | textarea |

> 태그(tags)는 카테고리와 개념이 겹치므로 **1차 개선으로 이동**.

```js
const studyRecord = {
  id: Date.now(),
  title: "Python 조건문",
  date: new Date().toISOString().slice(0, 10), // 자동 생성
  category: "Python",
  learned: "if, elif, else를 배웠다.",
  difficult: "조건 순서를 정하기 어려웠다.",
  code: "if age >= 19:",
  review: "논리 연산자 복습"
};
```

### MVP 기능 5가지

**① 학습 기록 작성**
- `<form>`, `<input>`, `<textarea>`, `<select>`, `<button>` 활용
- 필수 입력은 `required` 속성으로 간단히 처리

**② 기록 저장**
- 폼 값 → 객체 생성 → 배열에 추가 → `localStorage`에 저장

**③ 기록 목록**
- 저장된 기록을 카드 형태로 표시
- 카드에 표시할 항목: 제목 / 날짜 / 카테고리 / 짧은 내용 / [보기] [삭제]
- (검색·카테고리 필터는 MVP에서 제외 → 1차 개선)

**④ 기록 상세 보기**
- 전체 내용 보기 / 삭제 / Markdown 만들기 / 클립보드 복사
- 수정 기능은 시간 부족 시 1차 개선으로 이동

**⑤ Notion용 Markdown 내보내기**

```
# Python 조건문

- 학습 날짜: 2026-07-11
- 카테고리: Python

## 오늘 배운 내용
if, elif, else를 배웠다.

## 어려웠던 점
조건 순서를 정하기 어려웠다.

## 예제 코드
​```python
if age >= 19:
    print("성인입니다.")
​```

## 다음에 복습할 내용
논리 연산자 복습
```

버튼: `[Markdown 미리보기]` `[복사하기]`

> **주의**: `navigator.clipboard.writeText()`는 `file://`로 직접 열면 동작하지 않을 수 있습니다. VSCode Live Server 등으로 로컬 서버를 띄우거나, GitHub Pages 배포 후 테스트하세요.

---

## 4. 화면 구성

2개 화면(HTML 파일)으로 구성. SPA 대신 `index.html` / `write.html`로 분리해 초보 팀이 이해하기 쉽게 합니다.

### 화면 1: 기록 목록 (index.html)
```
┌─────────────────────────────────┐
│ Learning Mate        [새 기록]  │
├─────────────────────────────────┤
│ Python 조건문                   │
│ 2026-07-11 · Python             │
│ if, elif, else를 배웠다...      │
│              [보기] [삭제]      │
├─────────────────────────────────┤
│ Git 브랜치                      │
│ 2026-07-12 · Git                │
└─────────────────────────────────┘
```

### 화면 2: 작성·상세 화면 (write.html)
```
┌─────────────────────────────────┐
│ 학습 기록 작성                  │
├─────────────────────────────────┤
│ 제목                            │
│ [____________________________]  │
│                                 │
│ 카테고리                        │
│ [Python ▼]                      │
│                                 │
│ 오늘 배운 내용                  │
│ [                             ] │
│                                 │
│ 어려웠던 점                     │
│ [                             ] │
│                                 │
│ 예제 코드                       │
│ [                             ] │
│                                 │
│ 다음에 복습할 내용              │
│ [                             ] │
│                                 │
│          [취소] [저장]          │
└─────────────────────────────────┘
```

---

## 5. 파일 구조

```
learning-mate/
├── index.html
├── write.html
├── css/
│   └── style.css
├── js/
│   ├── storage.js
│   ├── record-list.js
│   ├── record-form.js
│   └── markdown.js
├── README.md
└── assets/
    └── icons/
```

| 파일 | 역할 |
|---|---|
| index.html | 기록 목록 |
| write.html | 학습 기록 폼, Markdown 미리보기 |
| storage.js | localStorage 저장 / 불러오기 / 삭제 |
| record-list.js | 목록 출력 |
| record-form.js | 폼 입력값 읽기, 객체 생성 |
| markdown.js | 객체 → Markdown 변환, 클립보드 복사 |

---

## 6. 데이터 흐름

```
HTML Form
   ↓
submit 이벤트
   ↓
입력값 읽기
   ↓
학습 기록 객체 생성
   ↓
기존 배열에 추가
   ↓
JSON 문자열로 변환
   ↓
localStorage 저장
   ↓
목록 화면에서 다시 불러오기
   ↓
HTML 카드 생성
```

**저장 과정**
```js
const records = [studyRecord];
localStorage.setItem("studyRecords", JSON.stringify(records));
```

**불러오는 과정**
```js
const records = JSON.parse(localStorage.getItem("studyRecords")) || [];
```

**Markdown 생성 과정**
```
저장된 학습 기록 객체
       ↓
제목 앞에 #
       ↓
항목 이름 앞에 ##
       ↓
예제 코드 앞뒤에 ```
       ↓
Markdown 문자열 완성
```

---

## 7. 팀 역할 분배 (4인)

**조장: 프로젝트 구조·통합 + Markdown 내보내기**
- 주제와 MVP 확정, 기본 파일 구조 생성
- GitHub 저장소 관리, 팀원 코드 통합, 기능 간 연결
- Markdown 생성 로직, 클립보드 복사 기능
- 작업 안내서 작성
- 배울/적용할 개념: 템플릿 리터럴, 문자열 처리, 함수, Clipboard API, Git 브랜치 전략

**팀원 A: 학습 기록 폼**
- 입력 화면, 필수 입력 검사(`required`), 폼 데이터 읽기
- 배울 개념: form, input, textarea, select, label, submit

**팀원 B: 저장 기능**
- 저장(setItem) / 불러오기(getItem, JSON.parse) / 삭제
- 배울 개념: 객체, 배열, JSON, localStorage

**팀원 C: 기록 목록 · 디자인 · 테스트**
- 목록 출력(카드 형태), 빈 목록 화면, 기본 CSS
- 기능 테스트 체크리스트, README 초안 작성
- 배울 개념: DOM, createElement, forEach, CSS 카드/반응형

### 작업 의존 관계
```
A(폼) → B(저장) → C(목록) → 나(Markdown 통합)
```
B가 저장 객체 구조를 먼저 확정해야 C와 통합 담당이 그 구조에 맞춰 작업할 수 있습니다. 따라서 **데이터 객체 구조(필드명)를 팀 전체가 먼저 합의**하고 시작합니다.

### Git 협업 방식
- 기능별 브랜치 생성 (예: `feature/form`, `feature/storage`, `feature/list`, `feature/markdown`)
- 작업 완료 후 PR 생성 → 통합 담당 리뷰 → merge
- merge conflict 발생 시 통합 담당에게 먼저 공유

---

## 8. 개발 순서

### 1단계: 사전 준비 (통합 담당)
- 프로젝트 한 줄 소개
- 입력 항목 확정 (완료 — 위 데이터 구조 참고)
- 간단한 화면 스케치 (완료)
- GitHub 저장소 생성
- 기본 폴더와 빈 파일 생성
- 샘플 데이터 한 개 준비
- 팀원용 기능 설명 문서 작성

### 2단계: HTML만으로 화면 완성
- 폼이 화면에 보인다
- 모든 입력칸에 label이 있다
- 저장 버튼을 누를 수 있다
- 기록 카드 샘플이 보인다

### 3단계: JavaScript로 입력 확인
```js
console.log(title);
console.log(learned);
console.log(studyRecord);
```

### 4단계: localStorage 연결
저장 → 새로고침 → 다시 불러오기 (가장 중요한 학습 포인트)

### 5단계: 목록 출력
```js
records.forEach(record => {
  // 기록 카드 생성
});
```

### 6단계: Markdown 출력
저장된 데이터를 정해진 템플릿으로 변환

### 7단계: 테스트와 배포
- 빈 값 입력 / 긴 문장 입력 / 여러 기록 저장 / 삭제 / 새로고침
- Markdown 복사 테스트 (로컬 서버 환경에서)
- GitHub Pages 배포

---

## 9. 이번 달에는 제외할 기능

- 로그인
- 서버 / 데이터베이스
- 팀원 간 실시간 공유
- Notion API 직접 연동
- AI API 연결
- PDF 생성
- 복잡한 리치 텍스트 편집기
- 퀴즈
- 일정 관리
- 통계 차트
- 태그, 검색, 카테고리 필터 (→ 1차 개선으로 이동)

---

## 10. 추후 개선 순서

**1차 개선**: 수정 기능, 검색, 카테고리 필터, 태그, 다크 모드, Markdown 파일 다운로드
**2차 개선**: CSV 내보내기, 여러 기록 일괄 내보내기, 학습 통계, 퀴즈 생성
**3차 개선**: Supabase/Django 저장, 사용자 로그인, 팀 기록 공유
**4차 개선**: Notion API 연동, 팀 공용 노션 전송, AI 요약 및 복습 문제 생성

---

## 11. AI 사용 기준

팀원은 AI가 작성한 모든 문법을 외울 필요는 없습니다. 대신 자신이 맡은 기능에 대해 다음 질문에는 답할 수 있어야 합니다.

- 어떤 HTML 요소에서 입력을 받는가?
- 버튼을 누르면 어떤 함수가 실행되는가?
- 입력값을 어떤 객체로 만드는가?
- 데이터는 어디에 저장되는가?
- 새로고침해도 남는 이유는 무엇인가?
- 목록은 어떤 배열을 이용해 출력하는가?
- Markdown은 어떤 규칙으로 만들어지는가?

### 최종 성공 기준

> 모든 코드를 직접 작성하지는 않았더라도, 사용자가 입력한 내용이 어떻게 저장되고 다시 표시되며 Notion용 문서로 만들어지는지 팀원들이 설명할 수 있다.
