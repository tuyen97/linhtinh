- Bước đầu tiên tạo ngữ cảnh cho ứng dụng(application context): ClassPathXmlApplicationContext(), thực hiện tải các cấu hình của các bean, khởi tạo các bean này.

- Bước 2: sử dụng phương thức getBean(), sử dụng bean id để lấy về đới tượng bean.

## Tạo file cấu hình cho bean 

- File xml chứa cấu hình các bean 

- thẻ bean dùng để định nghĩa 1 bean mới

- thẻ property đẻ truyền tham số cho các hàm trong bean.

## IoC container

- chịu trách nhiệm tạo các đối tượng, kết nối chúng lại, cấu hình và quản lí từ lúc được tạo cho đến khi bị hủy đi. Các đối tượng này gọi là Spring Beans.

- sử dụng các chỉ dẫn trong metadata để tạo ra các đối tượng mới này từ các lớp Java cũ.

- gồm 2 loại: BeanFactory và ApplicationContext.

## Định nghĩa bean

- là các đối tượng làm nên xương sống của ứng dụng, được tạo ra và quản lí bởi IoC.

- cung cấp metadata cho IoC để tạo ra bean

- 3 cách để viết file định nghĩa:
  - XML
  - dựa trên chú thích
  - file java thông thường

## Phạm vi của bean

```Singleton```: tạo ra 1 đối tượng dùng chung cho tất cả các request đến

```Protype```: tạo ra 1 đối tượng mới cho mỗi request.

## Vòng đời của bean

- Khi bean được tạo có thể cần thực hiện 1 vài hành động trước khi nó có thể sử đụng được, tương tự khi bean kết thúc vòng đời của nó.

- Có thể đăng kí 2 hàm được thực hiện lúc khởi tạo và kết thúc bean thông qua thuộc tính ```init-method``` và ```destroy-method``` trong thẻ bean của file XML metadata.

## Kế thừa các định nghĩa

bean con có thể kế thừa tất cả cấu hình của bean cha giống việc kế thừa trong Java.

## Dependence injection

- thực hiện kết nối các lớp trong Java, truyền các lớp như là tham số vào trong hàm.

### DI dựa trên constructor

được hoàn thành khi container gọi tới hàm khởi tạo của 1 lớp với các tham số, mỗi tham số này là 1 đối tượng của lớp khác.

Ví dụ:

```
<?xml version = "1.0" encoding = "UTF-8"?>

<beans xmlns = "http://www.springframework.org/schema/beans"
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation = "http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <!-- Definition for textEditor bean -->
   <bean id = "textEditor" class = "com.tutorialspoint.TextEditor">
      <constructor-arg ref = "spellChecker"/>
   </bean>

   <!-- Definition for spellChecker bean -->
   <bean id = "spellChecker" class = "com.tutorialspoint.SpellChecker"></bean>

</beans>
```

bean có id là spellChecker tương ứng với lớp spellChecker đã được truyền vào trong constructor của bean textEditor

- thứ tự truyền biến được xác định bằng từ khóa index.

-  truyền đối tượng sử dụng từ khóa ```ref```, truyền giá trị sử dụng từ khóa ```value```

### DI dựa trên setter

được hoàn thành khi gọi phương thức setter sau khi khởi tạo bean bằng hàm khởi tạo ko tham số.

```
 <bean id = "textEditor" class = "com.tutorialspoint.TextEditor">
      <property name = "spellChecker" ref = "spellChecker"/>
   </bean>

   <!-- Definition for spellChecker bean -->
   <bean id = "spellChecker" class = "com.tutorialspoint.SpellChecker"></bean>
```
- khác biệt với cách trên là từ khóa ```property``` so với ```constructor-arg```

### Kết luận

- thuộc tính/thẻ ```property``` dùng để gọi đến 1 hàm trong đối tượng java, ```contructor-arg``` gọi đến hàm khởi tạo. CÓ thể truyền tham số cho các hàm này bằng từ khóa ```value``` hoặc ```ref```.

ví dụ:

```
   <bean id = "textEditor" class = "com.tutorialspoint.TextEditor">
      <property name = "spellChecker">
         <bean id = "spellChecker" class = "com.tutorialspoint.SpellChecker"/>
      </property>
   </bean>
```

## Truyền tập hợp

- list, set, map, properties

```
<property name = "addressList">
         <list>
            <value>INDIA</value>
            <value>Pakistan</value>
            <value>USA</value>
            <value>USA</value>
         </list>
</property>
```

## Autowiring

Tự động liên kết các lớp có liên quan với nhau mà k cần sử dụng các thành phần ```property``` và ```constructor-arg```

### ByName

- Tự động liên kết thông qua tên thuộc tính. Container sẽ tự động tìm các bean có <b>tên</b> giống với tên thuộc tính và truyền vào hàm set

### ByType

Container sẽ tự động tìm các bean có tên giống với <b>loại</b> thuộc tính và truyền vào hàm set

### Constructor

Container sẽ tự động tìm các bean có tên giống với <b>loại</b> thuộc tính và truyền vào hàm khởi tạo.

### Auto detect

Spring sẽ tự động liên kết bằng <i>constructor</i>, nếu không được sẽ chuyển sang <i>byType</i>

## Cấu hình bean bằng chú thích

Thay vì sử dụng file XML để liên kết các bean, ta có thể định nghĩa liên kết trong các lớp bằng cách sử dụng các chú thích @.

### Required

Chú thích này đặt trước hàm set và chỉ ra rằng các thuộc tính trong hàm này bắt buộc phải xuất hiện trong file cấu hình XML

### Autowired

- Nếu đặt trước phương thức setter, ta không cần container sẽ thực hiện tự động liên kết <b>byType</b>

- Nếu đặt trước thuộc tính, ta không cần hàm set cho thuộc tính này nữa, container sẽ tự động tìm tên bean trùng với tên thuộc tính và gán thuộc tính này = bean.

- Nếu đặt trước constructor, container sẽ thực hiện tự động liên kết <b>constructor</b>.

- Mặc định thì @Autowired giống như required, nếu không cung cấp đủ tham số thì sinh lỗi. Nếu ta đặt @Autowired(required=false), thì thành phần này có thể không có trong file cấu hình XMl cũng đc.

