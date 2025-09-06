# Hướng dẫn chi tiết mô phỏng đèn LED sáng đều bằng vi điều khiển STM32F103C8T6 trên phần mềm Proteus và lập trình bằng phần mềm STM32CubeIDE.

## Các công đoạn chính
1. Thiết kế mạch bằng Proteus
2. Lập trình trong STM32CubeIDE
3. Nạp code vào Proteus để mô phỏng

---

## Công đoạn 1: Thiết kế mạch bằng Proteus

### 1.1 Tạo một project trong Proteus
1. Mở phần mềm Proteus.
2. Từ trang chủ, chọn **File** (góc trên bên trái) -> **New Project** để tạo dự án mới.
3. Trong cửa sổ **New Project Wizard**:
   - Đặt tên cho dự án và chọn đường dẫn lưu tùy ý.
   - Nhấn **Next** qua các bước và nhấn **Finish** để hoàn tất.
   Lưu ý: Trong khi tạo, nếu thêm một folder mới để lưu project thì phải đảm bảo trong folder đó có một folder tên Project     Backup, đây là tính năng của Proteus.
  <img width="775" height="349" alt="image" src="https://github.com/user-attachments/assets/dedb75f6-633e-4b83-9f74-02e4efb35ebc" />
  <img width="775" height="348" alt="image" src="https://github.com/user-attachments/assets/4803fb43-0a94-495a-a8b4-bc2dc2fc7bbd" />



### 1.2 Thêm các linh kiện cần thiết
1. Nhấp vào biểu tượng chữ **P** (Pick Devices) để thêm linh kiện.
   <img width="775" height="748" alt="image" src="https://github.com/user-attachments/assets/e668c031-d9a5-4458-b96f-89d9449ac017" />

3. Trong ô **Keywords**, nhập và chọn lần lượt các linh kiện:
   - **STM32F103C8**: Vi điều khiển chính.
   - **LED-RED**: Đèn LED màu đỏ.
   - **Resistor**: Điện trở.
   - **Vsource**: Nguồn điện.
4. Để thêm **GROUND**:
   - Nhấp chuột phải vào bản vẽ -> Chọn **Place** -> **Terminal** -> **Ground**.
5. Để quay lại danh sách linh kiện, chọn **Component Mode** (biểu tượng linh kiện) ở thanh công cụ bên trái.
6. **Lưu ý**: Nếu thư viện STM32F103C8T6 chưa có, cần tải và thêm thư viện vào Proteus trước khi sử dụng.
    Link hướng dẫn tải 1: https://www.youtube.com/watch?v=hWY_ZKpwD1A&list=PLCN0CoF7Jcpq9uhtHErKNUFY88M0kK6qU&index=5
    Link hướng dẫn tải 2: Chưa cập nhật
### 1.3 Thiết kế mạch
Sơ đồ kết nối mạch:
- Kết nối chân **PA5** của STM32F103C8T6 với **cực dương** của LED (phần không có dấu gạch ngang).
- **Cực âm** của LED nối với điện trở **330Ω**, sau đó nối tới **GND**.
- **VDDA** và **VBAT** nối với **cực dương** của nguồn điện (**Vsource**).
- **VSSA** nối với **cực âm** của nguồn điện (**GND**).
- **BOOT0** nối qua điện trở **10 kΩ** tới **GND**.
- **Lưu ý** vi điều khiển đang sử dụng đã nối sẳn VDD và VSS
<img width="752" height="437" alt="image" src="https://github.com/user-attachments/assets/3a48ae9f-52c2-4fb6-b1cc-8fb0a3382692" />

---

## Công đoạn 2: Lập trình trong STM32CubeIDE

### 2.1 Tạo một project
1. Mở STM32CubeIDE.
2. Từ trang chủ, vào **File** -> **New** -> **STM32 Project**.
3. Trong cửa sổ mới:
   - Tại ô **Commercial Part Number**, nhập **STM32F103C8T6**.
   - Chọn **STM32F103C8T6** trong danh sách **MCUs/MPUs List** -> Nhấn **Next**.
   - Đặt tên cho project và chọn đường dẫn lưu -> Nhấn **Finish**.

