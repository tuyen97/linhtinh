### Sử dụng socket
Có 2 cách để giao tiếp giữa các socket: ```send```-```receive``` và ```read```-```write```.

```read```-```write``` sử dụng các file để truyền tin, đây là cách Java sử dụng, cần chú ý ```flush``` khi truyền tin. Các file được lưu trữ trên buffer, nếu không được ```flush``` thì nó vẫn mãi mãi không truyền đi được.

```send```-```receive``` hoạt động trên network buffers. Mỗi socket có 1 vùng buffer nhận dữ liệu và buffer ghi dữ liệu.

```send``` sẽ ghi dữ liệu vào buffer, việc chuyển dữ liệu do card mạng thực hiện, dữ liệu được ghi vào buffer của máy nhận. ```receive``` đọc dữ liệu trên buffer.

Khi ```recv``` return 0 có nghĩa là đầu bên kia đã ngắt kết nối, từ lúc này trở đi không thể nhận thêm dữ liệu mặc dù có thể gửi. Chú ý là với 
```recv(buffsize)```, lệnh này chỉ đọc nhiều nhất là buffsize trong 1 lần từ buffer, nên cần phải lặp lại việc đọc và xác định độ dài của message của thông điệp để biết khi nào dừng đọc.

### Xác định ngắt kết nối

Khi 1 bên ngắt kết nối do phần mềm( Ctrl-C, exception...) có thể có socket.close/shutdown hoặc do bộ dọn rác thu dọn thì việc đọc từ bên còn lại sẽ thu được 1 packet có length = 0, việc ghi sẽ được phản hồi bằng ```SIGPIPE``` biểu hiện là 1 exception.

Còn việc ngắt kết nối do mất mạng, router hỏng... việc gọi ```recv``` không thể phát hiện được, việc ghi sẽ nhận được tín hiệu ```EPIPE``` cũng raise 1 exception.

### Blocking I/O

Ở chế độ blocking, các hàm ```recv``` và ```send``` sẽ đợi dữ liệu được đọc và ghi xong mới return. Trong thời gian chờ, các việc khác không thể được thực hiện. 

### Non-blocking

Các hàm ```recv``` và ```send``` có thể return ngay mà chưa thực hiện xong việc. Ví dụ ```send``` chưa ghi hết dữ liệu xuống buffer, buffer đầy và nó sẽ không đợi buffer trống mà dừng lại ngay. 

Hàm ```select.select( potential_readers, potential_writers,potential_errs, timeout)``` nhận đầu vào là 3 mảng. Mảng các socket muốn kiểm tra cs thể sử dụng để ghi dữ liệu, đọc dữ liệu, có lỗi xảy ra, và timeout là tham số để giới hạn thời gian kiểm tra. Đây là 1 câu lệnh dạng blocking.
Có thể ứng dụng trong server dạng event-driven khi các sự kiện xảy ra trên socket, client gửi dữ liệu lên server gây ra 1 socket có thể được đọc(readable), server gửi dữ liệu cho client cần kiểm tra socket có thể được ghi hay không. Tất cả server chỉ cần chạy trên 1 thread duy nhất(nodejs).

### TCP keepalive

```tcp_keepalive_time```

khoảng thời gian kể từ khi kết nối còn hoạt động lần cuối và cần kiểm tra 

```tcp_keepalive_intvl```

khoảng thời gian giữa 2 lần gửi thông điệp keepalive liên tiếp

```tcp_keepalive_probes```

số thông điệp thăm dò không có trả lời được phép trước khi quyết định là kết nối đã chết và thông báo cho tầng ứng dụng.

