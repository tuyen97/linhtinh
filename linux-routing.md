### Interface

- Có thể là interface vật lí (```eth0```) hoặc là ảo (```lo``` hoặc ```docker0```)

- Liệt kê bằng ```ifconfig```

- Nếu không có interface nào thì gói tin sẽ không thể vào được chồng mạng của linux. Để đưa gói tin vào chồng mạng cần có interface.

- Khi gửi gói tin tới địa chỉ IP nào đó, bảng định tuyến được sử dụng để quyết định gửi gói tin đến interface nào. Đây là việc đầu tiên xảy ra trong chồng giao thức mạng.

- ```tcpdump``` bắt các gói tin sau khi đã được định tuyến tức là đã được gán cho interface.

### Gateway

Là địa chỉ của 1 nút mạng, nút mạng này được kết nối với 1 interface.

1 interface có thể có nhiều gateway. Nhưng 1 gateway chỉ gắn với 1 interface.

```
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         _gateway        0.0.0.0         UG    600    0        0 wlp3s0
link-local      0.0.0.0         255.255.0.0     U     1000   0        0 wlp3s0
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
172.18.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker_gwbridge
192.168.3.0     0.0.0.0         255.255.255.0   U     600    0        0 wlp3s0
192.168.99.0    0.0.0.0         255.255.255.0   U     0      0        0 vboxnet0
```

Theo bảng định tuyến trên thì gói tin gửi tới địa chỉ IP thuộc mạng 192.168.3.0/24 sẽ được đưa tới interface ```wlp3s0``` sau đó chuyển tới gatewway mặc định.

### Định tuyến

Đối với ```router``` thì nó có thể chuyển gói tin IP giữa các interface khác nhau còn các thiết bị khác thì không. 1 máy tính thông thưởng nếu nhận được gói tin không phải của nó thì nó sẽ hủy gói tin còn router sẽ tìm interface và gateway phù hợp để chuyển gói tin ra ngoài.

Nguyên lí định tuyến: So khớp các bản ghi trong bảng định tuyến

- Nếu tìm thấy bản ghi nào khóp cả hostID và networkID thì gửi gói tin tới interface tương ứng.

- Nếu không thấy bản ghi nào khớp cả 2 phần thì so khớp phần networkID, nếu tìm thấy gửi tới interface và gateway tương ứng.

- Nếu không thấy cả 2 thì gửi tới interface với gateway mặc định.  
