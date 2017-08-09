
+++
date = "2017-08-01T23:47:03+07:00"
description = ""
images = []
menu = ""
categories = ["install"]
tags = ["install"]
title = "Cài đặt Raspbian cho Raspberry Pi 3 trên Window"
slug = "rasbian-cho-raspberry-tren-window"
banner = "post/begin/ubuntu-setup/banner.png"
+++

# Tổng quan
Tiếp tục chuỗi bài cài đặt Raspbian cho Raspberry PI trên các hệ điều hành. Bài viết này sẽ hưỡng dẫn bạn cài đặt Raspbian cho Raspberry PI trên hệ điều hành Window.

# Thực hiện

## Phần cứng
- Một Raspberry PI 3.
- Một bộ nguồn 5V-2.5A (theo khuyến cáo của nhà sản xuất nên dùng nguồn lớn hơn 2A).
- Thẻ nhớ: Từ 4GB trở lên.
- Cable mạng hoặc mạch chuyển đổi USB/UART.
- Một máy tính cài hệ điều hành Window có khả năng truy xuất vào Modem/Router.

## Một số phần mềm cần thiết khi cài đặt
- Phần mềm SD Formatter: Phần mềm dùng để format thẻ nhớ về đúng định dạng ban đầu.
https://www.sdcard.org/downloads/formatter_4/
- Phần mềm Win32 Diskimager: Phần mềm dùng để boot hệ điều hành vào thẻ nhớ.
https://sourceforge.net/projects/win32diskimager/
- Phần mềm putty: Phần mềm dùng để kết nối với Raspberry PI thông qua SSH/COM
http://www.putty.org/

-Lưu ý: Phần mềm SD Formatter và Win32 Diskimager là phần mềm cần phải cài đặt (quá trình cài đặt khá đơn giản). Còn phần mềm putty là application nên không cần cài đặt mà có thể dùng ngay.

## Cài đặt *Raspbian*
- Tải bản Raspbian mới nhất, nên dùng bản lite cho ứng dụng headless https://www.raspberrypi.org/downloads/raspbian/
- Giải nén file mới tải về
- Cắm thẻ nhớ vào, mở phần mềm SD Formatter lên và làm theo các bước sau:
    - B1: Chọn thẻ nhớ cần formmat.
    - B2: Vào "Option", tại thẻ "FORMAT TYPE" chọn "QUICK", thẻ "FORMAT SIZE" chọn "ON".
    - B3: Nhấn Format -> OK -> OK.
    - B4: Nhấn Exit.
- Mở phần mềm Win32 Diskimager, và thực hiện:
    - B1: Chọn đường dẫn đến bản Raspbian bạn vừa tải về
    - B2: Chọn thẻ nhớ để boot hệ điều hành
    - B3: Nhấn "Write" và đợi khoảng 10' để hoàn thành quá trình boot.
- Vậy là ta đã có một thẻ nhớ có thể khởi động được Raspbian.

## Kết nối Wifi và  SSH tới Raspberry PI
- Vì không thể thấy được 2 phân vùng của thẻ nhớ như trên Linux/Ubuntu hay OSX nên việc cài đặt wifi cho Raspberry PI cũng sẽ phức tạp hơn. Ở đây bạn có thể sử dụng 2 cách làm nhanh và đơn giản nhất:
C1: Nếu bạn đang ở gần modum/router, bạn dùng dây cable mạng kết nối raspberry với modun/router đó
C2: 
