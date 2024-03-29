# 파이썬 기초 2024
부경대 2024 IoT 개발자과정 기초 프로그래밍 언어 - 파이썬

## 1일차
- 개발환경 구축
    - 코딩폰트 - 나눔고딕코딩
    - Notepad++ 설치
    - Python 설치
    - Visual Studio Code 설치
    - Git 설치
        - TortoiseGit 설치
        - Github 가입
        - Github Desktop 설치

- 파이썬 기초
    - 콘솔출력
    - 주석    
    - 변수
    - 자료형
    - 연산자



    ```python
    # 이부분은 주석입니다.
    var01 - 110 # 정수, 실수, 불, 문자열 모두 가능
    print(var01)
    print(type(var01))

    print(5 + 4 / 2) # 7.0
    print(5 == 4) # False
    ```

## 2일차
- 파이썬 기초
    - 흐름제어
        - if : 참/거짓으로 조건 분기 (switch)
        - for : 반복문 기본 (다른언어 foreach)
        - while : 반복문 변형 (다른언어 do~while)
    - 복합자료형 + 연산자(연산함수)
        - 리스트, 튜플, 딕셔너리
    - 출력 포맷
    - 구구단 + 디버깅

    ```python
    #debugging
    # F9(중단점 토글), F5(디버깅시작), F10(한줄씩 진행), F11(함수안으로 진입)
    # Shift + F5(디버깅 종료)
    print('구구단 시작!')
    for x in range(2, 9+1): # 2부터 9까지 반복
        print(f'{x} 단 ----->')
        for y in range(1, 9+1): # 1부터 9까지 반복
            print(f'{x} x {y} = {x*y:2d}', end='  ') # end= 엔터대신 공백으로 변경
    print() # 안쪽 for문이 끝나면 마지막 엔터를 하나 추가
    ```

## 3일차
- 파이썬 기초
    - 입력 방법
    - 별찍기(피라미드 만들기)
    - 함수, 람다함수는 나중에...
    - 객체지향 00P
        - 객체는 명사와 동사의 집합
        - 명사는 변수, 동사는 함수
        - 변수와 함수를 모두 모아둔 곳 : 클래스(class)
        - 클래스에서 하나씩 생성 : 객체(object)
        - 캡슐화 (__plateNumber)
    - 패키지, 모듈

## 4일차
- 파이썬 기초
    - 패키지, 모듈, 계속
        - pip 사용

        ```shell
        > pip --version # 버전확인
        > pip list # 현재 설치된 라이브러리 목록 확인
        > pip install 패키지명 # 패키지를 내 컴퓨터에 설치
        > pip uninstall 패키지명 # 패키지를 삭제
        ```
    - 예외 처리 : 비정상적 프로그램종료 막기

```python
def divide(x, y):
    try:
        return x / y # ZeroDivisionError 발생
    except ZeroDivisionError as e:
        print('[오류!!] 제 수는 0 될 수 없습니다. 제발')
        return 0
```
    - 텍스트 파일 입출력

    ```python
    f = open('파일명', mode='r|w|a', encoding='cp949|utf-8')
    f.read()
    f.readline() # 읽기
    f.write('text') # 쓰기
    f.close() # 파일은 반드시 닫는다
    ```
- 파이썬 응용
    - 주피터 노트북
        - Ctrl + Shift + P (명령팔레트) 로 시작
        - 사용방법 (test31_jupytermb.ipynb 참조)
    - folium 기본사용
    ![folium사용법](https://raw.githubusercontent.com/MsgIsJmt/basic-python-2024/359a16d0ed3158a0b0bf18d8637d1491035c307a/images/Image20240201172037.png)

## 5일차
- 파이썬 응용
    - 주피터 노트북 활용 - 구글 코랩(colab)
    - folium 계속
    - json 입출력
    - 응용예제 연습(10개)
        - IP주소 확인
        - QRCODE 만들기

## 6일차
- Python 라이브러리 경로 : C:\DEV\Langs\Python311\Lib\site-packages 
- 파이썬 응용
     - Window App(PtQt) 만들기

    ```shell
    > pip install PyQt5
    > pip install PyQt5Designer
    ```

    - PtQt5 기본실행
    - QtDesigner 사용법
    - ☆☆☆쓰레드 학습 : UI쓰레드와 Background쓰레드 분리
        - [ ] GIL, 병렬프로세싱 더 학습할 것

    ![쓰레드예제](https://raw.githubusercontent.com/hugoMGSung/basic-python-2024/main/images/python_003.gif)

    ```python
    # 쓰레드 클래스에서 시그널 선언
    class BackWorker(QThread): # PyQt에서 스레드 클래스 상속
        initSignal = pyqtSignal(int) # 시그널을 UI스레드로 전달하기위한 변수객체
        # ...

        def run(self) -> None: # 스레드 실행
            # 스레드로 동작할 내용
            maxVal = 1000001
            self.initSignal.emit(maxVal) # UI쓰레드로 보내기...
            # ...

    class qtwin_exam(QWidget):  # UI 스레드
        # ...
        def btnStartClicked(self):
            th = BackWorker(self)
            th.start() # BackWorker 내의 self.run() 실행
            th.initSignal.connect(self.initPgbTask) # 스레드에서 초기화 시그널이 오면 initPgbTask 슬롯함수가 대신 처리
            # ...

    # 스레드에서 시그널이 넘어오면 UI처리를 대신 해주는 슬롯함수
    @pyqtSlot(int) # BackWorker 스레드에서 self.setLog.emit() 동작해서 실행
    def initPgbTask(self, maxVal):
        self.pgbTask.setValue(0)
        self.pgbTask.setRange(0, maxVal-1)
    ```

## 7일차
- 파이썬 응용
    - 객체지향 정리
        - 상속, 오버라이딩(재정의), 오버로딩(같은 이름의 함수를 마음대로 골라쓰자)
    - 가상환경 Virtualenv
        - 다른 버전 파이썬도 설치해야 사용 가능
        - 현재 3.11에서 3.9 가상환경 만들려면 3.9 파이썬 설치필요
    - PyQt5와 응용예제 연습
        - 이미지 뷰어
        - 이미지 에디터

        ![PyQt예제](https://raw.githubsercontent.com/hugoMGSung/basic-python-2024/main/images/python_004.png)

 ## 8일차
- 파이썬 응용
    - PyQt5 응용예제 계속

- 파이썬 기본 코딩 테스트
    - 주피터노트북 활용

## 추가
- 파이썬 실행파일 만들기
    - PyQt yi파일이나 이미지파일의 경로가 절대경로로 지정
    - pip install pyinstaller 패키지 설치
    - pyinstaller -w -F 파이썬.py (-w: 콘솔창 없애기, -F: 실행파일 하나로 만들기)


    - 객체지향(나중에...)
        - 오버로딩, 오버라이딩(재정의)
        - 상속, 다중상속
        - 추상클래스, 인터페이스...


