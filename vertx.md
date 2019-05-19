## Vert.x

"A toolkit for building reactive applications on the JVM"

- <b>toolkit</b>: Vert.x là 1 file jar, 1 ứng dụng Vert.x là ứng dụng sử dụng file ```jar``` này. Vert.x không định nghĩa cách đóng gói nào cả, tất các phần của Vert.x đều nằm trong file jar. 

- <b>reactive</b>: nó được làm ra để xây dụng các ứng hệ thống reactive, với 4 điểm mấu chốt:

   - Responsive: xử lí yêu cầu trong thời gian chấp nhận được.

   - Resilient: hệ thống phải giữ được tính responsive trước các lỗi(crash, timeout, ```500```...)

   - Elastic: phải giữ được tính responsive dưới bất kì tình huống tải nào, dù mở rộng hay thu hẹp quy mô của hệ thống và có thể xử lí tải với tài nguyên ít nhất có thể.

   - Hướng thông điệp: các thành phần trong hệ thống reactive tương tác bằng asynchronous message-passing.

## Microservice

Kiểu kiến trúc phát triển ứng dụng từ bộ các service nhỏ chạy trên các tiến trình riêng biệt giao tiếp với nhau qua kĩ thuật đơn giản, thường là HTTP. Các service được triển khai 1 cách độc lập bằng công cụ triển khai tự động.

Các service được triển khai tự động và cần có kĩ thuật để định danh.

Mục tiêu khi sử dụng microservice là khả năng triển khai nhanh chóng, tính thích ứng, độc lập lẫn nhau và khả năng thay thế sửa chữa. Mỗi service có thể được thay thế bằng service cung cấp dịch vụ/giao diện/API tương tự.

### Async RPC

Đối tượng gọi không bị chặn dừng, kết quả trả về từ phía được gọi là 1 ```AsyncResult<X>```.

```Future.succeededFuture``` dùng để tạo 1 đối tượng ```AsyncResult``` đại diện cho kết quả trả về.

```AsyncResult``` mô tả:

- kết quả trả về thành công

- lỗi, ```Throwable```

Async RPC có thể được gọi trên Event bus.
