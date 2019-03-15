### Chuỗi sự kiện tương ứng với HTTP request tới 

Thành phần đầu tiên tiếp nhận request là ```DispatcherServlet```:

- Sau khi nhận được HTTP request, DispatcherServlet hỏi ```HandlerMapping``` để gọi  ```Controller``` thích hợp.

- ```Controller``` nhận request và gọi các hàm thích hợp dựa vào phương thức là GET hay POST. Các hàm này sau đó gọi tới ```Model``` để trả về dữ liệu và ```View``` tương ứng.

- DispatcherServlet nhờ ```ViewResolver``` trợ giúp để biết view tương ứng.

- Sau khi biết, DispatcherServlet chuyển dữ liệu từ model cho view để tạo file html được render ra trên trình duyệt.

![](./img/spring_dispatcherservlet.png)

