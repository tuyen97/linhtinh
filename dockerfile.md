## Mục lục
- 1. [Usage](#1)
- 2. [Format](#2)
- 3. [ENV](#3)
- 4. [FROM](#4)
- 5. [RUN](#5)
- 6. [CMD](#6)
- 7. [ENTRYPOINT](#7)
- 8. [COPY](#8)

```Dockerfile``` là 1 file text chứa tất các chỉ dẫn để tạo ra 1 image. 

<a name="1"></a>
### Usage

Câu lệnh ```docker build``` tạo ra 1 image từ ```Dockerfile``` và <i>1 context</i>. Context là tập các file ở 1 vị trí nào đó xác định bởi ```PATH``` và ```URL```. ```PATH``` là thư mục trong máy tính còn ```URL``` là địa chỉ của Git repo. 

Context được xử lí đệ quy, tất cả file và thư mục trong context sẽ được gửi tới cho docker để thực hiện việc build.

<a name="2"></a>
### Format

Các lệnh  trong ```Dockerfile``` có dạng 

```
#Comment
INSTRUCTION arguments
```

Tên lệnh là không phân biệt hoa thường nhưng nên sử dụng chữ hoa để phân biệt với các biến số bên cạnh.

Docker sẽ thực hiện lệnh theo thứ tự, 1 ```Dockerfile``` phải bắt đầu với lệnh ```FROM``` chỉ ra "base image" của image đang xây dựng.

Các dòng bắt đầu bằng # được coi là các comment, các dấu # trong 1 dòng bất kì được coi là 1 tham số. Ví dụ:

```
# Comment
RUN echo 'we are running some # of cool things'
```
### ENV

Câu lệnh ```ENV``` khai báo các biến cùng giá trị để có thể sử dụng trong các câu lệnh khác, thường được dùng để đại diện cho 1 giá trị nào đó. 

Các biến được kí hiệu là ```$ten_bien``` hoặc ```${ten_bien}```, có thể đặt dấu \ ở trước kí hiệu biến để trình dịch không lấy giá trị của các biến này, ví dụ :

```
FROM busybox
ENV foo /bar
WORKDIR ${foo}   # WORKDIR /bar
ADD . $foo       # ADD . /bar
COPY \$foo /quux # COPY $foo /quux
```
Trong cùng 1 câu lệnh, giá trị của biến là không đổi, Ví dụ:

```
ENV abc=hello
ENV abc=bye def=$abc
ENV ghi=$abc
```

```def``` sẽ có giá trị là ```hello``` vì giá trị của ```abc``` đã được chọn trước cho toàn bộ lệnh thứ 2 là ```hello``` việc gán giá trị mới không có tác dụng trong lệnh này. Còn ```ghi``` sẽ có giá trị là ```bye``` vì đã ở câu lệnh khác và việc gán ```abc``` đã có tác dụng.

<a name="4"></a>
### FROM 

Các dạng:

```
FROM <image> [AS <name>]
```

```
FROM <image>[:<tag>] [AS <name>]
```

```
FROM <image>[@<digest>] [AS <name>]
```

Lệnh ```FROM``` chỉ ra image là "base-image" của image cần build, các câu lệnh phía sau sẽ thêm các layer trên image này. Một ```Dockerfile``` hợp lệ phải bắt đầu với ```FROM```.

- ```ARG``` là câu lệnh duy nhất có thể xuất hiện trước ```FROM``` dùng để khai báo giá trị các biến được dùng trong ```FROM```. Ví dụ:

```
ARG VERSION=latest
FROM busybox:$VERSION
```

- Trong ```Dockerfile``` có thể có nhiều lệnh ```FROM``` để tạo nhiều image cùng lúc hoác tạo ra 1 loạt các trung gian trước khi tạo image cuối cùng.

<a name="5"></a>
### RUN 

RUN có 2 dạng:

- ```RUN <command>``` ( dạng <i>shell<i>, command được chạy trong shell, mặc định của Linux là ```/bin/sh -c``` hoặc ```cmd /S /C``` trong Windows).

- ```RUN ["executable", "param1", "param2"]```(dạng <i>exec</i>).

Câu lệnh ```RUN``` tạo ra 1 layer mới từ layer ngay dưới nó. Kết quả của lệnh này tạo ra các thay đổi trên hệ thống file của base-image (như là việc cài đặt các gói phần mềm sẽ ghi vào hệ thống file này). Mỗi lệnh sẽ tạo ra 1 layer mới. 

Ở  dạng shell, shell <b>trong container</b> sẽ được gọi và thực thi lệnh, mặc định là ```/bin/sh```, nếu docker không tìm thấy shell này thì nó sẽ báo lỗi. Có thể thay đổi shell mặc định bằng lệnh ```SHELL```

Ở dạng exec, "excutable" là đường dẫn tói file thực thi nào đó, câu lệnh được thực hiện mà không cần gọi shell. ví dụ : ```RUN ["/bin/ping","localhost"]```.

<a name="6"></a>
### CMD

Để đặt câu lệnh mặc định khi khởi động 1 container ta có thể khai báo trong ```CMD``` với 3 dạng:

- ```CMD ["executable","param1","param2"]```(dạng exec)
- ```CMD ["param1","param2"]```( khai báo các tham số cho ```ENTRYPOINT```)
- ```CMD command param1 param2```(dạng shell)

Chỉ có thể có 1 câu lệnh ```CMD``` trong 1 ```Dockerfile```, nếu có nhiều hơn thì chỉ câu lệnh cuối cùng có hiệu lực.

Mục đích chính của ```CMD``` là đưa ra giá mặc định khi container được khởi động, các giá trị này có thể là 1 câu lệnh hoàn chỉnh hoặc chỉ là 1 tham số truyền vào cho ```ENTRYPOINT```.

Các giá trị này có thể bị ghi đè nếu ta định nghĩa khi chạy lệnh ```docker run```. Ví dụ, khi chạy image ```ubuntu``` nếu không truyền tham số gì thì mặc định container sẽ được khởi động và thực hiện shell ```sh```, còn nếu có tham số ví dụ:

```
docker run -it ubuntu /bin/ping localhost
```

Thì shell ```sh``` sẽ không được khởi chạy nữa mà thay vào đó là lệnh ```ping```.

<a name="7"></a>
### ENTRYPOINT

Mục đích của ```ENTRYPOINT``` cũng là cung cấp các mặc định khi container được khởi động. Tuy nhiên, khác với ```CMD``` các mặc định này là cố định sẽ luôn được thực hiện.

Có 2 dạng:

- ```ENTRYPOINT ["executable", "param1", "param2"]```(dạng <i>exec</i>)
- ```ENTRYPOINT command param1 param2```(dạng <i>shell</i>)

Các giá trị được cung cấp cho lệnh ```docker run ``` sẽ được thêm vào sau tất cả các thành phần của dạng <i>exec</i> và ghi đè các giá trị của lệnh ```CMD```.

Ví dụ về tương tác ```CMD/RUN```:

||Không có ```ENTRYPOINT```|ENTRYPOINT exec_entry p1_entry|ENTRYPOINT [“exec_entry”, “p1_entry”]|
|---|---|---|---|
|Không có CMD|Lỗi|/bin/sh -c exec_entry p1_entry|exec_entry p1_entry|
|CMD [“exec_cmd”, “p1_cmd”]|exec_cmd p1_cmd|/bin/sh -c exec_entry p1_entry|exec_entry p1_entry exec_cmd p1_cmd|
|CMD [“p1_cmd”, “p2_cmd”]|p1_cmd p2_cmd|/bin/sh -c exec_entry p1_entry|exec_entry p1_entry p1_cmd p2_cmd|
|CMD exec_cmd p1_cmd|/bin/sh -c exec_cmd p1_cmd|/bin/sh -c exec_entry p1_entry|exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd|

Chỉ có sử dụng ```ENTRYPOINT``` ở dạng excute(định dạng json) thì các lệnh ```CMD``` mới có giá trị sử dụng, ở dạng shell câu lệnh ```CMD``` hoàn toàn không gây tác động.

<a name="8"></a>
### COPY 

COPY có 2 dạng :
- ```COPY [--chown=<user>:<group>] <src>... <dest>```
- ```COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]```(dùng khi đường dẫn có chứa dấu cách)

Câu lệnh ```COPY``` thực hiện sao chép tất cả tệp và thư mục tại đường dẫn ```<src>``` của "context" tới filesystem của container tại đường dẫn ```<dst>```.

```<src>``` có thể là đường dẫn tuyệt đối hoặc đường dẫn tương đối so với ```WORKDIR```.Ví dụ:

```
COPY test relativeDir/   # adds "test" to `WORKDIR`/relativeDir/
COPY test /absoluteDir/  # adds "test" to /absoluteDir/
```

Ngoài ra còn có lệnh ```ADD``` có chức năng tương tự và có thêm chức năng:

- ```<src>``` có thể là URL của file nằm trên mạng

- Nếu ```<src>``` là 1 file nén trong máy tính thì nó có thể được tự động giải nén và sao chép vào bên trong ```<dst>```

Tuy nhiên, ```COPY``` được khuyên dùng do nó rõ ràng hơn vì chỉ cho phép sao chép các file và thư mục trong máy tính. ```ADD``` thường được dùng để dựng các base-image như busybox, alpine,... vì khả năng tự giải nén của nó. Ví dụ với busybox:

```
FROM scratch
ADD busybox.tar.xz /
CMD ["sh"]
```

<a name="9"></a>
### WORKDIR

```
WORKDIR /path/to/workdir
```

Đặt thư mục làm việc của các câu lệnh ```RUN```, ```CMD```, ```ENTRYPOINT```, ```COPY``` và ```ADD``` theo sau nó. Nếu thư mục này không tồn tại, nó sẽ được tạo mới.

Các đường dẫn tương đối đều lấy mốc là ```WORKDIR```.
