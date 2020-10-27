- Tách những phần biến thiên khỏi những phần cố định

- Hợp thành tốt hơn là kế thừa

- Program with interface, not implementations.i

- Bắt cặp lỏng lẻo giữa các đối tượng có tương tác với nhau.

- Class nên dễ dàng mở rộng nhưng khép kín với thay đổi. 

- Dependency inversion: phụ thuộc vào abstract không phụ thuộc vào class cụ thể.

- Nguyên lí Least Knowledge - giao tiếp với thành phần trung gian. Để ý đến số class mà 1 dối tượng tương tác. Tránh có 1 bộ phận lớn class phụ thuộc lẫn nhau.

Khi sử dụng kế thừa, hành vi được thiết lập trong lúc compile, tất cả các lớp con đều kế thừa cùng hành vi. Khi sử dụng hợp thành, hành vi được xác định lúc runtime. Có thể thêm bất kì chức năng nào mà không làm thay đổi logic đã có. 


## Stategy 

Sử dụng khi có 1 nhóm các hành vi tương tự  

## Observer

Các đối tượng có tương tác

## Decorator

Decorate(bao bọc) object 

Decorator có cùng lớp cha với đối tượng nó decorate (trang trí).

Decorator thêm hành vi 

Đối tượng có thể được decorate bao nhiều lần tùy ý ở runtime.

Sử dụng kế thừa để đạt tương thích kiểu (type matching) chứ không phải để đạt hành vi.

- Kế thừa là 1 dạng mở rộng nhưng có thể không phải cách tốt nhất để đạt được tính mềm dẻo

- Thiết kế cho phép mở rộng mà không cần chỉnh sửa thứ đã có sẵn.

- Hợp thành và uỷ nhiệm có thể được dùng để thêm hành vi mới lúc runtime

- Decorator cung cấp 1 cách thay thế cho kế thừa để mở rộng hành vi.

- Decorator bao gồm 1 tâp các lớp decorator đóng gói các component.

- Decorator phản chiếu (chính là) kiểu mà nó đóng gói. 

- Decorator thay đổi hành vi bằng cách thêm code vào trước hoặc/và sau lời gọi tới phương thức của component.
 
- Decorator tạo ra nhiều lớp gây rối loạn.

### Factory


### Facade

Không sử dụng đối tượng từ kết quả trả về của 1 hàm khác.

