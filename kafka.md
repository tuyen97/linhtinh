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

## Consumer

### Consumer group

1 topic gồm nhiều partition. Mỗi consumer nằm trong 1 consumer group, nhận thông điệp từ 1 tập các partition không giống nhau. 1 topic có thể có nhiều consumer group cùng consume.

### Rebalance

Khi 1 consumer trong group được thêm vào hoặc bỏ đi thì 1 consumer khác trong group sẽ thay thế nó xử lí các partition đang trống, đây gọi là rebalance. Trong quá trình rebalance, consumer sẽ không thể tiêu thụ được thông điệp .

consumer duy trì thông tin về các thành viên trong group và các partition ưngs với từng consumer thông qua <i>heartbeat</i> gửi tới 1 broker giữ vai trò là <i>group coordinator</i>.

Nếu consumer dừng gửi heartbeat đủ lâu, nó bị coi là dừng hoạt động và rebalance được thực hiện. Mất 1 khoảng thời gian để coordinator xác định là consumer đã chết hay chưa kể từ sau khi nó trễ gửi heartbeat. 

### Config

- <b>fetch.min.bytes</b>: nếu consumer request lấy dữ liệu từ broker nhưng các thông điệp mới có tổng kích cỡ nhỏ hơn giá trị tham số này, broker sẽ đợi thêm thông điệp để gửi cho consumer.

- <b>fetch.max.wait.ms</b>: với việc đặt <b>fetch.min.bytes</b> ta chỉ định cho kafka biết cần đợi cho đến khi dữ liệu đủ lớn mới gửi. Tham số này để điều khiển thời gian broker chờ cho đến khi đủ dữ liệu hoặc là gửi luôn. Ví dụ, nếu đặt fetch.min.bytes là 1 MB và fetch.max.wait.ms là 100ms thì khi nhận được yêu cầu từ consumer kafka sẽ đợi cho đến khi đủ 1 MB dữ liệu hoặc sau 100ms tùy theo cái gì diễn ra trước.

- <b>max.partition.fetch.bytes</b>: dung lượng dữ liệu tối đa mà broker trả về cho mỗi partition. Tham số này cần lớn hơn dung lượng thông điệp lớn nhất mà 1 broker chấp nhận.

- <b>session.timeout.ms</b>: thời gian tối đa giữa 2 heartbeat trước khi consumer bị coi là đã chết, mặc định là 3s. Nếu sau khoảng thời gian này mà consumer chưa gửi heartbeat nào tới broker thì nó bị coi là đã chết và rebalance được thực hiện. Tham số này có liên quan chặt chẽ tới <b>heartbeat.interval.ms</b>. <b>heartbeat.interval.ms</b> điều khiển khoảng thời gian giữa 2 lần consumer gửi heartbeat cho group coordinator. heartbeat.interval.ms phải nhỏ hơn session.timeout.ms và thường đặt bằng 1/3. Đặt session.timeout.ms nhỏ hơn mặc định giúp phát hiện lỗi sớm hơn và thực hiện khôi phục nhanh hơn nhưng có thể gây ra việc rebalance ngoài mong muốn vì consumer mất nhiều thời gian để thực hiện vòng lặpxử lí dữ liệu. Nếu đặt lớn thì việc rebalance xảy ra chậm hơn nên có thể làm cho việc phát hiện lỗi và phục hồi lâu hơn.

- <b>auto.offset.reset</b>: điều khiển hành vi của consumer khi nó bắt đầu đọc từ  1 partition chưa tưngf có commit offset nào hoặc offset là không hợp lệ(thường là do consumer dừng chạy quá lâu nên thông điệp ứng với offset đó đã bị hết hạn và bị broker xóa đi). Mặc định là latest, nghĩa là sẽ đọc từ thông điệp mới nhất kể từ khi consumer khởi động. Hoặc là earliest, nghĩa là consumer sẽ đọc bắt đầu từ bản ghi cũ nhất có thể.

- <b>enable.auto.commit</b>: điều khiển việc consumer sẽ tự động commit offset hoặc là người lập trình phải tự commit. Nếu đặt là true, thì có thể đặt khoảng thời gian để consumer tự động commit ofset bằng tham số <b>auto.commit.interval.ms</b>.

