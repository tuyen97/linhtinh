## RDD

RDD là đối tượng chứa dữ liệu trong spark. 

Trong spark mọi công việc đều thực hiện trên RDD: tạo mới RDD, thực hiện các hành động trên RDD để tính toán, biến đổi các RDD. Spark tự động phân tán dữ liệu trong RDD và song song hóa hành động thực hiện trên nó.

### Cơ bản về RDD

RDD là 1 tập các đối tượng được phân tán. Mỗi RDD được chia thành nhiều <i>partions</i> để tính toán trên nhiều node.

RDD phục vụ 2 loại hành động: <i>transformations</i> và <i>actions</i>:

- <i>Transformations</i> tạo dựng 1 RDD mới trên 1 RDD có sẵn. 

- <i>Actions</i> tính toán kết quả dựa trên 1 RDD hoặc lưu nó ra đĩa.

RDD có thể được khai báo nhưng chỉ được tính toán cho đến khi nó được sử dụng trong 1 action lần đầu tiên.

RDD mặc định được dựng lại mỗi khi thực hiện action trên nó. Có thể yêu cầu spark lưu lại RDD để sử dụng trong các lần tính toán tiếp theo bằng phương thức RDD.```persist```

### Tạo RDD

Có 2 cách tạo: nạp từ đĩa hoặc tạo từ các tập hợp(list,array...) trong chương trình.

### RDD Operations

Phân biệt transformation và action bằng kiểu trả về: transformation trả về RDD còn actions trả về 1 vài kiểu dữ liệu khác.

#### Transformation

Transform được thực hiện theo cách <i>lazily</i>, chỉ khi thực hiện action trên RDD nó trả về.

Transform không làm thay đổi RDD gốc, nó trả về con trỏ đến RDD hoàn toàn mới, RDD ban đầu vẫn có thể sử dụng để làm việc khác. VIệc này dẫn đến các RDD được phân cấp tổ tiên, Spark lưu lại thông tin này trong đồ thị họ hàng(lineage graph).

![](./img/spark_lineage_graph.png)

#### Actions

Mỗi lần thực hiện actions toàn bộ RDD mới bắt đầu được xây dựng, để tránh kém hiệu quả nên ghi kết quả trung gian ra đĩa.

#### Lazy Evaluation

Có thể coi mỗi RDD là các chỉ dẫn để có được dữ liệu, dữ liệu chỉ thực sự được nạp khi cần.


