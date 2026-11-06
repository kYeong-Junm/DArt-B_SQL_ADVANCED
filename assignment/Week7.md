# SQL_ADVANCED 7주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_7th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=KZmW6VaY5BU&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=16
https://www.youtube.com/watch?v=vWTDuoSG-YQ&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=17
https://www.youtube.com/watch?v=aiMSluMNzI8&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=18
-->

**교재 실습 예제 파일은 08_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_7th_TIL

### 8장 SQL과 파이썬 연결 
#### 01. 파이썬 개발 환경 준비
#### 02. 파이썬과 MySQL의 연동
#### 03. GUI 응용 프로그램 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | ✅         |
| 4주차 | p.216~271 | ✅         |
| 5주차 | p.274~327 | ✅         |
| 6주차 | p.330~369 | ✅         |
| 7주차 | p.372~407 | ✅         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 파이썬 개발 환경 준비

<!-- 이번 챕터에서 제시된 실습을 흐름에 맞게 진행한 후, 실습 과정이 보일 수 있도록 인증 사진을 2장 이상 제출해 주세요. -->

<img width="807" height="837" alt="image" src="https://github.com/user-attachments/assets/eedfda46-f975-4755-aa26-07ab40b04b0c" />
<img width="917" height="597" alt="image" src="https://github.com/user-attachments/assets/da94e7ed-fc6a-49e8-9999-3cbbb4ca2ca5" />



## 2. 파이썬과 MySQL의 연동

<!-- 이번 챕터에서 제시된 실습을 흐름에 맞게 진행한 후, 실습 과정이 보일 수 있도록 인증 사진을 2장 이상 제출해 주세요. -->

<img width="767" height="1007" alt="image" src="https://github.com/user-attachments/assets/bbf99922-a8d8-4669-8462-d9253473118b" />
<img width="902" height="367" alt="image" src="https://github.com/user-attachments/assets/44759feb-b7a5-48e2-a645-6d5e8fedaa7f" />
<img width="892" height="892" alt="image" src="https://github.com/user-attachments/assets/62ba9341-5bc2-49f7-8d44-e9c4b01ce6f5" />



## 3. GUI 응용 프로그램 

<!-- GUI 응용 프로그램에 관해 배우게 된 점을 적어주세요. -->

---

## GUI와 tkinter

- **GUI(Graphical User Interface)**: 윈도에 그래픽 환경으로 제공되는 화면을 통틀어 말함
- **tkinter**: 파이썬에서 GUI 응용 프로그램(엑셀, 한글, 크롬 같은 형태)을 만들 때 쓰는 **표준 윈도 라이브러리**
- SQL을 모르는 사용자도 **마우스 클릭만으로 데이터 입력·조회**가 가능하도록 만들 수 있음

---

## 기본 윈도의 구성

```python
from tkinter import *     # ① tkinter 사용 선언 (윈도 및 관련 내용 사용 가능)

root = Tk()               # ② 기본 윈도(루트 윈도) 생성

# 이 부분에 코드를 추가해서 화면을 구성하고 처리

root.mainloop()           # ③ 이벤트 처리 시작
```

| 코드 | 설명 |
|---|---|
| `from tkinter import *` | 윈도가 필요할 때 꼭 써야 하는 import 문 |
| `Tk()` | 기본이 되는 윈도(**루트 윈도, root window**)를 반환. 이 줄이 실행될 때 윈도가 출력됨 |
| `root.mainloop()` | 키보드 누르기, 마우스 클릭 등 **이벤트를 처리**하기 위해 필요. 창을 닫을 때까지 계속 실행됨 |

**윈도 제목과 크기 지정**

```python
root = Tk()
root.title("혼공 GUI 연습")     # 윈도 제목 표시
root.geometry("400x200")        # 초기 크기를 400x200으로 지정 (가로x세로)
root.mainloop()
```

---

## 위젯(widget)

윈도에 나오는 **버튼, 텍스트, 라디오 버튼, 이미지 등을 통합해서 지칭**하는 용어. 모든 위젯은 **`pack()` 함수를 사용해야 화면에 나타남**.

### 라벨(Label) — 문자 표현

형식: `Label(부모윈도, 옵션...)`

```python
label1 = Label(root, text="혼공 SQL은")
label2 = Label(root, text="쉽습니다.", font=("궁서체", 30), bg="blue", fg="yellow")

label1.pack()
label2.pack()
```

| 옵션 | 설명 |
|---|---|
| `text` | 글자 내용 |
| `font` | 글꼴과 크기 (예: `("궁서체", 30)`) |
| `fg` | foreground의 약자, **글자색** |
| `bg` | background의 약자, **배경색** |

