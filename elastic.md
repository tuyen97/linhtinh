### Shard

Chia chỉ mục thành các mảnh con gọi là shard.

Cho phép lặp các phần để chịu lỗi, xử lí song song các phần để tăng tốc độ.

Khi được sao lại nhiều lần, mỗi chỉ mục sẽ có các mảnh chính(primary shard) và các mảnh bản sao (replica shard). Số mảnh và bản sao có thể xác định trong lúc đánh chỉ mục, có thể chia lại số mảnh bằng ```_shrink``` và ```_split```.

Mặc định, mỗi chỉ mục được cấp phát 5 mảnh chính và 1 bản sao lưu, tức là sẽ có 5 mảnh chính và 5 mảnh bản sao(1 chỉ mục hoàn chỉnh) tổng cộng là 10 mảnh. 
