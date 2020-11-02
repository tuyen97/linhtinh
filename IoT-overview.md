## Mục lục 
- [1. Tổng quan về IoT](#1)
  - [1.1. Tổng quan các thành phần](#1.1)
  - [1.2. Các loại thông tin](#1.2)
  - [1.3. Device metadata](#1.3)
  - [1.4. Thông tin trạng thái](#1.4)
  - [1.5. Telemetry](#1.5)

<a name="1"></a>
## Tổng quan về Internet of Things

IoT là 1 lĩnh vực phức tạp, không có 1 định nghĩa rõ ràng và đơn giản nào có thể bao quát hết. Một trong các ví dụ là việc sử dụng các thiết bị được kết nối mạng đặt trong môi trường vật lí, để có thể thay đổi cách xử lí 1 số công việc hoặc cho phép thực hiện các tình huống mà trước đó chưa thể thực hiện được.

<a name="1.1"></a>
### Tổng quan các thành phần 

1 hệ thống IoT có thể được chia làm 3 phần:

<i>Device</i> bao gồm các thiết bị phần cứng và phần mềm tương tác trực tiếp với thế giới bên ngoài. Device  kết nối tới 1 mạng để giao tiếp với nhau hoặc tới các ứng dụng trung tâm, có thể được kết nối trực tiếp hoặc gián tiếp tới internet.

<i>Gateway</i> cho phép device không kết nối trực tiếp với internet có thể giao tiếp với cloud service. Gateway có thể thay mặt cho 1 nhóm các device, xử lí các dữ liệu từ nhóm này gửi lên và truyền tới cloud.

Dữ liệu từ mỗi device được gửi tới <i>Cloud</i>, được xử lí và tổng hợp với các device khác, có thể với cả các dữ liệu thương mại khác.

<a name="1.2"></a>
### Các loại thông tin 

Mỗi thiết bị(device) có thể cung cấp và tiêu thụ rất nhiều loại thông tin. Mỗi dạng thông tin được thiết kế để các hệ thống xử lí tốt nhất.

<a name="1.3"></a>
### Device metadata

Metadata chứ thông tin về thiết bị. Hầu hết metadata là không hoặc ít khi thay đổi. Ví dụ về các thành phần của metadata bao gồm:

- Định danh(ID)- 1 định danh là duy nhất xác định 1 thiết bị. ID nên không đổi trong suốt vòng đời khi thiết bị được triển khai.

- Loại thiết bị

- Model

- Nhà sản xuất

- Số sê ri

<a name="1.4"></a>
### Thông tin trạng thái

Thông tin trạng thái mô tả trạng thái hiện tại của thiết bị(không phải trạng thái môi trường). Thông tin này có thể được đọc/ghi, được cập nhật nhưng thường là không thường xuyên.

<a name="1.5"></a>
### Telemetry

Thông tin thu thập được từ các thiết bị gọi là <i>telemetry</i>(phép đo từ xa). Đây là các dữ liệu chỉ đọc về môi trường, thường được thu thập thông qua các cảm biến.

### Devices


