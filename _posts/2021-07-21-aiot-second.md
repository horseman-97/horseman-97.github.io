---
title: "AIoT 수업 2일차"
excerpt: Maixduino 개발환경 설정과 디지털 입출력

categories: AIoT

published: false

tags:
 - [MicroPython, Microcontroller, LED, Button]

date: 2021-07-20
last_modified_at: 2021-07-26
---
## MicroController

![image](https://user-images.githubusercontent.com/59239144/126373374-e6e6ecff-6d5a-450b-b460-0225fb2483ce.png)

- Maixduino를 설명하기 전에 먼저 마이크로컨트롤러에 대해 알아보자.

- 마이크로컨트롤러라는 이름의 반도체와 주변 장치들이 합쳐져서 board가 된다고 생각하면 된다.

	![image](https://user-images.githubusercontent.com/59239144/126373404-a4c7bd45-659e-4fa3-9ef8-7bd863e8cad4.png)

- 마이크로컨트롤러는 저 그림에서 중앙에 있는 연산 기능만 가지고 있는 것이다.

- 그림의 '일부 HDD 기능'은 flash memory가 존재하기 때문에 그걸로 처리할 수 있다.

  ![image](https://user-images.githubusercontent.com/59239144/126373449-87a8a033-fa02-4d10-97bb-cc302dd4e10a.png)

- 마이크로프로세서와는 다르다. 마이크로프로세서는 컴퓨터의 일반적인 CPU라고 생각하면 되지만, 마이크로컨트롤러는 거의 모든 전자제품에 다 들어 있는 싱글 칩 컴퓨터이다.

- 즉 컴퓨터의 부품이냐 vs 컴퓨터 그 자체이냐의 차이.

- | 항목        | Arduino Mega2560     | 일반 데스크톱    |
  | ----------- | -------------------- | ---------------- |
  | CPU         | ATmega2560           | Intel Core i7    |
  | 비트        | 8                    | 64               |
  | 메모리      | 256KB                | 8GB              |
  | 클록        | 16MHz 싱글코어       | 3.4GHz 쿼드코어  |
  | 시스템 가격 | 25,000원(ATmega2560) | 1,000,000원 정도 |

당연히 비트, 즉 한 번에 처리 가능한 자료 양에서 차이가 있다. 각각 2의 8승과 2의  64승만큼 한 번에 계산 가능하다. 그리고 메모리, 즉 RAM도 일반 컴퓨터는 8GB, 16GB가 일반적이지만 마이크로컨트롤러는 단위가 KB이다. 실행속도인 클록 역시 큰 차이가 있다.

![image](https://user-images.githubusercontent.com/59239144/126373468-8382065c-e69f-4ae9-a94e-01e7d2eccb11.png)

- 이렇게 성능이 낮지만, 단순 동작을 요하는 영역에서 아주 폭넓게 많이 쓴다.

---
## Maixduino
- 앞서 마이크로프로세서와 비교하면서 Arduino를 잠깐 언급했다.

- 하지만 우리는 이번에  Maixduino를 쓸 것이다.

  ![image](https://user-images.githubusercontent.com/59239144/126867413-ed467f9a-dd1d-4a29-ba36-5e60d12d1372.png)

- 이것 역시 주변기기가 존재하고, ESP32 모듈이라는 WIFI와 Bluetooth 통신을 위한 마이크로컨트롤러가 따로 존재한다.

  ![sipeed_maixduin_pins](https://user-images.githubusercontent.com/59239144/126868313-9d4f6892-98d0-46cd-9bec-02c3a88aedda.png)

- 저 보드를 뒤집으면 저런 모양이 나온다. pin마다 이름이 할당되어 있다.

- 초록색은 Maixduino 뒷면에 적힌 pin 번호이고, Maixduino의 메인 칩, 즉 K210의 핀 번호는 파란색이다. 왜 저렇게 나눠놨냐면, 아두이노 기반으로 기존 library가 존재하고 있고, Maixduino는 그걸 가져다 쓰는 식으로 작동하기 때문이다.

- 우리가 실제로 코드에 쓸 번호는 파란색 K210 번호이다.

  ![image](https://user-images.githubusercontent.com/59239144/126872511-70fedc61-12c4-444c-b188-d6bb95c30e7a.png)

- 하드웨어 스펙에 대해 간략히 알아보자.

- High-end SOC. 즉 CPU보다는 성능이 낮고, MCU보다는 성능이 높다고 보면 된다.

- 듀얼코어이고 RISC 아키텍쳐를 제공한다. FPU, 즉 부동소수점 계산 모듈을 제공한다. 연산이 빠르기 때문에 image recognition, 즉 매트릭스 연산도 가능하다.

- 그리고 오디오 지원, 보안 강화, 절전 모드 등의 설정이 있다.

- 마지막으로 다른 microcontroller들과 차별화되는 점은, DNN(Deep Neural Network) framework, 예를 들면 Tensorflow나 KNN을 사용 가능하다는 점이다. 이 기능을 이용해서 AIoT 수업을 진행할 것이다.

### MicroPython - MaixPy

- python을 이용해 microcontroller를 돌려 보기 위해 나온 게 micropython이다. 이렇게 하면 Python으로 디바이스 동작과 AI를 모두 처리할 수 있다.

- 구동 드라이버와 IDE, GUI는 아래 주소에서 받을 수 있다.

  - http://dl.sipeed.com/shareURL/MAIX/MaixPy/ide/v0.2.5

  - https://dl.sipeed.com/MAIX/tools/ftdi_vcp_driver

  - https://github.com/sipeed/kflash_gui/releases/tag/v1.6.7
---

## PIN mapping & LED

![image](https://user-images.githubusercontent.com/59239144/126886757-7b95043c-fcb6-48af-bd09-afc0d04761ac.png)

- LED는 전기가 흐르면 빛을 내는 다이오드이다. 다이오드란 무엇인가? 양극에서 음극으로만 전류가 흐르는 소자이다.
- 즉 LED역시 한 가지 방향으로만 전류가 흘러간다. 따라서 LED 다리를 양극과 음극을 서로 바꿔 꽂으면 불이 들어오지 않는다.<br /> 긴 다리가 양극, 짧은 다리가 음극임을 기억하자.

![image](https://user-images.githubusercontent.com/59239144/126889075-b859aa27-c788-4699-b569-7d4b4aca45ee.png)

- 저 보드는 아두이노지만 저걸 maixduino라고 생각하고 보자. 저렇게 LED와 보드를 연결한 후 IDE에 코드를 입력해 LED 제어를 할 수 있다.

  ![image](https://user-images.githubusercontent.com/59239144/126894335-3f537b9b-ece9-411b-b313-a96370762f82.png)

- 옛날에는 칩마다 PIN 번호가 존재했고, 그 PIN마다 기능이 모두 정해져 있었다.

- 문제는 이렇게 핀마다 기능이 정해진 보드에 코드를 잘못 짜면, 회로 짜는 비용이 수백만원이 들어도 전체를 바꿔야 한다.

- 이런 문제를 해결하기 위해서 software mapping 개념이 적용되어서, analog pin이 아닌 digital pin을 적용한다.

  ![image](https://user-images.githubusercontent.com/59239144/126899346-860014eb-8fb1-453e-aef5-736edc6ee30a.png)


- 파이썬 적용해서 FIOA를 써 보자. 저기서 숫자는 앞서 말한 대로, K210 핀 번호를 사용한다.<br />

  그리고 attribute를 줘서 해당 기능으로 사용할 수 있게 한다. 예를 들어서 11번 PIN은 저기서 GPIO0번으로 쓴다.<br />

  즉, 사용하고자 하는 K210 PIN 번호와 GPIO 번호를 fpioa_manager에 등록해야 한다.

  ![image](https://user-images.githubusercontent.com/59239144/126905621-c83fb50c-2d6c-409d-91e5-3a65c5792aba.png)

- GPIO는 여러 가지 목적의 입출력 PIN을 말한다. 예를 들어서 LED를 켜는 건 입출력 중 출력에 해당한다.

- 이제 LED 출력, 즉 LED를 켜는 코드를 보자.

  ```python

  from fpioa_manager import fm
  from Maix import GPIO
  import utime

  io_led_red = 15
  fm.register(io_led_red, fm.fpioa.GPIO0)

  led_red = GPIO(GPIO.GPIO0, GPIO.OUT)
  
  while(True):
      led_red.value(1)	
      utime.sleep_ms(1000)
      led_red.value(0)	
      utime.sleep_ms(1000)
  ```

- 1초마다 LED가 깜빡이게 하는 코드이다. 1000ms가 1초에 해당하므로, `sleep_ms(1000)`을 주면 된다.

- 이것 역시 utime 라이브러리에서 선택해서 초 단위를 ms, us, s로 바꿀 수 있다.

- `value(1)`은 LED를 켜고, `value(0)`은 끈다.

- 다음 코드는 4개의 LED를 제어하기 위한 코드이다.

  ```python
  from fpioa_manager import fm
  from Maix import GPIO
  import utime
  
  io_led_blue = 15
  io_led_blue2 = 32
  io_led_green = 24
  io_led_red = 23
  
  fm.register(io_led_blue, fm.fpioa.GPIO0)
  fm.register(io_led_blue2, fm.fpioa.GPIO1)
  fm.register(io_led_green, fm.fpioa.GPIO2)
  fm.register(io_led_red, fm.fpioa.GPIO3)
  
  led_b = GPIO(GPIO.GPIO0, GPIO.OUT)
  led_b2 = GPIO(GPIO.GPIO1, GPIO.OUT)
  led_g = GPIO(GPIO.GPIO2, GPIO.OUT)
  led_r = GPIO(GPIO.GPIO3, GPIO.OUT)
  
  while(True):
      led_b.value(1)
      led_b2.value(1)
      led_g.value(1)
      led_r.value(1)
      utime.sleep_ms(500)
      led_b.value(0)
      led_b2.value(0)
      led_g.value(0)
      led_r.value(0)
      utime.sleep_ms(500)
  ```

---

## Button


- 버튼은 스위치를 통해서 다른 디바이스를 제어할 수 있는 디바이스이다.

  ![image](https://user-images.githubusercontent.com/59239144/126907240-4713b0a8-abf4-4ec6-bfff-bba3ea9f98ee.png)

- 버튼에 다리가 4개 있는데, 그림에서 1번과 3번, 2번과 4번이 같은 다리이다. 즉 버튼이 눌리면 1, 2, 3, 4에 모두 전류가 흐른다.

- 그리고 breadboard, 즉 빵판 가운데 꽂아야 전류가 흐른다.

- 버튼 한 쪽은 VCC, 즉 3.3V 전원에 꽂아 두고, 다른 한 쪽은 21번 선에 꽂아 두자.

- 그럼 일단 이렇게 짜 보자.

  ```python
  from fpioa_manager import fm
  from Maix ipmort GPIO
  import utime

  io_btn = 22
  fm.register(io_btn, fm.fpioa.GPIO1)
  btn = GPIO(GPIO.GPIO1, GPIO.IN)

  while(True):
      btn_state = btn.value()
      print(btn_state)
  ```

- 이렇게 하면 output 창에 0과 1이 끝없이 반복되다고 버튼이 눌릴 때만 하나의 값으로 고정된다. 왜 그럴까?

>Floating
>
> - 왜 그럴까? 공기 중이나 보드 내 다른 기기들에 전하가 남아 있기 때문에, 0과 1로 완벽하게 나누어지지 않고, 0의 상태라고 해도 약 0.3  정도의 전위를 유지한다고 알려져 있다. 
> 따라서 전류를 흘려보내지 않았는데, 전류를 흘려보내는 것과 같은 현상이 발생한다. 이 현상을 [floating](https://en.wikipedia.org/wiki/Floating_ground) 현상이라 한다.
>- 즉 스위치를 누르지 않았지만 누르는 것과 같은 효과가 발생하는 것이다.

  ![image](https://user-images.githubusercontent.com/59239144/126908833-12f3ebe4-49e2-44b7-be33-082922e939be.png)

- 그럼 강제로 0.3 값을 0으로 만들어 버리면 해결이 된다. 끌어 내리는 방식이니 이 방식을 pull - down 저항이라고 하고, 회로도에서 확인 가능하듯이 register를 걸어서 전류 흐름을 방해한다. 그런데 왜 GND에 저항을 연결했을까?

-  VCC로부터 전류가 흐르면 D14로도 가지만, GND에도 연결되어 있으니 전류가 그쪽으로도 흘러간다. 문제는 VCC가 +극이고 GND가 -극이니, 합선이 나서 아주 따끈따끈해진다. 만약 5V 정도의 전압이면 회로가 타거나 빵판이 녹아내릴 것이다...

- 저기 저항을 연결한 건 그런 이유이다. VCC에서 GND로 전류가 가도, 저항을 연결해 주면 흐름에 방해를 받아서 GND로 안 가고, D14로 갈 것이다.

  ![image](https://user-images.githubusercontent.com/59239144/126908894-7bae5df1-4cfa-48d4-a58c-9605c9fdd0b6.png)

- 반대로 아예 1로 올려 버려서 해결할 수도 있는데, 이 경우를 pull-up 저항이라고 한다.

  ![image](https://user-images.githubusercontent.com/59239144/126908970-8db6e4c9-9f2d-4184-8da3-39565a2b1c57.png)

- 하지만 Maix 라이브러리는 아주 좋은 모드이다 ~~역시 비싼 게 최고다~~ . GPIO Pull mode를 사용하면 pull up&down 모두<br />소프트웨어로 구현 가능하다. 물론 K210 내부 회로 역시 저걸 쓸 수 있게 되어 있다.

  ```python
  from fpioa_manager import fm
  from Maix import GPIO
  import utime

  io_btn = 22
  fm.register(io_btn, fm.fpioa.GPIO1)
  btn = GPIO(GPIO.GPIO1, GPIO.IN, GPIO.PULL_DOWN)

  while(True):
      btn_state = btn.value()
      print(btn_state)
  ```

- 이렇게 GPIO.PULL_DOWN을 걸어 주면 버튼을 누를 때만 1로 올라가고 나머지 상태에선 0으로 계속 유지가 된다.

- 이걸 배웠으니까 이제 버튼이 눌린 횟수도 셀 수 있다. count 변수 하나 설정해 두고, 그냥 count += 1 하면 되지 않나?

- 근데 그게 그렇지가 않다. 만약 다음과 같이 코드를 짠다고 생각해 보자.
  ```python
  from fpoia_manager import fm
  from Maix import GPIO
  import utime
  
  io_btn = 22
  fm.register = (io_btn, fm.fpioa.GPIO1)
  btn = GPIO(GPIO.GPIO1, GPIO.IN, GPIO.PULL_DOWN)
  count = 0
  while(True):
      btn_state = btn.value()
      if btn_state == 1:
          count += 1
      print(count)
  ```


- 이렇게 짜면 버튼이 한번 눌릴 때 마다 count 값이 몇만 단위로 늘어난다. 

- 왜? while문이 1초에도 몇만 번씩 돌아가니까.

- 이건 sleep 함수를 준다고 해서 해결이 안 된다.

  ```python
  from fpioa_manager import fm
  from Maix import GPIO
  import utime
  
  io_btn = 22
  fm.register(io_btn, fm.fpioa.GPIO1)
  btn = GPIO(GPIO.GPIO1, GPIO.IN, GPIO.PULL_DOWN)
  count = 0
  
  while(True):
      state_current = btn.value()
      if state_current == 1:
          if state_previous == 0:
              count += 1
              state_previous = 1
              print(count)
          utime.sleep_ms(100)
      else:
          state_previous = 0
  ```

- 이렇게 state_count와 state_previous로 분기를 나눠 줘야 해결된다.

- 이러면 꾹 누르고 있으면 count가 안 올라가고, 한번 눌러줄 때만 1씩 올라간다.

- 그런데 `utime.sleep_ms(100)`은 왜 있는 걸까? chattering 해결을 위해서이다.


  ![image](https://user-images.githubusercontent.com/59239144/126931463-3885af0b-2c5b-4828-8160-26dffa8860b1.png)



> Chattering  & Debouncing
>
> - 버튼은 철판이 눌렸다가 다시 보강이 되면서 진동이 일어나는 원리로 작동한다.
> - 즉 버튼을 누르고 있는 상태인데도 진동에 의해 아주 짧은 시간 동안 버튼을 눌렀다가 뗀다고 인식이 될 수 있다.<br />난 한번 눌렀는데 여러 번 눌린 것으로 처리된다는 것. 그래서 ON/OFF가 반복된다.
> - 이로 인해 발생하는 하드웨어적 오류를 [chattering](https://en.wikipedia.org/wiki/Switch#Contact_bounce)이라 한다. 
> - 이를 해결하기 위한 방법은 위의 링크에도 설명되어 있듯, 디바운싱(debouncing)이다.
> - 채터링이 짧은 시간동안 지속되므로, 일정 시간 동안 코드 실행을 중지시킨 다음,<br />버튼의 상태를 검사하면 된다.

- 디바운싱은 하드웨어적 방법과 소프트웨어적 방법이 있다. 여기서는 소프트웨어적 방법을 택했다.
- 사람이 누를 수 있는 최소한의 시간을 100ms로 상정하고 그만큼 sleep를 걸어 줬다.



### Reference

[chattering and debouncing](https://juahnpop.tistory.com/39)

- 아두이노 UNO관점에서 채터링이 왜 발생하는지, 회로적 & 소프트웨어적으로 어떻게 디바운스를<br/> 구현하는 지 잘 설명되어 있다.
