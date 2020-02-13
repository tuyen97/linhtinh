###Xác thực người dùng:

- Mỗi người dùng đăng kí được tạo private key và public key ngẫu nhiên. Private key được mã hóa cùng với mật khẩu để lưu vào csdl

- Tạo authen bằng cách sign public key, mỗi client sẽ giữ authen key này và thêm vào header mỗi request. Server decode authen key để lấy public key xác thực tài khoản.

- Vấn đề: man-in-the-middle, đánh cắp token -> sử dụng https.

###Address:

- Nhiều block có cùng address để tiện truy nhập

- set_state(): thay đổi thông tin lưu trong 1 address, 1 chuỗi byte bất kì.

###Subscriber:


