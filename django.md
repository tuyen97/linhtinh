Các app trong django là "pluggable": sử dụng 1 app trong nhiều project khác nhaunên cần phải khai báo app trong project trong ```settings.py```

### Model 

Model là thiết kế cơ sở dữ liệu cộng với metadata.

Bao gồm các thuộc tính và hành vi cơ bản.

Định nghĩa trong file models.py.

Mỗi model là 1 lớp con của django.db.models.Model, có các biến, mỗi biến là 
1 thuộc tính trong cơ sở dữ liệu.

Mỗi thuộc tính trong model là 1 thể hiện của lớp ```Field``` ví dụ: ```CharField```... định nghĩa kiểu dữ liệu của các biến.

Các thay đổi model:

- Sửa file ```models.py```
- Chạy lệnh ```python3 manage.py makemigrations ``` để tạo "migration"
- Chạy lệnh ```python3 manage.py migrate``` để áp dụng các "migration" cho cơ sở dữ liệu.

### View 

VIew là 1 loại Web page trong ứng dụng Django chứa 1 số chức năng và có 1 template cụ thể.

Ánh xạ từ URL - view: Django tự động tìm view tương ứng với URL thông qua ```URLconfs```

### Template

Là đoạn mã python được nhúng trong html, sẽ được server biên dịch thành html và trả kết quả cho người dùng.
Template sẽ được view gọi ví dụ qua câu lệnh ```render```.
Các template thường đăt trong thư mục template ví dụ: /templates/polls/index.html

### Static file

là các file css, js,... trợ giúp cho hiển thị template
Được đặt trong static ví dụ: /static/polls/style.css

### Kế thừa lớp <b>User</b>

Django sẽ sử dụng thông tin người dùng cung cấp để đăng nhập vào lần lượt các backend, nếu có kết quả thành công sẽ trả về.

Cách đơn gỉan nhất để xây dựng Backend riêng là xây dựng 1 lớp mới kế thừa lớp AbstractUser:

Các thuộc tính cần chỉ rõ:

- <b>USERNAME_FIELD</b>: Thuộc tính được set phải có giá trị là duy nhất cho mỗi đối tượng user mới(phải có unique=True) trong khai báo thuộc tính

Để chỉ ra backend dùng khi quản lí truy cập, cần đặt tên backend trong AUTHENTICATION_BACKENDS

1 backend là 1 lớp thực thi 2 phương thức get_user(user_id) và authenticate(request, **credentials)

Django cho phép ghi đè user mặc định bằng cách khai báo trong AUTH_USER_MODEL, ví dụ:

```
AUTH_USER_MODEL = 'myapp.MyUser'
```
### Form

Django làm 3 việc trên 1 form: 

- Chuẩn bị và cấu trúc dữ liệu 

- Tạo form HTML 

- Nhận và xử lí dữ liệu từ form trả về từ client

#### Lớp Form

- 1 thuộc tính của lớp Form ánh xạ tới 1 thẻ input trên HTML form.( ModelForm ánh xạ 1 thuộc tính của model tới 1 thẻ input).

- Mỗi thuộc tính của form đều là lớp riêng, chúng quản lí dữ liệu, kiểm tra ràng buộc khi form được submit.

- Mỗi thuộc tính của form được biểu diễn là 1 thành phần "widget" của HTML trên trình duyệt.

##### Xây dựng 1 form trên Django 

```python
from django import forms

class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100)
```

Đoạn code định nghĩa lớp Form với 1 thuộc tính là <b>your_name</b>, label là giá trị thuộc tính label trong HTML(mặc định cùng tên với thuộc tính của form).

<b>max_length</b> định nghĩa độ dài tối đa của trường dữ liệu này. Trình duyệt sẽ ngăn người dùng nhập quá 100 kí tự và khi nhận được dữ liệu Django cũng sẽ kiểm tra tính đúng đắn của trường này.

Đối tượng <b>Form</b> có phương thức <b>is_valid()</b>, phương thức này sẽ kiểm tra hợp lệ trên tất cả các thuộc tính của form, nếu tất cả đã hợp lệ, nó sẽ:

- trả về True 

- đặt dữ liệu của form vào thuộc tính <b>cleaned_data</b> của chính nó.

Đoạn code trên tạo ra mã HTML

```html
<label for="your_name">Your name: </label>
<input id="your_name" type="text" name="your_name" maxlength="100" required>
```
Phần còn lại của form HTML cần tự viết 

##### View

Form sau khi được submit được trả lại cho view, thường là cùng view đã in ra form.

Để xử lí form, cần tạo 1 đối tượng mới trong view: ```form = NameForm(request.POST)```, việc này tương đương với việc "đẩy dữ liệu từ POST vào form".

Sau đó, có thể gọi phương thức <b>is_valid()</b>, nếu trả về True, các trường của form được đưa vào thuộc tính cleaned_data và được xóa sạch, nếu không các trường này được giữ nguyên và được hiển thị trở lại để người dùng nhập lại cho đúng.

##### Duyệt qua các thuộc tính của trường field trong form

<b>{{ field.label }}</b> : nhãn của thuộc tính ví dụ: Email address

<b>{{ field.label_tag }}</b>: <label for="id_email">Email address:</label>

<b>{{ field.errors }}</b>: đưa ra thẻ <ul class="errorlist"> chứa các thông tin chưa hợp lệ về trường này. có thể tùy chỉnh hiển thị bằng cách duyệt qua các lỗi này bằng <b>{% for error in field.errors %}</b>, mỗi đối tưọng trong vòng lặp là 1 chuỗi hiển thị lỗi.




