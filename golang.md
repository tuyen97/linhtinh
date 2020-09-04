## Golang Package

Một package có thể bao gồm nhiều file code. Tất cả các file trong cùng package phải nằm trong cùng 1 thư mục, thư mục chứa các file code của 1 package gọi là thư mục của package. Các file nằm trong một thư mục (không bao gồm các file nằm trong thư mục con) phải thuộc cùng 1 package.

Package có đường dẫn import chứa ```internal``` là 1 package đặc biệt. Nó chỉ có thể được import từ các package nằm trong thư mục cha của thư mục ```internal```, ví dụ package ```.../a/b/c/internal/d/e/f``` và ```.../a/b/c/internal``` chỉ có thể được import bởi packet có đường dẫn bắt đầu bởi ```.../a/b/c```

Module là các package được tập hợp lại có cùng thư mục gốc. Biến ```GO111MODULE``` điều khiển tính năng module, có thể là on, off hoặc auto.

Nếu package nằm trong thư mục ```GOPATH/src ```, module feature off thì đường dẫn import của nó chính là đường dẫn tương đối tới ```GOPATH/src``` hoặc tới thư mục ```vendor``` gần nhất chứa thư mục của package.

Ví dụ: 

```bash
_ GOPATH
  |_ src
     |_ x
        |_ vendor
        |  |_ w
        |     |_ foo
        |        |_ foo.go    // package foo
        |_ y
        |  |_ vendor
        |  |  |_ w
        |  |     |_ foo
        |  |        |_ foo.go // package foo
        |  |_ y.go            // package y
        |_ z
        |  |_ z.go            // package z
        |_ x.go               // package x
```

- import path của 2 package foo đều là w/foo

- import path của x, y, z lần lượt là x, x/z, x/y

Khi module feature là on, đường dẫn import của module được chỉ ra trong file ```go.mod``` nằm trong thư mục gốc của module. Đường dẫn này được sử dụng làm tiền tố cho tất cả các đường dẫn import của các  package trong module. Ngoại lệ đối với thư mục ```vendor``` nằm trong thư mục gốc của module. 

Ví dụ: 

```
_ MyProject
     |_ go.mod                // module example.com/mypkg
     |_ vendor
     |  |_ w
     |     |_ foo
     |        |_ foo.go       // package foo
     |_ x
        |_ y
        |  |_ vendor
        |  |  |_ w
        |  |     |_ foo
        |  |        |_ foo.go // package foo
        |  |_ y.go            // package y
        |_ z
        |  |_ z.go            // package z
        |_ x.go               // package x

```

- Đường dẫn để import package foo đầu tiên là w/foo vì ```vendor``` được coi là thư mục đặc biệt

- Đường dẫn để import package foo thứ hai là ```example.com/mypkg/x/y/vendor/w/foo```

- ĐƯờng dẫn để import x, y, z tướng ứng là ```example.com/mypkg/x```, ```example.com/mypkg/x/y```, ```example.com/mypkg/x/z```

Khi 1 file trong package import 1 package khác, ta nói package đó phụ thuộc vào package được import.

Go không cho phép import vòng, nếu ```a``` phụ thuộc vào ```b```, ```b``` phụ thuộc vào ```c```, thì ```b``` không thể import ngược lại ```a```, ```c``` không thể import ngược lại ```b``` và cũng không thể import ```a``` do tạo thành vòng tròn. 

Tên của folder chứa package không nhất thiết cần phải giống với tên package.  
