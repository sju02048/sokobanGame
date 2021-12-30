# 2022 마스터즈 코스 테스트


## 목차

- [😶 1단계: 지도 데이터 출력하기](#-1단계-지도-데이터-출력하기)
  
- [🙂 2단계: 플레이어 이동 구현하기](#-2단계-플레이어-이동-구현하기)
  
- [😎 3단계: 소코반 게임 완성하기](#-3단계-소코반-게임-완성하기)
 
- [🥰 4단계: 추가기능 구현](#-4단계-추가기능-구현)
  
- [🙄 생각 정리](#-생각-정리)


<br/><br/>


# 😶 1단계: 지도 데이터 출력하기

## STEP-1 : 문자열로 넘겨서 처리하는 함수를 작성한다.

### ⚽ 코드동작

`Level-1.js`

1. 입력값 받기
   - `readMap` 함수를 통해 `map.txt`의 정보를 담는다.

2. `readFileSync` 이용. 동기적으로 값 리턴하기.
   - 처음에는 비동기 함수인 `readFile`을 사용했는데 `readMap`함수를 실행했을 때 이벤트루프로 인해 `readFile`의 콜백함수가 제일 늦게 실행되어 return값을 제대로 받지 못했었다. 

<br/>

### ⚾ 실행결과

![image](https://user-images.githubusercontent.com/72546335/144833620-dcdb8ef3-44b1-49e5-b629-ee851d95e93f.png)


<br/><br/>

## STEP-2 : 2차원 배열로 변환 저장한다.

### ⚽ 코드동작

1. 기호를 저장값으로 바꾸기
   - `replace`, 정규표현식으로 숫자값으로 바꾸어 준다.
   
2. 2차원 배열로 변환 후 객체에 저장하기
   - 경계선을 split 해 Stage 마다 나누고 빈 배열 값(\r\n) 제거한다.
   - split, map 함수를 사용 후 요소를 한번 더 split해 `2차원 배열`로 만든다.
   - 배열 안에 빈 값을 filter로 없애준다.
   - `setArrayInObject` 함수를 통해 배열 첫번째를 키, 나머지를 밸류인 객체로 푸시한다.
   - 객체로 만든 이유는 Stage 별로 관리하기 쉽게 하기 위함!


<br/>

### ⚾ 실행결과

![image](https://user-images.githubusercontent.com/72546335/144833979-e6d81d2c-fee3-40d3-a7d8-fe4f7de60675.png)


<br/><br/>

## STEP-3 : 각 스테이지 정보를 출력한다.

### ⚽ 코드동작


1. 스테이지 정보 출력하기
   - `printStageInfo` 함수에 있는 `printStr` 변수에 출력정보를 모두 담아 한번에 출력할 예정
   - for in으로 객체의 키 값과 밸류를 printStr에 담기
   - 스테이지 세부정보 (가로크기, 세로크기, 구멍의 수, 공의 수, 플레이어 위치)를 따로 담을 `printStageDetailInfo` 함수 생성
   - 가로크기는 배열의 길이를 모두 담아 최대값 `Math.max`로 구함.
   - 구멍의 수와 공의 수는 정규표현식 `match`를 이용 원하는 값을 담아 `flat`하게 만든다음 `filter`로 맞는 개수를 찾아 길이를 구함.
   - 플레이어는 1개만 구하면 되기때문에 위치를 구하기 위해 findIndex와 indexOf로 2차원 배열의 x축과 y축을 얻음.


<br/>

### ⚾ 실행결과

![image](https://user-images.githubusercontent.com/72546335/145118762-aba1bb22-7cf0-4d73-816b-ec1d6fed05ee.png)


<br/><br/>


# 🙂 2단계: 플레이어 이동 구현하기

## STEP : 스테이지2의 지도를 읽고 사용자 입력을 받아 캐릭터를 움직이는 프로그램을 작성

### ⚽ 코드동작

`Level-2.js`

1. 스테이지를 2차원 배열로 바꾸기
   - 1 단계에서 이용했던 `readMap`함수로 `map.txt` 파일의 값을 불러온다.
   - map함수와 split으로 불러온 값를 2차원 배열로 바꾸어준다.

2. 사용자 입력값 받기
   - node.js의 `readline`을 불러와 한 줄씩 입력값을 받는다.
   - 값에 따라 정규표현식 조건을 걸고 통과시 `moveController` 함수의 인자로 넣고 실행.
   
3. 커맨드 동작에 따른 함수 실행
   - 처음 생성된 지도 2차원배열 `currArr` 전역으로 정의
   - `moveController`: W,A,S,D 중 한 가지를 받음 (소문자일때 변환)
   - 2차원 배열을 [y][x]로 접근할 수 있는 것을 이용해 `movePos` 객체에 이동거리 값 정의
   - switch문을 통해 장애물 유무에 따른 `currArr`의 플레이어 위치와 이동한 위치 조정
   - command: q 실행 시 "Bye~" 를 외치며 퇴장

<br/>

### ⚾ 실행결과

 - `command: wddw`
  
![image](https://user-images.githubusercontent.com/72546335/144978423-aee00dce-4ee5-428b-b4a1-910fc423fc3c.png)

<br/>

 - `command: q`
  
![image](https://user-images.githubusercontent.com/72546335/144978528-9eb76469-c13b-4733-bd5c-c75b1ee8b23f.png)
 

<br/><br/>

# 😎 3단계: 소코반 게임 완성하기

## STEP : 플레이어 이동조건을 참고해 정상적인 소코반 게임을 완성하기

- 정상적인 소코반 게임을 완성한다.
- https://www.cbc.ca/kids/games/play/sokoban 를 참고하자.


### ⚽ 코드동작

`Level-3.js`

1. 입력창과 스테이지 지도파일 받기
   - `readline`과 `readFileSync`로 prompt 입력과, 5개의 스테이지가 있는 `sokobanMap.txt` 파일을 불러들인다.

2. prompt창에 입력할 때 마다 출력 함수 호출!
   - `printMap()` 함수는 prompt에 값이 실행될 때마다 현재 스테이지 배열인 `currStageArr`의 정보를 보기좋게 출력해준다.

3. 플레이어 이동 조건 설정하기
   - `2차원 배열`은 x축과 y축으로 표현할 수 있어 값을 특정할 수 있다.
   - 물론 1차원 배열도 값을 특정할 수 있지만 접근은 가능하고 수정은 불가능하다! 
   - `y, x` 좌표, 해당하는 방향에 어떤 것이 있을지 찾는 `mY, mX` 좌표, 돌을 밀어넣어야 하기에 그 뒤 `behindY, behindX`좌표 3가지를 설정한다.
   - 벽, 돌, 구멍, 구멍 안에 돌, 플레이어, 빈 공간 을 좌표로 값을 특정해 switch문으로 이동 조건을 설정한다.

4. prompt의 다양한 입력값
   - prompt에 들어오는 값들은 정규표현식(/^W$/)을 이용 한 글자만이 조건식에 통과된다.
   - `[W, A, S, D]`: 상하좌우 방향으로 이동. 돌이 있을 때 밀면서 같이 이동한다. 뒤에 벽이나 돌이 있을 경우 이동하지 않는다. 
   - `R`: `stageInfo.stageLevel`을 통해 해당하는 스테이지의 맵의 레벨을 찾아 초기화 한다. (다시하기)
   - `?`: 조작방법과 커맨드종류 그리고 해당하는 스테이지와 몇 턴을 사용했는지 보여준다.
   - `Q`: 게임을 종료한다. 



<br/>

### ⚾ 실행결과

- `시작`
  
![image](https://user-images.githubusercontent.com/72546335/145086728-a7d4050c-7c22-4077-9c06-7efff51731eb.png) 

<br/>

- `command: 틀릴 시 `

![image](https://user-images.githubusercontent.com/72546335/145092603-895486f4-14c2-4946-b4e7-30acd09f1f08.png)

<br/>

- `command: ?`

![image](https://user-images.githubusercontent.com/72546335/145092697-35cef2a9-68b4-4ca6-9122-333e8edc4a28.png)

<br/>

- `command: 게임동작 시현`

![image](https://user-images.githubusercontent.com/72546335/145094659-faf789c9-38d0-419a-a7de-78f132d89ced.png)



<br/><br/>

# 🥰 4단계: 추가기능 구현

## STEP 1 : 저장하기 불러오기 기능

### ⚽ 코드동작

`Level-3.js`

1. 현재 상태의 스테이지를 저장하기
   - 현재 스테이지 값은 `stageInfo.currStageArr` 객체가 계속해서 추적한다.
   - `saveDataFile`함수는 `writeFileSync`를 통해 `saveData.txt`에 현재의 스테이지 값을 저장한다.
   - 현재스테이지, 위치, 레벨 정보를 같이 저장한다.
   - 저장할 때는 현재 슬롯에 덮어쓴다.

2. 저장된 스테이지 불러오기
   - `saveData.txt`에 저장된 정보를 `JSON.parse`로 변환시켜 객체에 담는다.
   - 값을 쪼개서 `stageInfo` 객체안에 전부 담는다.
   - 초기화함수 `resetStage`를 실행한다.

3. 그외 동작
   - 저장슬롯은 정규표현식 조건을 걸어 1~5번 까지만 들어오게 했다.
   - `command: l` -> 저장된 슬롯 출력


<br/>

### ⚾ 실행결과

 - `command: ?`

![image](https://user-images.githubusercontent.com/72546335/145184639-f87a8afd-9a0b-4272-b23f-78afa7354f65.png)

  
- `command: l`

![image](https://user-images.githubusercontent.com/72546335/145184781-e5c90ca2-4481-4748-a25a-83b4940eb4aa.png)


- `command: [1-5]s`

 ![image](https://user-images.githubusercontent.com/72546335/145184959-ad9e6b89-fc8d-4475-b871-cc259fb2a4fe.png)


- `command: [1-5]l`

![image](https://user-images.githubusercontent.com/72546335/145185088-114538eb-6d29-41dd-9279-c77aaef24639.png)


<br/><br/>

## STEP 2 : 지도 데이터 변환하기 프로그램

- 미구현!

- 텍스트편집기로 읽을 수 없는 파일을 만드는 것 인지. 해쉬나 진수 등 어떤 조건으로 변환하는 것인지 헤깔렸다. 
  
- 시간이 없어 구현하지 못했지만 이후에 할 예정! 


<br/>


## STEP 3 : 되돌리기 기능 및 되돌리기 취소 기능 구현

- 미구현!

- Move함수를 실행하기 전 현재의 값을 기억하려고 시도
   - 스프레드 연산자를 사용했지만 깊은 복사가 되질 않았다.. 왜일까!?

- 고민하다 하나의 배열을 만들어 움직일 때마다 값을 넣은 후! 되돌리기 시 최대길이-1의 인덱스를 가져오는 방법을 생각했다.
   - 값이 어느 순간에 적용되지 않는 것 같다..

- 시간이 없어 전부 구현하지 못했지만 이후에 할 예정!


<br/>




<br/><br/>


# 🙄 생각 정리

- 문제 접근
   - 0, 1, 2, 3, 4 단계가 있었고 한 단계씩 차근차근 읽어봤다.
   - `gist`를 코드공유할 때 쓴 적은 있었지만 `repository`처럼 쓰는 것은 처음이어서 초반에 많이 헤멨다.. 😂 특히 `단계별 revision 번호`는 해쉬번호인지, index 번호인지 헤깔려서 문제를 전부 구현하고 나중에 알아보고자 했다!
   - 단계를 읽던 중 세 번째 단계에서 `.txt`파일을 이용해 입력값을 받는 것을 알게되어 첫 번째 단계 구현할 때 미리 `readFile`로 읽어왔다.
   - 읽어보니 단계별로 천천히 진행함에 따라 게임이 완성되가는 그림이라. 0단계부터 천천히 게임을 만들기 시작했다!
   - 게임을 직접 해보면서 공 뒤에 공있을 때의 조건같은거나 버그를 쉽게 잡을 수 있었다.

- 전역변수 문제
  - 전역변수를 최대한 사용하지 않으려고 노력했다..
  - 3단계에서 특히 전역객체를 설정해두었는데, 전역객체를 빼버리면 각 함수들의 파라미터가 너무 많아져 어쩔 수 없이 정의했다.
  - 함수의 파라미터로 전달받으면서 함수가 연속적으로 실행되는 것이 코드를 흐름대로 볼 수 있다 생각했는데, 이 함수가 어딨는지 찾고 다니는게 더 오래걸린 것 같다. 하나의 큰 함수(컨트롤러처럼) 에 사용되는 함수를 모아두는 것도 방법인 것 같다. (어떤 방법이 좋고 나쁜지 알고 싶다...)
  
  
- 리팩토링
   - 익숙하지 않아서 그런지 어떤식으로 수정해야할지, 손 대기가 힘들었다.
   - 한 번 완성됐다 생각한 코드는 다시 안보는 경향이 있는데, 요번 테스트를 계기로 리팩토링을 하면서 내가 가진 안좋은 코딩습관과 반복적인 코드를 줄여보았다.
   - 리팩토링은 끝이 없는 것을 느꼈다. 만족하진 못하지만 100% 만족하려면 시간이 부족 할 것 같다.


- 터미널로 만든 게임의 단점
   - 일단 눈이 너무 아프다... 게임 테스트를 할 때 한 스테이지당 수십번은 움직이게 되는데, 캐릭터가 이동하면서 렌더링 될 때마다 번쩍거려서 눈이 아팠다.
   - 한 글자 치고 엔터 누르다보면 빠르게 이동하고 싶을 때 연타하게 되는데 실수로 값이 여러번 찍혀 `ex) ddd` 실행이 안되는 점이 있었다. (조건을 바꾸면 되지만 엄격하게 하고 싶었다.)
   - 엔터를 치지않고 바로 실행되는 방법이 없는 것 같다... (된다 해도 `2s` 같은 것이 문제가 된다...!)
   - 다음에 만들 때는 html과 css로 출력을 게임스럽게 표현하는게 이쁠 것 같다.. 눈 건강에도 좋고


---

## 5단계까지 클리어~~ o((>ω< ))o

![image](https://user-images.githubusercontent.com/72546335/145194687-d51a6308-6c72-4f65-853d-a5f00dd85d21.png)
