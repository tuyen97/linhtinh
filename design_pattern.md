## Principles
- Tách những phần biến thiên khỏi những phần cố định

- Hợp thành tốt hơn là kế thừa

- Program with interface, not implementations.i

- Bắt cặp lỏng lẻo giữa các đối tượng có tương tác với nhau.

- Class nên dễ dàng mở rộng nhưng khép kín với thay đổi. 

- Dependency inversion: phụ thuộc vào abstract không phụ thuộc vào class cụ thể.

- Nguyên lí Least Knowledge - giao tiếp với thành phần trung gian. Để ý đến số class mà 1 dối tượng tương tác. Tránh có 1 bộ phận lớn class phụ thuộc lẫn nhau.

- Nguyên lí Hollywood: Don't call us, we call you: sử dụng hook để thành phần mức cao xác định nên gọi thành phần bậc thấp khi nào chứ không để thành phần mức thấp gọi trực tiếp thành phần mức cao.

- Single responsibility

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

Để lớp con quyết định việc khởi tạo đối tượng

Lớp cha định nghĩa 1 phương thức abstract để tạo đối tượng và 1 phương thức sử dụng phương thức abstract đó.

### Facade

1 phương thức của 1 class nên gọi hàm:

- Component của class

- Object do phương thức đó tạo ra

- Object được truyền làm tham số

- Phương thức khác đối tượng đó 

Không sử dụng đối tượng từ kết quả trả về của 1 hàm khác.

### Dependency Injection

Đối tượng không trực tiếp khởi tạo đối tượng khác nên chỉ phụ thuộc vào interface chứ không phụ thuộc vào kiểu cụ thể. Việc khởi tạo đối tượng được giao cho container sau đó được truyền nhờ các phương thức set hoặc constructor.
 
Đây cũng là cách thực hiện nguyên lí Dependency Inversion.

### Template method

Lớp con định nghĩa hành vi, lớp cha định nghĩa việc sử dụng các hành vi - don't call us, we call you.

High level gọi low level

### Composite

Compose object into tree structure,individual objects and compositions uniformly: coi mỗi nút con và các thuộc tính là 1
 
