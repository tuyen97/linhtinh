## Mục lục

- 1. [Tổng quan](#tq)
- 2. [Nguyên lí hoạt động](#ngli)
  -  2.1. [Filesystem](#ngli_filesystem)
<a name="tq"></a>
## Tổng quan.

Docker là 1  nền tảng để phát triển, triển khai, chạy các ứng dụng thông qua ý tưởng container. Tập trung vào việc tự động hóa việc triển khai các ứng dụng, giúp việc triển khai dễ dàng hơn.

Container cung cấp môi trường tách biệt cho các ứng dụng chạy trên cùng 1 phần cứng. Môi trường này là hoàn tòan đầy đủ và không phụ thuộc vào phần cứng hay hệ điều hành nên việc triển khai các container là cực kì tiện lợi.

Để tạo được môi trường này, người ta sử dụng các chức năng của nhân hệ điều hành Linux là Namespace và cgroups. Namespace dùng để giới hạn tài nguyên mà 1 nhóm các tiến trình được sử dụng. Cgroups dùng để giới hạn tổng tài nguyên mà 1 container được cấp phát.

Các container chia sẻ chung hệ điều hành với nhau, truy cập trực tiếp vào tài nguyên mặc dù các tài nguyên được cấp phát và sử dụng là tách biệt. Điều này làm cho các container khởi động nhanh hơn rất nhiều so với dùng máy ảo. Mỗi container khi được khởi động là 1 tiến trình con của daemon ```dockerd```, mang ```pid = 1``` trong namespace của nó. Từ tiến trình này có thể sinh ra nhiều tiến trình con thuộc cùng 1 container.

```
root      3967  0.0  0.0   7652  4436 ?        Sl   15:18   0:00      \_ docker-containerd-shim -namespace moby -workdir /var/lib/docker/conta
root      3984  0.0  0.0  18508  3276 pts/0    Ss+  15:18   0:00      |   \_ /bin/bash
root      4118  0.0  0.0   7524  3192 ?        Sl   15:21   0:00      \_ docker-containerd-shim -namespace moby -workdir /var/lib/docker/conta
root      4136  0.1  0.0   1592   900 pts/0    Ss+  15:21   0:00          \_ /bin/sh
```
<a name="ngli"></a>
## Nguyên lí hoạt động

Mục đích của container là tạo ra môi trường chạy ứng dụng tách biệt nên mỗi container cần có 1 thư mục làm việc tách biêt, cấu trúc cây tiến trình riêng, bộ nhớ và cpu được cấp phát riêng,... Việc này đạt được nhờ 2 chức năng của hệ điều hành Linux là Namespace và cgroups.

<a name="y_tuong"></a>
1. Ý tưởng cơ bản

   Ta sẽ chọn 1 thư mục để tạo thư mục làm việc cho container, thư mục này là chỉ có container thấy được mà các tiến trình bên ngoài không được phép truy cập vào và nó cũng không thấy được bên ngoài thư mục này. VIệc này giống như ta thực hiện lệnh ```chroot```. 
   
   Mọi file nhị phân thực thi đều được đặt trong thư mục này. Hệ điều hành sẽ cho thực thi các file này để tạo các tiến trình. Các ```image``` thực ra là việc triển khai thư mục làm việc này thành các thư mục con và các file nhị phân bên trong nó . Ví dụ, đối với busybox có Dockerfile như sau:
   ```
   FROM scratch
   ADD busybox.tar.xz /
   CMD ["sh"] 
   ```
 
   Câu lệnh ```ADD``` sẽ copy tất cả thư mục trong busybox.tar.xz vào thư mục làm việc của container này tại "/" tức là thư mục gốc. Ta có thể tải về và giải nén thư mục busybox.tar.xz để xem bên trong :

   ![](./img/busybox.png)
 
   Khi chạy lệnh ```ls``` trong busybox cũng thu được cấu trúc thư mục tương tự:  
   ``` 
   root@tuyen:~# docker run -it busybox
   / # ls
   bin   dev   etc   home  proc  root  sys   tmp   usr   var
   / #
   ```  
  
   Khi ta chạy ```sh``` bên trong này thực chất là đã yêu cầu hệ điều hành thực thi file "sh" nằm trong thư mục ```/bin``` của container. Tương tự đối với ubuntu và các image khác. 

   Điều này làm rõ rất nhiều nhận xét về container. Ví dụ như các container chia sẻ chung nhân hệ điều hành chủ vì các tiến trình của container đều do hệ điều hành chủ thực thi file nhị phân nằm trong thư mục làm việc của container. Không thể chạy các ứng dụng windows được do các file nhị phân của windows linux không thể thực hiện được. Container khởi động rất nhanh vì chỉ là tiến trình của hệ điều hành chủ...

   Và cũng đem đến 1 số trở ngại như: cây tiến trình bên trong container là riêng biết với 1 process được đặt ```pid=1```, tuy nhiên không có ```init process```(do không tải đầy đủ nhân hệ điều hành mà chỉ có hệ thống thư mục gốc )để quản lí các tiến trình con và khởi động các dịch vụ hệ thống như: nginx, sshd, ... Ta cần tạo tự khởi động các dịch vụ và tạo ra 1 tiến trình "giả" chức năng của ```init process``` như ```phusion/base-image```.
 
<a name="ngli_fs"></a>
1. Filesystem

   Mỗi container cần có 1 hệ thống filesystem riêng mà chỉ có nó thấy được mà các container khác không thấy được và cũng không thể thao tác ra bên ngoài filesystem này. Việc này giống như ta thực hiện câu lệnh ```chroot``` trên 1 thư mục vậy. 
   
   Mọi file nhị phân được thực hiện phải đặt trong filesystem này. Điều này có ý nghĩa quan trọng, 1 container thực ra là các tiến trình được tạo ra bằng cách thực thi các  file nhị phân đặt trong filesystem này, thực hiện các thao tác trên nó. Ví dụ đối với ```image``` ubuntu mà ta pull từ docker hub về thực ra là bản sao của  filesystem ```rootfs```(cấu trúc thư mục gốc của ubuntu), việc ta chạy ```image``` này là tạo ra các tiến trình sử dụng các file thực thi nhị phân trong nó - file  /bin/bash sẽ được gọi khi ta chạy bash shell của ```image``` ubuntu này. Các gói phần mềm được cài đặt cũng đặt trên filesystem này. Do việc thực thi mã nhị phân là do nhân hệ điều hành chủ thực hiện nên container có ưu điểm là khởi động rất nhanh vì không cần boot vào hệ điều hành ảo như VM và nhẹ do không cần lớp trung gian cho các yêu cầu tài nguyên(hypervisor). Nhưng cũng chính vì không tải nguyên cả nhân hệ điều hành trong container nên không thể chạy được các distro khác trên máy chủ.  

   Mặc định kể cả ```image``` không chứa thông tin về cây thư mục ví dụ như ```image``` Busybox( Busybox chỉ là 1 file nhị phân khi chạy cung cấp các câu lệnh của hệ điều hành tựa Unix) thì docker vẫn tạo cho ```container``` 1 filesystem riên. Ta có thể thực hiện câu lệnh ```ls``` trong container Busybox để thấy rõ:

   ```
   root@tuyen:~# docker run -it busybox
   / # ls
   bin   dev   etc   home  proc  root  sys   tmp   usr   var
   / # 
   ```
   Mọi thay đổi tạo ra bởi container trên hệ thống file này sẽ bị hủy bỏ khi container dừng hoạt đông.

### Dockerfile

```Dockerfile``` là 1 file text chứa các câu lệnh để xây dựng lên 1 image. 

Mỗi layer trên image được tạo ra bởi 1 lệnh trong ```Dockerfile```. Các tầng xếp chồng lên nhau và mỗi tầng lại là các thay đổi từ tầng dưới nó.

### Swarm

Swarm là 1 nhóm các máy tính đang chạy Docker và tham gia vào 1 cluster.

Các máy tính có thể là thực hoặc là máy ảo, được gọi là các ```node```.

Trong cluster có 1 ```node``` quản lí các ```node``` còn lại- các ```worker```, các lệnh chỉ có thể được gọi trên node quản lí, các ```worker``` đóng vai trò nhận và thực hiện nhiệm vụ được giao từ quản lí.

1. Set up swarm
  
   Máy chạy lệnh ```docker swarm init``` sẽ là máy quản lí, các máy khác tham gia vào swarm bằng cách chạy lệnh ```docker swarm join```.