> 💡 `pack()`을 호출하지 않으면 위젯이 화면에 표시되지 않음. 부모 윈도를 `root`로 지정했으므로 루트 윈도에 라벨이 출력됨.

### 버튼(Button) — 클릭 시 작업 실행

형식: `Button(부모윈도, 옵션...)`. GUI에서 상당히 많이 쓰이는 위젯.

- 라벨과의 차이점: **`command` 옵션**으로 버튼을 눌렀을 때 실행할 작업(함수)을 지정함. 나머지 옵션은 라벨과 거의 동일

```python
from tkinter import *
from tkinter import messagebox      # ① 메시지 상자 사용을 위한 import

def clickButton():
    messagebox.showinfo('버튼 클릭', '버튼을 눌렀습니다..')

root = Tk()
root.geometry("200x200")

button1 = Button(root, text="여기를 클릭하세요", fg="red", bg="yellow",
                 command=clickButton)       # ② command에 함수 지정
button1.pack(expand=1)                      # ③ expand=1: 버튼을 화면 중앙에 표현

root.mainloop()
```

> 📝 `command=clickButton`처럼 **함수 이름만** 지정함 (괄호 `()`를 붙이지 않음). 버튼 클릭 시 메시지 상자가 나타남.

---

## 위젯의 정렬과 여백 — `pack()` 옵션

### 정렬: `side`

| 값 | 설명 |
|---|---|
| `LEFT` | 왼쪽부터 가로로 채움 |
| `RIGHT` | 오른쪽부터 가로로 채움 |
| `TOP` | 위에서부터 수직으로 정렬 |
| `BOTTOM` | 아래에서부터 수직으로 정렬 |

```python
button1.pack(side=LEFT)
button2.pack(side=LEFT)
button3.pack(side=LEFT)       # 혼공1 혼공2 혼공3 (가로 배치)
```

> ⚠️ `UP`이라는 값은 없음. 수직 정렬은 `TOP` / `BOTTOM`을 사용.

### 여백: `padx`, `pady`

위젯 사이에 여백을 주는 옵션. **`padx=픽셀값`**(가로), **`pady=픽셀값`**(세로) 방식으로 사용.

```python
button1.pack(side=TOP, fill=X, padx=10, pady=10)
```

- `fill=X`: 가로 방향으로 채움

---

## 프레임, 엔트리, 리스트 박스

| 위젯 | 설명 |
|---|---|
| **프레임(Frame)** | 화면을 여러 구역으로 **나눌 때** 사용. 구역만 나눌 뿐 화면에는 표시되지 않음 |
| **엔트리(Entry)** | **입력 상자**를 표현 |
| **리스트 박스(Listbox)** | **목록**을 표현 |

```python
from tkinter import *
root = Tk()
root.geometry("200x250")

upFrame = Frame(root)             # ① 프레임 2개 생성 후 pack
upFrame.pack()
downFrame = Frame(root)
downFrame.pack()

editBox = Entry(upFrame, width=10)         # ② 부모를 root가 아닌 upFrame으로 지정
editBox.pack(padx=20, pady=20)

listbox = Listbox(downFrame, bg='yellow')  # ③ 리스트 박스는 아래쪽 downFrame에 표시
listbox.pack()

listbox.insert(END, "하나")                # ④ END: 데이터를 제일 뒤에 첨부
listbox.insert(END, "둘")
listbox.insert(END, "셋")

root.mainloop()
```

> 💡 위젯을 만들 때 **첫 번째 매개변수(부모)**로 지정한 곳에 위젯이 출력됨 (root 또는 특정 프레임).

---

## 완전한 형태의 GUI 응용 프로그램

SQL과 연동해서 **GUI 화면에서 데이터 입력·조회**가 가능한 프로그램 (`userTable` 사용).

### 프로그램 구조

```
[메인 코드부]  윈도 / 프레임 / 엔트리 4개 / 버튼 2개 / 리스트 박스 4개 구성
      ├─ [입력] 버튼 → insertData() 호출
      └─ [조회] 버튼 → selectData() 호출
```

---

이번 주는 마지막 주로 별도의 실습 과제는 진행하지 않습니다.

한 학기 동안 다소 부족한 커리큘럼이었음에도 불구하고 성실하게, 그리고 열심히 과제를 수행해주셔서 진심으로 감사드립니다.

이번 과정을 통해 배운 내용이 앞으로 여러분이 SQL을 다루는 데에 조금이나마 도움이 되기를 바랍니다.

여러분의 앞날을 진심으로 응원합니다. 😊


### 🎉 수고하셨습니다.






