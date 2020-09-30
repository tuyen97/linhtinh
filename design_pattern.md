- Tách những phần biến thiên khỏi những phần cố định

- Hợp thành tốt hơn là kế thừa

- Program with interface, not implementations.i

- Bắt cặp lỏng lẻo giữa các đối tượng có tương tác với nhau.

- Class nên dễ dàng mở rộng nhưng khép kín với thay đổi. 

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


