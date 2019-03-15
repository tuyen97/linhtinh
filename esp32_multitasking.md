Tạo hàm task mới bằng cách gọi hàm ```xTaskCreate``` vơi các tham số:

- <b>TaskCode</b>: con trỏ đến hàm cần thực hiện 

- <b>TaskName</b>: tên task 

- <b>StackDepth</b>: kích cỡ stack của task-số biến nó có thể lưu được(khác với kích cỡ bằng byte). 

- <b>Parameter</b>: con trỏ tới các tham số mà task có thể nhận, cần có kiểu là (void*)

- <b>Priority</b>: độ ưu tiên của task

- <b>TaskHandle</b>

Hàm task không nên có trả về, nên ko được chứa từ khóa return và phải chạy cho đến cuối cùng. 
