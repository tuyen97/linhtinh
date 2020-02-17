## Cài đặt kafka

Kafka sử dụng Zookeeper để có thể hoạt động

Để join 1 cluster kafka thì broker phải có <b>broker.id</b> là duy nhất, và trỏ vào cùng 1 zookeeper ensemble.

Broker lưu dữ liệu cho từng partition liên tục thành từng segment. 1 segment đạt đến giới hạn(dung lượng hoặc thời gian tốn tại) sẽ được ngắt và dữ liệu tới được ghi ra segment mới. Segment cũ có thể được tính toán xem là hết hạn hay chưa (để xóa log quá hạn).

Topic có thể cài đặt số partition, thời gian lưu trữ dữ liệu cho 1 topic(thời gian hoặc là dung lượng dữ liệu đã ghi), thời gian lưu trữ cho từng segment log.

## Producer

Các thuộc tính cơ bản:

- <b>bootstrap.servers</b>: danh sách các broker để client khởi tạo kết nối.

- <b>serializers</b>: class sử dụng để serialize key và value

- <b>acks</b>: producer đợi bao nhiêu replica phản hồi trước khi ghi nhận việc ghi đã thành công.

- <b>buffer.memory</b>: kích thước bộ nhớ dùng để buffer message trước khi được gửi tới broker, nếu buffer đầy thì việc gửi sẽ bị block hoặc trả ra exception, được đặt bởi tham số <b>block.on.buffer.full</b> (block 1 khoảng thời gian trước khi trả ra ngoại lệ).

- <b>compression.type</b>: có thể đặt là ```snappy```, ```gzip``` hoặc ```lz4```. ```snappy``` sử dụng ít CPU nên sử dụng khi cả hiệu năng và băng thông cần được xem xét, ```gzip``` sử dụng nhiều CPU nhưng hiệu quả nén cao nên sử dụng khi yêu cầu băng thông thấp.

- <b>retries</b>: số lần producer thực hiện gửi lại thông điệp khi nhận được báo lỗi từ broker. Thời gian giữa 2 lần gửi lại có thể đặt thông qua tham số <b>retry.backoff.ms</b>.

- <b>batch.size</b>: khi các thông điệp được gửi đến cùng 1 partition, producer gom chúng lại thành các batch.Tham số này là  kích thước bộ nhớ(bytes) của mỗi batch, producer có thể thực hiện gửi batch khi mới chỉ đầy 1 nửa hoặc chỉ có 1 thông điệp trong batch nên sử dụng batch không gây trễ khi gửi thông điệp.

- <b>linger.ms</b>: producer sẽ gửi batch khi nó đầy hoặc sau linger.ms từ lần gửi cuối. Mặc định, producer sẽ gửi batch nếu có 1 sender thread đang rồi. Nếu đặt linger.ms lớn hơn 0, producer sẽ đợi vài ms trước khi gửi batch đi. Có thể làm tăng độ trễ nhưng cũng làm tăng thông lượng vì có nhiều thông được được gửi trong 1 lần hơn.

- <b>client.id</b>: bất kì chuỗi nào, broker định danh thông điệp gửi từ client.

- <b>max.in.flight.requests.per.connection</b>: số thông điệp được gửi trước khi nhận được bất kì phản hồi nào từ broker.

- <b>timeout.ms, request.timeout.ms, and metadata.fetch.timeout.ms</b>: điều khiển khoảng thời gian  producer đợi phản hồi từ server khi gửi dữ liệu (<b>request.timeout.ms</b>)

Thứ tự khi ghi:

- Kafka đảm bảo thứ tự ghi trong cùng 1 partition. 

- Bằng việc đặt <b>retries</b> khác 0 và <b>max.in.flights.requests.per.session</b> lớn hơn 1 có thể xảy ra trường hợp mà thông điệp đầu tiên fail còn thông điệp thứ 2 gửi thành công, sau đó thông điệp đầu tiên được gửi lại, do đó thứ tự giữa 2 thông điệp bị đảo lộn.

- Nếu thứ tự thông điệp là quan trọng thì có thể đặt <b>max.in.flights.requests.per.session</b> bằng 1 để đảm bảo rằng trong khi 1 thông điệp được gửi lại thì không có thêm thông điệp nào khác được gửi nữa.

### Serializer

Registry: lưu trữ toàn bộ các schema trong registry(1 url), thông điệp được gắn kèm với id của schema. Consumer có thể dựa vào id gắn kèm trong thông điệp để deserialize dữ liệu.
