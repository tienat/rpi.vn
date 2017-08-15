+++
date = "2017-08-14T23:47:03+07:00"
description = ""
images = []
menu = ""
categories = ["tutorial"]
tags = ["tutorial"]
title = "Điều chế độ rộng xung PWM trên Raspberry PI 3"
slug = "pwm-led-python"
banner = "post/begin/pwm/banner.png"
+++

# Tổng quan
- Trong bài học "Điều khiển nhấp nháy LED trên Raspberry PI 3 sử dụng Python" bạn đã được tìm hiểu về điều khiển LED thông qua chân GPIO của Raspberry PI 3. Trong bài học này, bạn sẽ học cách điều khiển độ sáng của LED thông qua một khái niệm đó là "Điều chế độ rộng xung PWM". Vậy PWM là gì:
    - Pulse Width Modulation (hay PWM): là phương pháp điều chỉnh năng lượng điện áp ngõ ra.
    - Khác với phương pháp Digital Output (được sử dụng trong bài "Điều khiển nhấp nháy LED") chỉ có thể xuất ra 2 mức năng lượng điện áp là 0% (LOW) và 100% (HIGH) thì với PWM bạn có thể xuất ra tất cả các mức năng lượng từ 0% đến 100%
    - Dưới đây là sơ đồ cho thấy tín hiệu từ chân PWM của Raspberry PI

    ![Sơ đồ tín hiệu từ chân PWM](/post/begin/pwm/01.png)

    - Cứ mỗi 1/500 giây, chân PWM sẽ tạo ra một xung. Chiều dài của xung này kiểm soát mức năng lượng đưa ra bởi chân PWM. Khi không có xung mức năng lượng sẽ là 0%, nếu có xung ở nữa chu kỳ thì mức năng lượng sẽ là 50%. Vậy, ứng với mỗi độ rộng xung trong một chu kỳ bạn sẽ nhận được một mức năng lượng tương ứng.

# Thực hiện
## Phần cứng
- Một Raspberry PI 3
- Một bộ nguồn 5V-2.5A (theo khuyến cáo của nhà sản xuất nên dùng nguồn lớn hơn 2A)
- Một thẻ nhớ đã cài hệ điều hành Raspbian và đã cài đặt wifi
- Một máy tính kết nối chung wifi với Raspberry
- Đèn LED
- Điện trở 470 Ohm
- Breadboard
- Dây cắm

## Kết nối
- Ta vẫn giữ kết nối như ở bài "Điều khiển nhấp nháy LED trên Raspberry PI 3 sử dụng Python"

![Kết nối LED với Raspberry PI 3](/post/begin/pwm/02.png)

## Giới thiệu về API GPIO cho PWM trên python
- Tương tự như bài "Điều khiển nhấp nháy LED trên Raspberry PI 3 sử dụng Python", ta vẫn sẽ sử dụng `import RPi.GPIO as GPIO` để gọi module điều khiển GPIO.
- Cài đặt chân số 11 trên board làm ngõ ra

  ```
  GPIO.setmode(GPIO.BOARD)
  GPIO.setup(11, GPIO.OUT)
  ```
- Để khởi tạo PWM, sử dụng hàm `GPIO.PWM([pin], [frequency])`
- Cho phép chân PWM hoạt động bằng lệnh `pwm.start([duty cycle])`, giá trị [duty cycle] có thể thay đổi từ 0 đến 100 ứng với mức năng lượng 0% đến 100%
- Ví dụ: Khởi tạo chân số 11 với tần số 1KHz, và điều chế xung ở mức 50%

```
pwm = GPIO.PWM(11, 1000)
pwm.start(50)
```
- Để thay đổi giá trị đầu ra, dùng hàm `pwm.ChangeDutyCycle([duty cycle])`

  ```
  pwm.ChangeDutyCycle(75)
  ```
- Khi không sử dụng chức năng này nữa, ta có thể tắt bằng `pwm.stop()`

## Lập trình
- Sử dụng máy tính kết với với Raspberry thông qua terminal (Ubuntu) hay Putty (Windows)
- Khi đã hoàn thành kết nối, thực hiện lệnh: `sudo nano pwm.py` để tạo file pwm.py hoặc mở file pwm.py có sẵn
- Nhập vào đoạn chương trình:

```
import RPi.GPIO as GPIO
import time
LedPin = 11                         # Pin 11

GPIO.setmode(GPIO.BOARD)            # Numbers GPIOs by physical location
GPIO.setup(LedPin, GPIO.OUT)        # Set LedPin's mode is output

pwm = GPIO.PWM(LedPin, 50)          # Frequency=50Hz
pwm.start(0)                        # Start with PWM = 0
try:
  while 1:
    for i in range(0, 101, 5):      # increase PWM 0 - 100
      pwm.ChangeDutyCycle(i)
      time.sleep(0.1)
    for i in range(100, -1, -5):    # decrease PWM 100 - 0
      pwm.ChangeDutyCycle(i)
      time.sleep(0.1)
except KeyboardInterrupt:           # When 'Ctrl+C' is pressed, the child program destroy() will be executed.
  pass
  pwm.stop()                        # Stop PWM
  GPIO.cleanup()                    # Release resource
```
- Lưu chương trình bằng cách nhấn Ctrl+X -> Y -> Enter
- Để chạy chương trình, thực hiện lệnh `sudo python pwm.py`. Nếu chương trình chạy đúng bạn có thể thấy LED chớp tắt với chu lỳ 1 giây.

Trên đây là toàn bộ hướng dẫn giúp bạn hiểu rõ hơn về điều chế độ rộng xung PWM và cách sử dụng nó trên Raspberry PI, với ví dụ minh họa là thay đổi độ sáng của LED. Chúc các bạn thành công.
