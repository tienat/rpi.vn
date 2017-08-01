+++
date = "2017-08-01T23:47:03+07:00"
description = ""
images = []
menu = ""
categories = ["install"]
tags = ["install"]
title = "Cài đặt Rasbian cho Raspberry Pi 3 trên Linux/Ubuntu"
slug = "rasbian-cho-raspberry-tren-linux"
banner = "post/begin/ubuntu-setup/banner.png"
+++

# Tổng quan
Như đã giới thiệu ở bài viết "Môi trường phát triển ứng dụng cho Raspberry Pi trên OS X", Raspberry Pi 3 có nhiều tính năng vượt trội, tích hợp sẵn wifi, BLE... Chính vì thế, việc lựa chọn Raspberry Pi 3 để sử dụng là hợp lý nhất so với những phiên bản cũ mà giá thành không chênh lệnh nhau quá lớn. Tuy nhiên việc người dùng sử dụng quá nhiều hệ điều hành khác nhau đưa đến việc cài đặt hệ điều hành cho Raspberry Pi cũng khác nhau.

Bài viết này sẽ giúp bạn dễ dàng cài đặt hệ điều hành cho Raspberry Pi (ở đây sử dụng Raspbian) một cách đơn giản với Linux/Ubuntu.

# Thực hiện
## Phần cứng
- Một Raspberry PI 3
- Một bộ nguồn 5V-2.5A (theo khuyến cáo của nhà sản xuất nên dùng nguồn lớn hơn 2A)
- Thẻ nhớ: Từ 4GB trở lên
- Một máy tính cài hệ điều hành Linux/Ubuntu có khả năng truy xuất vào Modem/Router

## Cài đặt *Raspbian*
- Tải bản Raspbian mới nhất, nên dùng bản lite cho ứng dụng headless https://www.raspberrypi.org/downloads/raspbian/
- Giải nén file mới tải về, ở đây file tải về nằm trong Downloads
- Cắm thẻ nhớ vào, mở terminal lên và thực hiện lệnh `sudo fdisk -l`
- Xác định tên disk khi cắm thẻ nhớ, thông thường là /dev/mmcblk0
- Tiếp tục thực hiện các lệnh:
    - `cd Downloads/`
    - `sudo dd bs=4M if=2017-07-05-raspbian-jessie-lite.img of=/dev/mmcblk0`
- Lưu ý: Lệnh `cd Downloads/` dùng để đi đến thư mục chứa file cài đặt
- Sau khi thực hiện xong thì chúng ta có thẻ nhớ có thể khởi động được Raspbian.

## Kết nối Wifi và  SSH tới Raspberry PI
- Rút thẻ nhớ vừa khởi tạo ra, cắm lại. Lúc này trên máy Linux/Ubuntu sẽ mount 2 phân vùng, phân vùng boot và phân vùng root filesystem.
- Truy cập tới phân vùng root filesystem, truy cập vào thư mục `/etc/wpa-supplicant/`. Để sửa file `wpa-supplicant.conf` trong thư mục này, ta thực hiện như sau:
    - Nhấp chuột phải vào vùng trắng của thư mục này, chọn Properties
    - Copy địa chỉ của thư mục tại dòng Location:
    - Mở terminal và thực hiện lệnh `sudo nano đường dẫn/wpa-supplicant.conf`
    - Sửa file với nội dung như sau:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
  ssid="your ssid"
  psk="your pass"
}
```
- Cắm thẻ nhớ vào RPI và chờ khởi động trong khoảng 1 phút
- Đăng nhập vào trang cấu hình của Modem (ví dụ: http://192.168.1.1)
- Hầu hết tất cả các modem sẽ hiển thị DHCP Client list, tìm cái nào có host name là raspberrypi, lấy IP đó, ví dụ 192.168.1.12
- Mở terminal lên, ssh vào: `ssh pi@192.168.1.12` và gõ mật khẩu: `raspberry`
- Thực hiện lệnh `passwd` để thay đổi mật khẩu (option)
- Vậy là đã hoàn thành quá trình cài đặt Raspbian cho Raspberry Pi thông qua Linux/Ubuntu. Chúc các bạn thành công.
