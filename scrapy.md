### Spider

Spider là lớp con của lớp ```scrapy.Spider```, có thêm 1 số thuộc tính và phương thức:

- ```name```: tên của spider, là duy nhất trong project

- ```start_requests()```: phải trả về 1 iterable các request- là 1 list hoặc generator các đường dẫn cần cào

- ```parse```: hàm callback được gọi khi có response trả về cho request, chịu trách nhiệm xử lí thông tin và có thể tìm link từ response để cào tiếp theo đồ thị

Cơ chế để chạy theo đồ thị: yield 1 <i>Request</i> mới, Scrapy sẽ tự lập lịch gửi request và đặt 1 hàm callback sẽ chịu trách nhiệm xử lí thông tin khi có response về.

### Items

Để định dạng dữ liệu sau khi được cào về Scrapy cung cấp lớp ```Item```. ```Item``` là các đối tượng bao gói dữ liệu được cào về. Cung cấp các phương thức truy cập dữ liệu dạng ```dict```, có cú pháp định nghĩa thuộc tính đơn giản.

### Item pipeline

Sau khi 1 item được cào về bằng spider, nó được chuyển tới cho Item Pipeline để xử lí thông qua lần lượt các thành phần. 

Mỗi 1 thành phần của item pipeline là 1 lớp python thực thi 1 phương thức. Nó nhận 1 item và thực hiện 1 hành động trên đó, quyết định item này sẽ được đi tiếp qua pipeline hoặc là bị hủy bỏ không được xử lí nữa.

Mỗi thành phần trong itempipeline phải thực thi phương thức 

```python
process_item(self, item, spider)
```

- item(```Item``` hoặc là 1 dict)- item đã được cào về bởi spider

- spider(đối tượng ```spider```): sipder đã cào item kia về.

#### Áp dụng 1 thành phần trong Item Pipeline:

Để áp dụng cần thêm tên lớp của nó vào trong ```ITEM_PIPELINES``` phần setting, ví dụ:

```python
ITEM_PIPELINES = {
    'myproject.pipelines.PricePipeline': 300,
    'myproject.pipelines.JsonWriterPipeline': 800,
}
```

Chỉ số trên là thứ tự thực hiện của các thành phần trong pipeline: chạy lần lượt từ số bé đến số lớn.