- <b>partition.assignment.strategy</b>: Cho các consumer và các topic chúng đã subscribe, xác định partition được gán cho từng consumer. Kafka có 2 chiến lược mặc định:

   - <i>Range</i>: chia cho mỗi consumer những phần liền nhau của từng topic, lần lượt từng topic. Nếu C1 và C2 cùng subscribe topic T1 T2, mỗi topic có 3 patition. C1 sẽ nhận được partition 0 và 1 của cả T1 và T2, C2 nhận được partition 2 cả 2 topic này.

   - <b>RoundRobin</b>: gộp tất cả các partition lại và chia mỗi partition lần lượt cho từng consumer. 

   Có thể sử dụng chiến lược riêng bằng cách trỏ tham số này tới class cài đặt việc phân chia.

- <b>max.poll.records</b>: Số bản ghi tối đã trong 1 lần poll

### Commit và offset

Kafka cho phép consumer tự quản lí thông tin về vị trí của thông điệp cuối cùng nó đã đọc.
 
Consumer commit offset b   ằng cách ghi 1 thông điệp vào 1 topic đặc biệt là <b>__consumer_offsets</b>, với mỗi offset hiện tại cho từng partition. 

#### Automatic Commit

Cách đơn giản nhất là để consumer commit offset tự động, bằng cách đặt <b>enable.auto.commit=true</b>, sau mỗi 5s consumer sẽ tự động commit offset lớn nhất mà client đã từng nhận được. Quãng thời gian này có thể thiết lập bằng tham số <b>auto.commit.interval.ms</b>. 

Với autocommit, 1 lời gọi hàm poll sẽ luôn commit offset cuối cùng trả về bởi lần poll trước đó. 

#### Commit offset hiện tại

Sử dụng hàm <b>commitSync()</b>, commit offset được trả về bởi hàm <b>poll()</b>, phát sinh ngoại lệ nếu xảy ra lỗi.

#### Asynchronous Commit

1 trong những nhược điểm của commit bằng tay là ứng dụng bị chặn dừng cho đến khi broker phản hồi cho commit request. Gây giảm thông lượng ứng dụng. 1 tùy chọn khác là sử dụng API commit bất đồng bộ.

Không giống cách thực hiện đồng bộ, hàm bất đồng bộ sẽ không retry gửi commit, việc gửi sẽ chỉ được thực hiện 1 lần. Có 1 tùy chọn để sử dụng callback được gọi khi broker phản hồi.

1 cách đơn giản là lưu lại offset đã commit vào 1 biến, khi hàm callback được gọi lúc broker phản hồi, kiểm tra xem offset trả về có bằng biến này hay không, nếu bằng thì chưa có commit mới nào thành công, việc retry cần được thực hiện, còn lại thì không.

#### Kết hợp cả 2 cách commit

Bình thường thì 1 vài lần commit không thành công mà không có gửi lại sẽ không thành vấn đề, tuy nhiên nếu nó là commit lần cuối trước khi đóng consumer hoặc là trước khi rebalance, thì cần phải chắc chắn rằng đã thành công.

Bằng cách sử dụng hàm bất đồng bộ trong try, còn hàm đồng bộ đặt trong finally.

#### Commit offset bất kì

ngoài commit offset cuối cùng ta còn có thể commit 1 offset bất kì nào đó. Commit API cho phép truyền 1 map partition-offset để gửi cho broker.

### Rebalance Listeners

Consumer API cho phép ta thực hiện 1 đoạn code khi partition được thêm hoặc bớt khỏi consumer. Thực hiện bằng cách truyền vào hàm <b>subsribe()</b> 1 đối tượng <b>ConsumerRebalanceListener</b>. Đối tượng này cần cài đặt 2 phương thức:

<b>public void onPartitionsRevoked(Collection<TopicPartition> partitions)</b>

   Được gọi trước khi rebalance bắt đầu và trước khi consumer dừng nhận thông điệp. Đây là chỗ cần commit offset để cho bất kì consumer nào sau đó biết bắt đầu từ đâu

<b>public void onPartitionsAssigned(Collection<TopicPartition> partitions)</b>

   Được gọi sau khi partition được gán cho broker, trước khi consumer bắt đầu consume thông điệp.

### Consume bản ghi với 1 offset bất kì

