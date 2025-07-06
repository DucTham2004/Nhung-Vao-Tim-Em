## GIỚI THIỆU

__Đề bài__: SD card 

__Sản phẩm:__
1. Đọc ghi thẻ nhớ trên STM32 với hệ thống FAT file 
2. Sử dụng giao thức SPI để kết nối với SD card 
3. Tính năng
- Ảnh chụp minh họa:\
  ![Ảnh minh họa](https://soict.hust.edu.vn/wp-content/uploads/logo-soict-hust-1-1024x416.png)

## TÁC GIẢ

- Tên nhóm: Nhúng vào tim em
- Thành viên trong nhóm
  |STT|Họ tên|MSSV|Công việc|
  |--:|--|--|--|
  |1|Nguyễn Đức Thắm|20225395||
  |2|Bùi Chí Thức|20225230||
  |3|Nguyễn Quốc Anh|20225252||


## MÔI TRƯỜNG HOẠT ĐỘNG

- STM32F429ZIT6
- Bộ kit STM32F429ZIT6
- Đầu đọc thẻ SD card
- Micro SD card 

## SO ĐỒ SCHEMATIC
|STM32F429|Module ngoại vi|
|--|--|
|PB1 (GPIO SD_CS)|CS|
|PB13 (SPI2 SCLK)|SCK|
|PB15 (SPI2 MOSI)|MOSI|
|PB14 (SPI2 MISO)|MISO|
|5V|VCC|
|GND|GND|

### TÍCH HỢP HỆ THỐNG
- Phần cứng
  
 |Thành phần|Vai trò|
 |--:|--|
 |STM32F429ZIT6|Bo mạch chính, xử lý logic và giao tiếp với thẻ nhớ|

- Mô tả các thành phần phần cứng và vai trò của chúng: máy chủ, máy trạm, thiết bị IoT, MQTT Server, module cảm biến IoT...
- Mô tả các thành phần phần mềm và vai trò của chúng, vị trí nằm trên phần cứng nào: Front-end, Back-end, Worker, Middleware...

### ĐẶC TẢ HÀM

- Giải thích một số hàm quan trọng: ý nghĩa của hàm, tham số vào, ra

  ```C
     /**
      *  Hàm tính ...
      *  @param  x  Tham số
      *  @param  y  Tham số
      */
     void abc(int x, int y = 2);
  ```
  
### KẾT QUẢ

- Các ảnh chụp với caption giải thích.
- Hoặc video sản phẩm