### 2.2 Cấu hình chân PA5
1. Trong giao diện cấu hình, thiết lập chân **PA5** là **GPIO_Output**.
2. Lưu cấu hình bằng phím tắt **Ctrl + S**. File **main.c** sẽ được tạo tự động.

### 2.3 Viết code
1. Trong file **main.c**, tìm hàm `int main(void)` với nội dung như sau:
   ```c
   int main(void)
   {
       HAL_Init();              // Khởi tạo HAL
       SystemClock_Config();    // Cấu hình clock
       MX_GPIO_Init();          // Cấu hình GPIO (bao gồm PA5)
       while (1)
       {
           /* USER CODE BEGIN WHILE */
           // Vùng để viết code chính
           /* USER CODE END WHILE */
       }
   }
   ```
2. Thêm dòng code sau vào vùng **USER CODE BEGIN WHILE** để điều khiển LED sáng:
   ```c
   HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);
   ```
3. Lưu file bằng **Ctrl + S**.

### 2.4 Build project
1. Nhấn **Ctrl + B** để build project. Nếu thành công, thông báo **"Build finished"** sẽ xuất hiện.
2. Kiểm tra file **.hex**:
   - Trong thanh thư mục bên trái, vào thư mục dự án -> thư mục **Debug**.
   - Tìm file **<tên dự án>.hex**.
     
     <img width="136" height="296" alt="image" src="https://github.com/user-attachments/assets/4fcd7ba2-4143-4cdc-a011-9eaa752defed" />

3. Nếu không thấy file **.hex**:
   - Chuột phải vào tên dự án (ví dụ: **LEDlight**) -> Chọn **Properties**.
   - Vào **C/C++ Build** -> **Settings** -> **Tool Settings** -> **MCU Post build outputs**.
   - Tích chọn **Convert to Intel Hex file (-O ihex)** -> **Apply and Close**.
   - Nhấn **Ctrl + B** để build lại. File **.hex** sẽ xuất hiện.

     <img width="142" height="329" alt="image" src="https://github.com/user-attachments/assets/f504ee8e-e71c-4022-9abd-b8f7dfabd155" />


### 2.5 Sao chép đường dẫn file .hex
1. Trong thanh thư mục, chuột phải vào file **.hex** -> Chọn **Properties** -> **Resource**.
2. Tại phần **Location**, sao chép đường dẫn của file **.hex**.

---

## Công đoạn 3: Nạp code vào Proteus

1. Mở file Proteus đã thiết kế ở Công đoạn 1.
2. Nhấp đúp chuột vào vi điều khiển **STM32F103C8T6**.
3. Trong cửa sổ hiện ra, tại ô **Program File**, dán đường dẫn của file **.hex** đã sao chép.
4. Nhấn **OK** để lưu.
5. Nhấn nút **Run** (biểu tượng tam giác) ở thanh công cụ dưới cùng để chạy mô phỏng.
6. Kết quả: LED sẽ sáng đều trong mô phỏng Proteus.
Kết quả như sau:
<img width="975" height="549" alt="image" src="https://github.com/user-attachments/assets/1baaf968-ebf6-419a-b07e-a1cc2a28fc2c" />

---

## Lưu ý
- Đảm bảo các linh kiện trong Proteus được kết nối chính xác.
- Kiểm tra kỹ đường dẫn file **.hex** khi nạp vào Proteus.
- Nếu gặp lỗi, kiểm tra lại cấu hình GPIO trong STM32CubeIDE hoặc kết nối mạch trong Proteus.
----------------------------------------------------------------------------------------------
Ngày thực hiện: 27/8/2025

© 2025 Huỳnh Tử Khiêm. All rights reserved.
