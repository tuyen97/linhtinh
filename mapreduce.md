## MapReduce 

- MapReduce hoạt động bằng cách chia tính toán làm 2 pha: pha map và pha reduce.

- Mỗi pha nhận các cặp key-value làm đầu vào và đầu ra do người lập trình chỉ định. Người lập trình cũng viết 2 hàm: hàm map và hàm reduce.

- 1 chương trình Map-Reduce cần 1 hàm map, 1 hàm reduce và các thủ tục để chạy 2 hàm này.

### Map

- Lớp map cài đặt interface ```Mapper``` với 4 tham số chỉ ra kiểu của input key, input value, output key, output value.

- Phương thức ```map()``` được truyền 1 cặp key-value và 1 đối tượng ```OutputCollector``` để ghi output.

### Reduce

- Tương tự như lớp map

### Các thủ tục để thực hiện Map-Reduce

---
- job

- task

- jobtracker

- tasktracker

- input split

- record

- Map task ghi output ra local disk, nếu có lỗi Hadoop tự động chạy map trên node khác.

- ```combiner function``` nhận đầu vào là output của map function, đưa đầu ra cho input của reduce function. Giúp làm giảm lượng dữ liệu truyền giữa map và reduce.

## HDFS

Được thiết kế hoạt động trên hệ thống:

- Very large file

- Streaming data access

- Phần cứng thông thường

Hoạt động không tốt nếu:

- Low-latency data access

- Rất nhiều file nhỏ

- Không hỗ trợ nhiều người dùng cùng ghi 1 file, không hỗ trợ chỉnh sửa file theo offset.

### HDFS Concepts

#### Blocks

- File trong HDFS được chia thành các block có kích thước cố định - mặc định là 64MB nhưng nếu file nhỏ hơn kích thước 1 block nó sẽ không chiếm toàn bộ 1 block.

- Kích thước block lớn để tối thiểu hóa chi phí dịch chuyển đầu đọc. Thời gian đọc block từ đĩa sẽ lớn hơn đáng kể so với thời gian định vị đầu đọc. Do đó thời gian truyền 1 file lớn tạo thành từ nhiều block sẽ chỉ phụ thuộc vào tốc độ đọc file từ đĩa cứng.

- Lợi ích:

   - Kích thước file lưu trữ có thể lớn hơn kích thước bất kì ổ đĩa cứng nào trên mạng.

   - Nhân bản để chịu lỗi, tăng tính khả dụng.

#### Namenode và Datanode

- 1 cluster HDFS hoạt động theo chế độ master-worker(namenode và datanode).

- Namenode quản lí hệ thống file, lưu trữ metadata, cấu trúc cây thư mục của toàn bộ hệ thống dưới dạng namespace image và edit log  trên đĩa cứng của nó. Ngoài ra, namenode còn giữ thông tin về vị trí block của các file trên datanode, thông tin này liên tục được xây dựng lại từ các datanode.

- Datanode là nơi lưu trữ thực sự của dữ liệu, nó thực hiện lưu trữ và lấy dữ liệu theo yêu cầu từ namenode hoặc client, báo cáo lại định kì cho namenode danh sách các block mà nó đang giữ.

- Để tránh việc namenode chết gây hỏng toàn bộ hệ thống, Hadoop cung cấp các giải pháp:

   - namenode có thể ghi metadata của nó sang nhiều nơi khác, không cùng 1 máy.
  
   - Sử dụng <i>secondary namenode</i>, có nhiệm vụ hợp namespace image với edit log để phòng khi có lỗi lấy ra sử dụng luôn.

### Data Flow

#### Đọc 

- Client giao tiếp trực tiếp với datanode để lấy dữ liệu theo chỉ dẫn của namenode để tìm được datanode tốt nhất cho từng block. Báo cáo lại tình trạng của datanode nếu có lỗi trong truyền thông hoặc dữ liệu bị lỗi. Namenode chỉ làm nhiệm vụ đáp ứng yêu cầu vị trí các block(được lưu trữ trong bộ nhớ chính) đảm bảo tốc độ và tránh tắc nghẽn.

![](./img/hadoop_read.png)

#### Network Topology

- Hadoop coi toàn bộ mạng là 1 cây có gốc, khoảng cách giữa 2 nút mạng tính bằng tổng khoảng cách từ chúng đến tổ tiên chung gần nhất.

- Các level có thể không được định nghĩa rõ ràng tuy nhiên khoảng cách trong các trường hợp sau được coi là tăng dần:

   - Các tiến trình trên cùng 1 node = 0

   - Node trên cùng 1 rack = 2

   - Node khác rack nhưng cùng 1 data center = 4

   - Node khác data center = 6

![](./img/hadoop_network_topology.png)

#### Ghi


