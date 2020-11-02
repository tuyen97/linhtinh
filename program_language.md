### JVM vs Compiled

- Performance: compile có lợi thế do được biên dịch trực tiếp thành mã máy

- Security: jvm an toàn hơn do có thêm bước check bytecode trước khi thực thi

- Portability: write once, run everywhere. Mỗi nền tảng có 1 bộ thư viện lập trình khác nhau: socket, GUI,... code cần viết riêng cho mỗi nền tảng. JVM làm trung gian nên code không cần phải viết riêng.

### HotSpot JVM

Đoạn code được lặp lại nhiều lần sẽ được biên dịch thẳng ra mã máy.
