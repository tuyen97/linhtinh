### Communicator
   1 nhóm tiến trình có khả năng giao tiếp với nhau.Mỗi tiến trình được gán rank và giao tiếp với tiến trình có rank khác.
   
   Collective vs point-to-point communication

- ```MPI_Recv``` truyền 1 mảng các phần tử, nhận tham số là 1 con trỏ.

- ```MPI_Recv``` không đảm bảo nhận toàn bộ số phần tử được truyền trong lời gọi hàm. Nó nhận đúng số phần tử được truyền đến ( trả về lỗi nếu nhận được số phần tử nhiều hơn so với tham số được truyền vào). ```MPI_Get_count``` được sử dụng để để xác định số phần tử đã nhận.

- Sử dụng ```MPI_Probe``` để xác định kích thước thước thông điệp trước khi gửi đi.:

### Random walk

- 
