# Mục lục

- [1.Index API](#1)

- [2.Index Template](#2)

- [3.Mapping](#3)

- [4.Tạo index trong ES](#4)

<a name="1"></a>
## Index API

Index API thêm hoặc sửa 1 đối tượng JSON(gọi là document) vào trong 1 index, ví dụ thêm 1 văn bản vào index:

```python
PUT twitter/_doc/1
{
    "user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch"
}
```

Index sẽ được tự động tạo nếu chưa có sẵn, theo 1 index template có sẵn hoặc mặc định. Dynamic mapping cũng được tự động tạo khi thêm văn bản vào trong index.

<a name="2"></a>
## Index Template 

Index template cho phép cấu hình cả settings và mapping được áp dụng vào tất cả các văn bản trong index. 

[Setting](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html#index-modules-settings) là các cấu hình xác định cách thức lưu trữ dữ liệu: số bản sao(replica), kiểu nén dữ liệu(LZ4, DEFLATE), ..

[Mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html) xác định cấu trúc văn bản được lưu và đánh chỉ mục(gồm nhiều trường).

<a name = "3"></a>
### Mapping 

Mỗi index có 1 <i>mapping type</i> để xác định cách tạo ra index từ văn bản.

1 mapping type gồm 2 phẩn:

- Meta-fields: dữ liệu ngoài văn bản được đánh chỉ mục: _index, _id, _type,...

- Fields/properties: các trường ứng với các phẩn trong văn bản JSON.

Kiểu dữ liệu của Fields:

- Kiểu đơn giản: text, keyword, long, double...

- Kiểu dữ liệu nested như object hoặc json.

- Kiểu đặc biệt như geo-point, geo_shape.

Kiểu text được sử dụng cho full-text search, được tách token và index. Kiểu keyword phù hợp cho tìm kiếm chính xác giá trị như id, email, hostname, tag....

Ví dụ về 1 mapping:

<a name="create_mapping"></a>
```python
PUT my_index
{
  "mappings": {
    "properties": {
      "tags": {
        "type":  "keyword"
      }
    }
  }
}
```

Lệnh trên thực hiện tạo index tên là my_index nếu chưa có sẵn, với 1 trường tên là tags có kiểu là keyword. Các văn bản trong index này sẽ có trường tags, khi tìm kiếm trên tags ta phải đưa chính xác giá trị ví dụ:

<a name="search"></a>
```python
GET /my_index/_search
{
    "query" : {
        "match" : { "tags" : "kimchy" }
    }
}
```

## Tạo index trong ES

- Xác định [mapping](#create_mapping) cho index

- [Thêm](#1) văn bản vào index 

- [Tìm kiếm](#search)



