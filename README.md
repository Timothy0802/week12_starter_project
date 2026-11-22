# OSS Week 12 — React Hooks Playground (Student Starter)

## 실행 방법
Node.js 20.19+ 또는 22.12+ 권장. 프로젝트 폴더에서 실행:

```bash
npm install
npm run dev
```

브라우저에 터미널의 Local URL을 열고 LAB 탭을 이동하세요. 제출 전 `npm run build`도 실행하세요.

## 수행 방법
1. `src/components/` 아래의 LAB 1~4 파일에 있는 `TODO`를 직접 완성합니다.
2. `src/App.jsx`, `src/App.css`는 공통 화면이므로 가급적 수정하지 않습니다.
3. 각 LAB의 동작 화면과 콘솔 결과를 기록합니다.
4. 한 LAB에서 다른 LAB으로 탭을 전환하면 기존 컴포넌트가 언마운트됩니다. Cleanup 결과를 관찰합니다.


## 이해도 확인 질문
1. `setCount(prev => prev + 1)`은 왜 유용한가?
2. `useEffect(fn, [])`과 `useEffect(fn)`은 어떻게 다른가?
3. Cleanup은 언제 실행되는가? 개발 환경 StrictMode 영향은?
4. `useState`와 `useRef`의 값 변경 시 렌더링 차이는?
5. LAB 2와 LAB 4에서 Timer를 관리하는 방식의 차이는?

## Project Information: 프로젝트 소개 및 Starter Project 정보

## Installation: 설치·실행 방법 및 실행 화면
Node.js 20.19+ 또는 22.12+ 권장. 프로젝트 폴더에서 실행:

```bash
npm install
npm run dev
```

실행 화면
![Alt text](week12%EC%8B%A4%ED%96%89%ED%99%94%EB%A9%B4.png)

## Source Code Exploration: 주요 파일 구조, useEffect 및 useRef 분석, 주요 코드 2개 설명
App.jsx, Lab1~4     

useEffect를 통해서 정해진 이벤트가 발생하면(최초 마운트, 특정 값 변경 등) 함수를 실행하게 되며
useRef를 통해서는 값이 변경되어도 리렌더링하지 않는 ref를 만들 수 있다.
```jsx
    // TODO 1: 1초마다 setCount(prev => prev + 1)을 실행할 setInterval 작성
    // TODO 2: 만든 타이머 ID를 보관
    // TODO 3: return Cleanup 함수에서 clearInterval 호출
    // TODO 4: Dependency Array가 []일 때와 생략했을 때의 차이 관찰
useEffect(() => {
    const id = setInterval(() => {setCount(prev => prev + 1)}, 1000);
    return () => {return clearInterval(id);}
  }, []);
```
1초마다 setCount를 이용하여 그 전 값에 1씩을 더하고 언마운트 할 때는 clearInterval을 사용하여 타이머를 해제한다.

```jsx
  const timerRef = useRef(null);
  const start = () => {
    // TODO 2: timerRef.current에 타이머가 있으면 중복 시작 방지
    if(timerRef.current !== null) return;
    // TODO 3: setInterval의 반환 ID를 timerRef.current에 보관
    timerRef.current = setInterval(() => {
      setCount(prev => prev + 1);
    }, 1000);
    // TODO 4: 1초마다 count 증가, running을 true로 변경
    setRunning(true);
  };
```
timerRef라는 ref를 하나 만들어서 setInterval 할 때의 id를 저장하고 ref를 이용하여 타이머를 제어한다/

## Lab Implementation: 4개 Lab 구현 내용 및 실행 결과
1. 1초마다 카운트 수가 1씩 증가하는 타이머
2. Start, Stop, Reset 기능이 있는 타이머
3. ref를 이용하여 input 박스에 포커스를 적용하는 버튼 생성
4. ref를 이용한 타이머. 화면은 Lab2와 동일

## Lab Comparison: STEP 3의 비교 분석 질문 4개에 대한 답변
1. Project Structure: App.jsx와 주요 Lab 컴포넌트의 역할
import를 하는 부모 컴포넌트와 export를 하는 자식 컴포넌트
2. useEffect: Effect, Dependency Array 및 Cleanup의 동작 원리   
Dependency가 빈 배열이면 Cleanup이 언마운트 될 때만 한 번 실행되고 값이 있으면 그 값이 바뀔 때 cleanup을 먼저 하고 effect를 다시 실행한다. 아예 없으면 리렌더링 될 때마다 항상 cleanup이 실행된다.
4. useRef: .current 속성과 useState의 차이
ref의 값을 바꾸는 setter가 따로 없고 .current를 사용하여 값을 바꿀 수 있다. useState에서 setter로 값을 바꿀 때마다 리렌더링이 되지만 ref는 값을 바꿔도 리렌더링이 되지 않는다.
5. Code Analysis: useEffect와 useRef 관련 주요 코드 각 1개(5~15줄)를 선택하여 동작 원리 설명
``` jsx
useEffect(() => {
    const id = setInterval(() => {setCount(prev => prev + 1)}, 1000);
    return () => {return clearInterval(id);}
  }, []);
```
매번 리렌더링 될 때마다 타이머가 새로 추가되는 것을 막기 위하여 useEffect를 사용하여 타이머가 생성되는 때와 사라지는 때를 정확히 정해준다.

```jsx
const inputRef = useRef(null);
  const focusInput = () => {
    inputRef.current.focus();
  };

  const clearInput = () => {
    inputRef.current.value = "";
    inputRef.current.focus();
  };


  <input ... ref={inputRef}/>
```
처음에는 빈 ref를 만들고 후에 연결한 뒤 current를 이용하여 해당 html 요소에 기능을 수행할 수 있다.

### useEffect의 Dependency Array가 필요한 이유는?
Dependency Array가 없으면 setCount를 통한 매 리렌더링 때마다 새로운 effect가 생성된다.

### Cleanup을 사용하지 않으면 어떤 문제가 발생할 수 있는가?
타이머가 눈에는 보이지 않을지라도 계속해서 백그라운드에서 실행되고 있다.

### useState와 useRef의 가장 중요한 차이는?
useState는 값이 변경될 때마다 리렌더링 되지만 ref는 그렇지 않다.

### LAB 2와 LAB 4의 Timer 관리 방식은 어떻게 다른가?
LAB2에서는 타이머 id를 상수가 가지고 있는데 LAB4에서는 timerRef가 가지고 있다.

## Customizing: 수정 내용 및 파일명
1. App.jsx 프로젝트 제목 및 설명 수정
2. App.css 글자 및 border 색상 변경

## Deployment: 실제 접근 가능한 Vercel URL
https://week12-starter-project-kohl.vercel.app/

## Weekly Review:
새롭게 배운 내용 3가지
어려웠던 점과 해결 방법
AI 활용 내용 및 느낀 점