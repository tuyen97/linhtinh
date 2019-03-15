Xây dựng chuẩn để build project, định nghĩa các phần mà project chứa trong nõ, công bố các thông tin về project dễ dàng hơn và 1 cách để chia sẻ file JAR giữa các project.

### Mục tiêu của Maven

- Làm cho việc build dễ dàng hơn

- Cung cấp hệ thống build đồng bộ

- Cung cấp thông tin về project

- Cho phép chắp nối khi có tính năng mới

### POM 

Là 1 file XML chứa các thông tin về project và chi tiết cấu hình để build project. Chứa các thông tin mặc định cho hầu hết các project: thư mục build là ```target```, thư mục mã nguồn ```src/main/java```, thư mục kiểm thử ```src/test/java```. Khi thực hiện 1 nhiệm vụ maven sẽ truy theo thông tin của POM trong thư mục hiện tại để lấy thông tin cấu hình.

#### POM tối thiểu

- project root

- modelVersion

- groupId

- artifactId

- version

```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1</version>
</project>
```

### Mẫu thư mục chuẩn

|Thư mục|Mục đích|
|---|---|
|```src/main/java```|mã nguồn của ứng dụng/thư viện|
|```src/main/resources```|mã nguồn của ứng dụng/thư viện|
|```src/main/webapp```|mã nguồn ứng dụng web|
|```src/test/java```|mã nguồn kiểm thử|

Trong 1 project, cấp đầu tiên của thư mục chưa ```pom.xml``` và có thể có ```README.txt```, ```LICENSE.txt```...

Chỉ có 2 thư mục con của cấp đầu tiên là ```src``` và ```target```. Ngoại lệ với ```.CVS```, ```.svn``` và ```.git```.

```target``` là thư mục chứa đầu ra của việc build.

```src``` là thư mục chứa các tài nguyên cần thiết để build project. 
