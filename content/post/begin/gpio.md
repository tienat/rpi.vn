+++
date = "2017-08-13T23:47:03+07:00"
description = ""
images = []
menu = ""
categories = ["tutorial"]
tags = ["tutorial"]
title = "Điều khiển nhấp nháy LED trên Raspberry PI 3 sử dụng Python"
slug = "blink-led-python"
banner = "post/begin/gpio/banner.png"
+++

# Tổng quan
- Trong bài học này bạn sẽ học cách kết nối LED với chân GPIO trên Raspberry PI, và làm cho nó nhấp nháy sử dụng python. Trước khi vào bài học, ta cùng tìm hiểu GPIO trên Raspberry PI.
- GPIO là chân vật lý trên một mạch tích hợp (trong trường hợp của PI là BCM2837 SoC) có thể được sử dụng với mục đích làm ngõ vào hoặc ngõ ra và được kiểm soát bởi người dùng trong thời gian chạy. Bằng cách kết nối các cảm biển, thiết bị hiển thị và cơ cấu truyền động... Raspberry có thể giao tiếp với thế giới điện tử một cách dễ dàng.
- Cách đơn giản nhất bạn có thể quan sát sự hoạt động của GPIO trên Raspberry là nhấp nháy LED. Nó tương tự như chương trình Hello World thông thường mà bạn thường học khi bắt đầu một ngôn ngữ lập trình mới.
- Dưới đây là sơ đồ chân GPIO trên Raspberry PI 3:

![Sơ đồ chân GPIO trên Raspberry PI 3](/post/begin/gpio/01.png)
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
- Ta sử dụng chân số 9 và 11 như hình bên dưới. Để chắc chắc kết nối mạch chính xác, bạn cần lưu ý phân cực của LED. Có 3 cách nhận biết cực của LED:
    - Cách 1: dựa vào chân của LED, chân dài hơn thường được kết nối với nguồn áp cao, chân ngắn kết nối với nguồn áp thấp (ở đây là GND). --> thường được sử dụng
    - Cách 2: dựa vào bản cực bên trong LED, bản cực nhỏ - áp cao, bản cực lớn - áp thấp (GND)
    - Cách 3: dựa vào mảnh khuyết ở trên thân LED, không khuyết - áp cao, khuyết - áp thấp (GND)
- Như vậy ta sẽ kết nối chân dài vào điện trở để kết nối với chân 11, và chân ngắn kết nối với chân 9 là GND (như sơ đồ chân ở hình trên).

![Kết nối LED với Raspberry PI 3](/post/begin/gpio/02.png)

## Giới thiệu về API GPIO trên python
- Để truy suất dữ liệu về GPIO trên python ta sử dụng module ```RPi.GPIO```. Ta sẽ gọi module này ở đầu chương trình như sau:

```
    import RPi.GPIO as GPIO
```

- Có 2 cách khái báo chân cho Raspberry:
    - Cách 1: GPIO.BOARD: Khai báo theo số PIN của header (PIN1 - PIN40)
    `
    GPIO.setmode(GPIO.BOARD)
    `
    - Cách 2: GPIO.BCM: Khai báo theo số GPIO của IC (GPIO02 - GPIO27)
    `
    GPIO.setmode(GPIO.BCM)
    `
- Để cài đặt mode sử dụng cho GPIO, sử dụng hàm `setup([pin], [GPIO.IN, GPIO.OUT]`. Ví dụ cài đặt chân số 11 làm ngõ ra, và sử dụng khai báo `GPIO.BOARD`:

  ```
  GPIO.setmode(GPIO.BOARD)
  GPIO.setup(11, GPIO.OUT)
  ```
- Để xuất mức tín hiệu cao hay thấp, sử dụng hàm ```GPIO.output([pin], [GPIO.LOW, GPIO.HIGH])```:

  ```
  GPIO.output(11,GPIO.HIGH)
  ```
## Lập trình
- Trong bài học này ta sử dụng python 2.7 để lập trình. Đây là phần mềm có sẵn khi cài đặt Raspbian, nên ta không cần cài đặt lại nữa
- Sử dụng máy tính kết với với Raspberry thông qua terminal (Ubuntu) hay Putty (Windows)
- Khi đã hoàn thành kết nối, thực hiện lệnh: `sudo nano blink.py` để tạo file blink.py hoặc mở file blink.py có sẵn
- Nhập vào đoạn chương trình:

```
import RPi.GPIO as GPIO
import time
LedPin = 11                         # pin11

GPIO.setmode(GPIO.BOARD)            # Numbers GPIOs by physical location
GPIO.setup(LedPin, GPIO.OUT)        # Set LedPin's mode is output
GPIO.output(LedPin, GPIO.HIGH)      # Set LedPin high(+3.3V) to turn on led

try:
  while True:
    GPIO.output(LedPin, GPIO.HIGH)  # led on
    time.sleep(1)
    GPIO.output(LedPin, GPIO.LOW)   # led off
    time.sleep(1)
except KeyboardInterrupt:           # When 'Ctrl+C' is pressed, the child program destroy() will be  executed.
  pass
  GPIO.output(LedPin, GPIO.LOW)     # led off
  GPIO.cleanup()                    # Release resource
```
- Lưu chương trình bằng cách nhấn Ctrl+X -> Y -> Enter
- Để chạy chương trình, thực hiện lệnh `sudo python blink.py`. Nếu chương trình chạy đúng bạn có thể thấy LED chớp tắt với chu lỳ 1 giây.

Trên đây là toàn bộ hướng dẫn giúp bạn giao tiếp với các thiết bị ngoại vi thông qua GPIO ở mode Digital Ouput của Raspberry PI với ví dụ điển hình là chớp tắt LED. Chúc các bạn thành công.

# Tham Khảo:
- https://www.raspinews.com/blinking-led-on-raspberry-pi-using-python/
