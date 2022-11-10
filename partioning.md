## Các chiến lược partition
- Round-robin
- Hash partitioning
- Range partioning
## So sánh các kiểu partition
### Round-robin:
Phù hợp với scan toàn bộ dữ liệu trong mỗi lần truy vấn. Việc tìm kiếm phải thực hiện trên tất cả các node.
### Hash partitioning
Phù hợp nhất cho point query, câu truy vấn được gửi tới 1 node duy nhất chứa dữ liệu, các node khác có thể thực hiện các truy vấn khác.
### Range partitioning 
Phù hợp cho cả point lẫn range query, phạm vi tìm kiếm được thu hẹp trong 1 vài node.
Tuy nhiên, có thể gặp phải vấn đề mất cân bằng khi 1 node giữ quá nhiều data hoặc là truy vấn chỉ tập trung vào 1 khoảng nhất định.

## Giải pháp với tình trạng lệch partition

Partition đc map tới virtual node, virtual node sau đó mới đc map tới real node.
Khi 1 partition trở lên quá lớn, nó sẽ được tách ra làm 2 phần và đc map tới virtual node mới.

## Index partition

Để tránh query được thực hiện trên tất cả các node khi truy vấn theo các trường ko phải là partition key.

Query được thu hẹp lại phạm vi thực hiện tới vài node bằng cách thực hiện việc tìm kiếm giá trị partition key trc.
