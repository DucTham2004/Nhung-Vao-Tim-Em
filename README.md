## GIỚI THIỆU

__Đề bài__: SD card 

__Sản phẩm:__
1. Đọc ghi thẻ nhớ trên STM32 với hệ thống FAT file 
2. Sử dụng giao thức SPI để kết nối với SD card 
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
- Modun đọc thẻ micro SD card giao diện SPI
- Micro SD card 

## SƠ ĐỒ SCHEMATIC
|STM32F429|Module ngoại vi|
|--|--|
|PG13 (GPIO SD_CS)|CS|
|PA5 (SPI1 SCLK)|SCK|
|PA6 (SPI1 MOSI)|MOSI|
|PA7 (SPI1 MISO)|MISO|
|5V|VCC|
|GND|GND|

### TÍCH HỢP HỆ THỐNG
- Phần cứng
  
 |Thành phần|Vai trò|
 |--|--|
 |STM32F429ZIT6|Bo mạch chính, xử lý logic và giao tiếp với thẻ nhớ|
 |Thẻ SD|Là thiết bị lưu trữ được giao tiếp qua SPI để đọc/ghi file|
 |Modun đọc thẻ Micro SD|Chuyển tín hiệu SPI sang chuẩn tương thích với SD, thường có chân CS, CLK, MISO, MOSI|
 |UART/Serial tới PC|Giao tiếp nối tiếp từ STM32 tới máy tính|
 |Máy tính cá nhân (PC)|Thiết bị giám sát, điều khiển, ghi log|
 

- Phần mềm

 |Thành phần|Vai trò|
 |--|--|
 |STM32CubeIDE|Môi trường phát triển, biên dịch và nạp chương trình|
 |FATFS library|Quản lý truy xuất file hệ thống FAT trên thẻ SD|
 |USB Device CDC class|Giao tiếp USB CDC (COM ảo) qua thư viện USB Device|
 |UART Debug Logger|Gửi log qua UART1|
 |Hercules|Phần mềm trên PC đọc dữ liệu UART|

### ĐẶC TẢ HÀM

- Giải thích một số hàm quan trọng: ý nghĩa của hàm, tham số vào, ra

  ```C
     /**
     * @brief  Hàm ghi log ra UART với chuỗi định dạng như printf.
     * @param  fmt  Chuỗi định dạng (format string) giống printf, ví dụ: "Giá trị = %d\n"
     * @param  ...  Các đối số tương ứng với định dạng trong fmt
     */
      void myprintf(const char *fmt, ...);
  ```

  ```C
    /**
     * @brief  Gắn (mount) hệ thống tập tin FATFS lên thẻ nhớ.
     * @param  fs     Con trỏ đến cấu trúc FATFS để lưu thông tin hệ thống tập tin.
     * @param  path   Chuỗi đường dẫn ổ đĩa (ví dụ: "", "0:", ...).
     * @param  opt    Tùy chọn gắn (0 = không gắn ngay, 1 = gắn ngay).
     * @retval FRESULT  Mã lỗi hoặc thành công (FR_OK nếu thành công).
     */
    FRESULT f_mount(FATFS* fs, const TCHAR* path, BYTE opt);
  ```  
  ```C
    /**
     * @brief  Lấy thông tin về dung lượng trống của thẻ nhớ.
     * @param  path     Chuỗi đường dẫn ổ đĩa (ví dụ: "").
     * @param  nclst    Con trỏ để lưu số lượng cluster còn trống.
     * @param  fatfs    Con trỏ FATFS pointer để trả lại thông tin chi tiết hệ thống file.
     * @retval FRESULT  Mã lỗi hoặc thành công.
     */
    FRESULT f_getfree(const TCHAR* path, DWORD* nclst, FATFS** fatfs);
  ```
  ```C
    /**
     * @brief  Mở hoặc tạo một file.
     * @param  fp     Con trỏ đến đối tượng FILE để chứa thông tin file.
     * @param  path   Đường dẫn và tên file (ví dụ: "test.txt").
     * @param  mode   Cờ mở file: FA_READ, FA_WRITE, FA_CREATE_ALWAYS,...
     * @retval FRESULT  Mã lỗi hoặc thành công.
     */
    FRESULT f_open(FIL* fp, const TCHAR* path, BYTE mode);
  ```
  ```C
    /**
     * @brief  Đọc một dòng ký tự từ file văn bản.
     * @param  buff   Bộ đệm đích để lưu dòng đọc được.
     * @param  len    Số ký tự tối đa để đọc (bao gồm cả null-terminator).
     * @param  fp     File đang mở để đọc.
     * @retval TCHAR*  Con trỏ đến buff nếu thành công, NULL nếu lỗi.
     */
    TCHAR* f_gets(TCHAR* buff, int len, FIL* fp);
  ```
  ```C
    /**
     * @brief  Ghi dữ liệu vào file.
     * @param  fp         Con trỏ đến file đã được mở.
     * @param  buff       Bộ đệm chứa dữ liệu ghi.
     * @param  btw        Số byte dự định ghi.
     * @param  bw         Con trỏ để trả lại số byte đã ghi thực tế.
     * @retval FRESULT    Mã lỗi hoặc thành công.
     */
    FRESULT f_write(FIL* fp, const void* buff, UINT btw, UINT* bw);
  ```
  ```C
  /**
   * @brief  Đóng file đang mở.
   * @param  fp     Con trỏ đến file cần đóng.
   * @retval FRESULT  Mã lỗi hoặc thành công.
   */
  FRESULT f_close(FIL* fp);
  ```
  ```C
  /**
   * @brief  Gỡ hệ thống tập tin khỏi thẻ nhớ (unmount).
   * @param  fs     NULL để gỡ mount.
   * @param  path   Đường dẫn ổ đĩa (ví dụ: "").
   * @param  opt    Không dùng (thường để 0).
   * @retval FRESULT  Mã lỗi hoặc thành công.
   */
  FRESULT f_mount(FATFS* fs, const TCHAR* path, BYTE opt);
  ```

### KẾT QUẢ

- Các ảnh chụp với caption giải thích.
- Hoặc video sản phẩm
