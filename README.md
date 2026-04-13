# ESP32 AC Current Monitor PCB (Altium Designer)

Dự án này là thiết kế mạch đo dòng điện AC 220V sử dụng **ESP32-WROOM-32**, được thực hiện trên **Altium Designer**.

## 1. Mục tiêu dự án

Bo mạch được thiết kế để đáp ứng các yêu cầu:

- Đo dòng điện của tín hiệu **220V AC**.
- Cấu hình địa chỉ thiết bị để phân biệt nhiều mạch tương tự (tối đa **16 địa chỉ**).
- Hỗ trợ đo dòng tối đa theo 2 lựa chọn:
  - đến **5A**
  - đến **10A**
- Gửi dữ liệu về gateway qua:
  - **RS485** (UART ↔ RS485)
  - **Wi-Fi** hoặc **Bluetooth** (từ ESP32)
- (Tùy chọn) Hiển thị bằng LED 7 đoạn thông qua **IC 74HC595**.

## 2. Giải pháp phần cứng

Dựa trên yêu cầu hệ thống, thiết kế sử dụng các khối chính sau:

- **Cảm biến dòng TA12/TA17** để đo dòng AC:
  - TA12 cho dải đo tới 5A
  - TA17 cho dải đo tới 10A
- **ESP32-WROOM-32** làm vi điều khiển trung tâm, xử lý dữ liệu và truyền thông không dây.
- **Mạch chuyển mức UART sang RS485** để truyền dữ liệu có dây khoảng cách xa.
- **4 công tắc gạt (slide switch)** để đặt địa chỉ board (4-bit → tối đa 16 địa chỉ).
- **Mạch nguồn đầu vào 5V–36V** và **bộ ổn áp 3.3V** cấp cho ESP32.
- **LED trạng thái** và các **tụ lọc nhiễu** phục vụ vận hành ổn định.

## 3. Thành phần chính trong project Altium

Các file thiết kế chính trong repository:

- `PCB_ASSProject.PrjPcb` – file project Altium.
- `PCB1.PcbDoc` – layout PCB tổng.
- `ESP_32.SchDoc` – khối ESP32.
- `CURRENT_SENSOR.SchDoc` – khối cảm biến dòng.
- `RS485_PART.SchDoc` – khối truyền thông RS485.
- `3V3REGULATOR.SchDoc` – khối ổn áp 3.3V.
- `SW.SchDoc` – khối địa chỉ bằng switch.
- `LED.SchDoc` – khối LED hiển thị trạng thái.

## 4. Cấu trúc chức năng gợi ý

Luồng hoạt động điển hình:

1. Cảm biến TA12/TA17 lấy tín hiệu dòng AC.
2. ESP32 đọc và xử lý giá trị dòng.
3. Địa chỉ board được đọc từ 4 switch gạt.
4. Dữ liệu được truyền về gateway qua RS485 hoặc Wi-Fi/Bluetooth.
5. LED trạng thái (và tùy chọn 7 đoạn) hiển thị trạng thái vận hành.

## 5. Công cụ và cách mở dự án

### Yêu cầu

- Altium Designer (khuyến nghị phiên bản tương thích với định dạng `PrjPcb`/`SchDoc`/`PcbDoc`).

### Mở project

1. Mở Altium Designer.
2. Chọn **File → Open Project**.
3. Mở file `PCB_ASSProject.PrjPcb`.
4. Kiểm tra lần lượt các schematic sheet và PCB layout.

## 6. Ghi chú an toàn

- Mạch làm việc với tín hiệu liên quan đến **220V AC**, cần tuân thủ quy tắc an toàn điện trong quá trình thử nghiệm.
- Nên kiểm tra cách ly, khoảng cách creepage/clearance, và bảo vệ đầu vào trước khi sản xuất hoặc triển khai thực tế.

## 7. Hướng phát triển

- Bổ sung firmware ESP32 cho:
  - hiệu chuẩn cảm biến dòng,
  - đóng gói dữ liệu theo địa chỉ board,
  - truyền thông Modbus RTU (RS485) hoặc MQTT (Wi-Fi).
- Hoàn thiện phần hiển thị 7 đoạn dùng 74HC595 (nếu cần).
- Tối ưu EMC/EMI và bảo vệ nguồn cho môi trường công nghiệp.

---

Nếu bạn muốn, mình có thể viết thêm:

- README bản tiếng Anh,
- sơ đồ chân kết nối giữa các khối,
- checklist review thiết kế PCB trước khi xuất Gerber.
