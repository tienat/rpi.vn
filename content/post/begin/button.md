+++
date = "2017-08-15T11:47:03+07:00"
description = ""
images = []
menu = ""
categories = ["tutorial"]
tags = ["tutorial"]
title = "Đọc trạng thái nút nhấn từ Raspberry PI 3"
slug = "button-python"
banner = "post/begin/button/banner.png"
+++

# Tổng quan
- Trong bài học này bạn sẽ học cách kết nối nút nhấn với chân GPIO trên Rapsberry PI, và xuất tín hiệu ra đèn LED. Đây là một ví dụ điển hình cho việc xử lý dữ liệu Digial Input cho Raspberry PI.

# Thực hiện
## Phần cứng
- Một Raspberry PI 3
- Một bộ nguồn 5V-2.5A (theo khuyến cáo của nhà sản xuất nên dùng nguồn lớn hơn 2A)
- Một thẻ nhớ đã cài hệ điều hành Raspbian và đã cài đặt wifi
- Một máy tính kết nối chung wifi với Raspberry
- Đèn LED
- Nút nhấn
- 2 Điện trở 470 Ohm
- Breadboard
- Dây cắm

## Giới thiệu phương pháp kết nối nút nhấn với Raspberry PI
- Trước khi tìm hiểu phương pháp kết nối nút nhấn với Raspberry, ta cần thông qua một khái niệm là "Điện trở kéo". Vậy điện trở kéo là gì?

![Hai phương pháp kết nối nút nhấn với Raspberry](/post/begin/button/01.jpg)

- Chúng ta có một chân GPIO được cấu hình là một đầu vào (Input). Nếu không kết nối với thiết bị khác, khi đọc trạng thái của chân này, rất khó để xác định chính xác trạng thái mà chân này đọc được. Để giải quyết vấn đề  trên, người ta dùng một điện trở kéo lên hay kéo xuống để xác lập một trong hai trạng thái là HIGH (mức cao) hay LOW (mức thấp) và đảm bảo dòng qua nó đủ nhỏ để không làm hỏng Raspberry.
- Trong bài viết này, ta chỉ đề cập đến điện trở kéo xuống (hình b), mọi thứ sẽ tương tự với điện trở kéo lên. Khác biệt ở đây là:
    - Điện trở kéo lên (hình a): điện trở nối với VCC của mạch, thường là 3.3V hay 5V
    - Điện trở kéo xuống (hình b): điện trở nối với GND
- Đối với điện trở kéo xuống, đầu vào của chân GPIO sẽ nhận trạng thái là LOW khi nút nhấn không được bấm. Khi nút nhấn được bấm, nó sẽ kết nối trực tiếp chân GPIO với VCC, vì thế đầu vào của chân GPIO sẽ nhận trạng thái là HIGH.
- Nên chọn giá trị điện trở kéo như thế nào? Thông thường, người ta thường chọn điện trở 10k để đảm bảo dòng điện đi qua là dòng điện đủ nhỏ để không làm hỏng Raspberry.

## Kết nối
- Trong bài này chúng ta sử dụng:
    - Phương pháp điện trở kéo xuống
    - Chân đọc tín hiệu đầu vào: Chân 12 trên board
    - Chân xuất tín hiệu ra LED: Chân 11 trên board

![Kết nối Button và LED với Raspberry PI 3](/post/begin/button/02.png)

## Giới thiệu về API trên python
- Tất cả các API đều đã được giới thiệu trong bài "Điều khiển nhấp nháy LED trên Raspberry PI 3 sử dụng Python"
- Để truy suất dữ liệu về GPIO, ta sử dụng module: `import RPi.GPIO as GPIO`
- Khai báo chân số 12 trên board làm ngõ vào:

    ```
    GPIO.setmode(GPIO.BOARD)
    GPIO.setup(12, GPIO.IN)
    ```
- Để đọc tín hiệu từ chân GPIO, sử dụng hàm ```GPIO.input([pin])```.Ví dụ: `GPIO.input(12)`

## Lập trình
- Sử dụng máy tính kết với với Raspberry thông qua terminal (Ubuntu) hay Putty (Windows)
- Khi đã hoàn thành kết nối, thực hiện lệnh: `sudo nano button.py` để tạo file button.py hoặc mở file button.py có sẵn
- Nhập vào đoạn chương trình:

```
import RPi.GPIO as GPIO
import time
ButtonPin = 12                        # pin 12
LedPin = 11                           # pin 11

GPIO.setmode(GPIO.BOARD)              # Numbers GPIOs by physical location
GPIO.setup(LedPin, GPIO.OUT)          # Set LedPin's mode is output
GPIO.output(LedPin, GPIO.LOW)         # Set LedPin high(+3.3V) to turn on led
GPIO.setup(ButtonPin, GPIO.IN)        # Set ButttonPin's mode is input

try:
  while True:
    input = GPIO.input(ButtonPin)     # Read state ButtonPin
    if (input == 1):
      GPIO.output(LedPin, GPIO.HIGH)  # led on
    else:
      GPIO.output(LedPin, GPIO.LOW)   # led off
except KeyboardInterrupt:             # When 'Ctrl+C' is pressed, the child program destroy() will be  executed.
  pass
  GPIO.output(LedPin, GPIO.LOW)       # led off
  GPIO.cleanup()                      # Release resource
```
- Lưu chương trình bằng cách nhấn Ctrl+X -> Y -> Enter
- Để chạy chương trình, thực hiện lệnh `sudo python button.py`. Nếu chương trình chạy đúng bạn có thể thấy LED sáng khi nhấn nút và tắt khi thả nút ra.

Trên đây là toàn bộ hướng dẫn giúp bạn giao tiếp với các thiết bị ngoại vi thông qua GPIO ở mode Digital Input của Raspberry PI với ví dụ điển hình là đọc trạng thái nút nhấn với phương pháp dùng điện trở kéo. Chúc các bạn thành công.
