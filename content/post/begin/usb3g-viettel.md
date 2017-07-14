+++
date = "2017-07-14T21:20:03+07:00"
description = ""
images = []
menu = ""
categories = ["install"]
tags = ["usb3g"]
title = "Raspberry Pi kết nối Internet sử dụng USB 3G Viettel - D6601"
slug = "rpi-ket-noi-internet-voi-usb3g-viettel"
banner = "post/begin/usb-3g/usb3g.jpg"
+++

Máy tính nhúng giá rẻ Raspberry Pi không còn xa lạ với các bạn thích công nghệ. Ngoài những ứng dụng dành cho giải trí đa phương tiện, Raspberry Pi rất thích hợp để phát triển các ứng dụng điều khiển từ xa, M2M. 

<!--more-->

![raspberry-pi][rpi-img]

Nội dung:
=========
Hướng dẫn nhanh dial-up 3G cho Raspberry Pi.

### Công cụ và các thao tác chuẩn bị
1. Máy tính nhúng Raspberry Pi cài đặt sẵn Raspbian (mình sử dụng model B+)
2. USB 3G Viettel - D6601, cắm trực tiếp vào 1 trong 4 cổng USB (đảm bảo nguồn cấp cho RPi > 1A)
3. Kết nối đến RPi sử dụng SSH, hay gõ trực tiếp từ bàn phím, miễn sao terminal được.
4. Cài đặt các công cụ cần thiết:
``` 
sudo apt-get update
sudo apt-get install ppp usb-modeswitch wvdial
```
### Thiết lập usbswitch mode
Bởi vì hầu hết các USB 3G hiện nay chạy 2 chế độ mass storage (lưu trữ) và modem. Khi lần đầu cắm vào máy tính mà chưa cấu hình gì thì hầu hết sẽ chạy chế độ Mass Storage để cung cấp driver cho người sử dụng. Chế độ này thì không thể dial-up, do vậy cần chuyển sang chế độ modem. 
Cách để biết vendor và product id khi nào ở chế độ Mass Storage, khi nào ở chế độ Modem thì khá nhiều bài viết trên `mạng`, chỉ cần `google`: `raspberry pi usb 3g` thì có khá nhiều bài viết bổ ích. Đối với bài này thì chỉ đích danh USB 3G của Viettel rồi nên không hướng dẫn các thao tác đó nữa.

Tiếp theo, sửa file: 
```
sudo nano /etc/usb_modeswitch.conf
```
Thêm nội dung sao vào:

```
DefaultVendor=0x19d2 
DefaultProduct=0x0154

TargetVendor=0x19d2 
TargetProduct=0x0108

MessageContent="5553424312345678000000000000061e000000000000000000000000000000"
MessageContent2="5553424312345679000000000000061b000000020000000000000000000000" 
MessageContent3="55534243123456702000000080000c85010101180101010101000000000000"
```

`Ctrl + X  ==> Y ` để lưu

Thực thi:
```
sudo usb_modeswitch -c /etc/usb_modeswitch.conf
```
để chuyển sang chế độ Modem

```
ls /dev/ttyUSB*
```
để kiểm tra xem có đủ ```ttyUSB0 ... ttyUSB3``` thì ok

### Thiết lập dial-up ppp
Tạo file
```
sudo nano /etc/wvdial.conf 
```
Thêm nội dung sau:
```
[Dialer 3gviettel] 
Init1 = ATZ 
Init2 = ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0 
Init3 = AT+CGDCONT=1,"IP","v-internet" 
Stupid Mode = 1 
Modem Type = Analog Modem 
ISDN = 0 
Phone = *99# 
Modem = /dev/ttyUSB3 
Username = { } 
Password = { } 
Baud = 460800
```

Thực thi:

```
wvdial 3gviettel
```

xem kết quả:

```
pi@raspberrypi /tmp $ sudo wvdial 3gconnect
--> WvDial: Internet dialer version 1.61
--> Initializing modem.
--> Sending: ATZ
ATZ
OK
--> Sending: ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0
ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0
OK
--> Sending: AT+CGDCONT=1,"IP","v-internet"
AT+CGDCONT=1,"IP","v-internet"
OK
--> Modem initialized.
--> Sending: ATDT*99#
--> Waiting for carrier.
ATDT*99#
CONNECT 21600000
--> Carrier detected.  Starting PPP immediately.
--> Starting pppd at Wed May  6 15:52:34 2015
--> Pid of pppd: 3304
--> Using interface ppp0
--> pppd: б?[01]X??[01]???[01]
--> pppd: б?[01]X??[01]???[01]
--> pppd: б?[01]X??[01]???[01]
--> pppd: б?[01]X??[01]???[01]
--> pppd: б?[01]X??[01]???[01]
--> pppd: б?[01]X??[01]???[01]
--> local  IP address 27.78.39.146
--> pppd: б?[01]X??[01]???[01]
--> remote IP address 10.64.64.64
--> pppd: б?[01]X??[01]???[01]
--> primary   DNS address 203.113.131.6
--> pppd: б?[01]X??[01]???[01]
--> secondary DNS address 203.113.131.2
--> pppd: б?[01]X??[01]???[01]
```

thì coi như Raspberry Pi của bạn đã kết nối internet được sử dụng 3G.

Nếu muốn tự động kết nối khi mất kết nối, dial khi khởi động, có thể tham khảo thêm: https://lawrencematthew.wordpress.com/2013/08/07/connect-raspberry-pi-to-a-3g-network-automatically-during-its-boot/

### Nguồn tham khảo
- http://raspberrypi.org
- https://www.thefanclub.co.za/how-to/how-setup-usb-3g-modem-raspberry-pi-using-usbmodeswitch-and-wvdial


[rpi-img]: https://www.raspberrypi.org/wp-content/uploads/2014/07/rsz_b-.jpg