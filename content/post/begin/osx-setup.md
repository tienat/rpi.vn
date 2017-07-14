+++
date = "2017-05-20T12:10:51+02:00"
description = ""
images = []
menu = ""
categories = ["install"]
tags = ["install"]
title = "Môi trường phát triển ứng dụng cho Raspberry Pi trên OS X"
banner = "post/begin/osx-setup/banner.jpg"
+++

# Tổng quan
Hiện tại Raspberry PI đã ra phiên bản 3, với nhiều tính năng vượt trội, đặc biệt là đã tích hợp Wifi + BLE trong khi giá vẫn không đổi. Chính sự thay đổi này đã làm giá của RPI giảm đi rất nhiều, bởi vì trước đây việc sử dụng RPI bắt buộc phải mua thêm 1 cái USB Wifi là khó tránh khỏi.

Nhưng, việc xây dựng môi trường lập trình ứng dụng trên RPI không phải là đơn giản, nếu bạn không có 1 cái màn hình hỗ trợ HDMI và không có luôn 1 cái cổng Mạng (Macbook giờ mất luôn cái cổng huyền thoại này :cry:) thì đảm bảo là không thể luôn.

Bài viết này sẽ giúp bạn dễ dàng để xây dựng một môi trường lập trình hoàn hảo với 1 máy tính kết nối Wifi và 1 con RPI hỗ trợ Wifi.  __Không cần Monitor HDMI, không cần cáp mạng__

<!--more-->

![alt text](/post/begin/osx-setup/01.png)


# Thực hiện
## Phần cứng
- Một Raspberry PI, nếu trước phiên bản 3 thì cần mua 1 USB Wifi, theo kinh nghiệm thì chỉ cần mua loại thông dụng thì RPI sẽ tự nhận (TPLink, TotoLink ...)
- Nguồn thì cắm trực tiếp vào máy tính là đủ, nếu không thì dùng Adapter sạc điện thoại, hoặc mua adapter rời
- Đảm bảo là có quyền truy cập vào Modem & Router (nếu có)
- Một máy tính kết nối trực tiếp đến Modem/Router mà RPI sẽ kết nối.
- Thẻ nhớ: 8Gb tốt nhất

## Cài đặt hỗ trợ đọc ghi Ext2/3 cho OSX

Bởi vì RPI sử dụng định dạng Ext3/4, nên phải đảm bảo máy Mac cũng đọc/ghi được định dạng này
- Cài đặt [Fuse for OS X](http://osxfuse.github.io/)
- Cài đặt [fuse-ext2](http://sourceforge.net/projects/fuse-ext2/)
- Khi cài đặt xong chỉ hỗ trợ read-only, cần thay đổi chế độ đọc/ghi
    - Edit file: `/System/Library/Filesystems/fuse-ext2.fs/fuse-ext2.util`
    - Tìm tính năng `Mount`
    - Sửa dòng có `OPTIONS="auto_xattr,defer_permissions"` bằng `OPTIONS="auto_xattr,defer_permissions,rw+"`

## Cài đặt *Raspbian*
- Tài bản Raspbian mới nhất, nên dùng bản lite cho ứng dụng headless https://www.raspberrypi.org/downloads/raspbian/ 
- Cắm thẻ nhớ vào, mở command thực hiện lệnh `diskutil list`
- Xác định tên disk khi cắm thẻ nhớ vào, không chắc thì rút ra, thực hiện `diskutil list` rồi cắm lại thực hiện lại lệnh trên. Ở đây ví dụ `disk3`, rồi thực hiện các lệnh:
    - `diskutil unmountDisk /dev/disk3`
    - `sudo newfs_msdos -F 16 /dev/disk3`
    - `sudo dd if=~/Desktop/rpi.dmg of=/dev/rdisk3 bs=1m`
- Lưu ý: `~/Desktop/rpi.dmg` là đường dẫn tới file ảnh Raspbian tải về, `rdisk3` và `bs=1m` direct access sẽ giúp việc ghi nhanh hơn gấp nhiều lần. Muốn xem progress thực hiện thì nhấn tổ hợp `CTRL + T`
- Sau khi thực hiện xong thì chúng ta có thẻ nhớ có thể khởi động được Raspbian.

## Kết nối Wifi và  SSH tới Raspberry PI
- Rút thẻ nhớ vừa khởi tạo ra, cắm lại. Lúc này trên máy Mac sẽ mount 2 phân vùng, phân vùng boot và phân vùng root filesystem.
- Truy cập tới phân vùng root filesystem, sửa file `/etc/wpa-supplicant/wpa-supplicant.conf` với nội dung:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
  ssid="your ssid"
  psk="your pass"
}
```
- Cắm thẻ nhớ vào RPI và cho khởi động
- Đăng nhập vào trang cấu hình của Modem (ví dụ: http://192.168.1.1)
- Hầu hết tất cả các modem sẽ hiển thị DHCP Client list, tìm cái nào có host name là raspberrypi, lấy IP đó, ví dụ 192.168.1.123
- Mở terminal lên, ssh vào: `ssh pi@192.168.1.123` và gõ mật khẩu: `raspberry`
- Thực hiện lệnh `passwd` để thay đổi mật khẩu (option)

## Cấu hình forward port và ddns cho RPI
- Đăng ký tài khoản DDNS ở [noip](http://www.noip.com/), ví dụ: `myrpi.ddns.net`
- Cài đặt cập nhật IP động cho modem, cập nhật tới url của DDNS, tuỳ từng loại modem, kiếm service nào tên DDNS, điền thông tin sử dụng dịch vụ noip. Điều này giúp domain `myrpi.ddns.net` luôn được cập nhật IP internet của modem nếu modem bị thay đổi IP
- Cầu hình forward port 22 tới địa chỉ IP của RPI, giúp tất cả các kết nối từ bên ngoài vào cổng 22 sẽ được forward trực tiếp tới RPI (đề SSH từ bên ngoài vào) - Việc cấu hình này cũng tuỳ từng loại modem, tuy nhiên kiếm dịch vụ Forward Port hay Virtual Server, hay đơn giản nhất là DMZ, forward tất cả dữ liệu tới IP nội bộ.
- Kiểm tra port đã open chưa ở : http://www.canyouseeme.org 

## Develop app từ xa over SSH
- Đăng ký tài khoản tại https://codetasty.com
- Setup workspace như hình
![alt text](/post/begin/osx-setup/02.png)
- Sau khi kết nối thành công bạn có thể dễ dàng develop app trên board RPI ở bất kỳ đâu, các ngôn ngữ hỗ trợ rất mạnh là C/C++, Python, Nodejs
![alt text](/post/begin/osx-setup/03.png)

# Một số lưu ý
- Vài viết có ít thông tin về cấu hình forward port cũng như setup DDNS do trên thị trường có rất nhiều router, modem, không thể cụ thể hoá
- Nếu bạn có Router kết nối tới Modem, thì phải setup IP tĩnh của Router trong mạng của Modem, rồi DMZ modem sang Router, rồi setup forward port  từ Router tới RPI
- Trong hầu hết các Modem và Router sẽ cho phép setup `static DHCP assign`, bạn chỉ cần copy MAC của RPI và, và điền IP muốn assign, thì không cần phải setup IP tĩnh trên RPI, mỗi khi RPI xin IP qua DHCP sẽ được cấp đúng IP đó.

# Tham khảo:
- http://blog.darrenparkinson.uk/2013/10/accessing-raspberry-pi-filesystem-from.html