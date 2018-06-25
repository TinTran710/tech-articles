[Source](https://www.percona.com/blog/2016/10/12/mysql-5-7-performance-tuning-immediately-after-installation/ "Permalink to MySQL 5.7 Performance Tuning Immediately After Installation")

# Điều chỉnh hiệu năng MySQL 5.7 ngay sau khi cài đặt
Blog này cập nhật từ bài viết [Stephane Combaudon’s blog on MySQL performance tuning][1], và nói về việc điều chỉnh hiệu năng của MySQL 5.7 ngay sau khi cài đặt.

Vài năm trước đây, Stephane Combaudon đã viết một blog nói về [10 cài đặt để điều chỉnh hiệu năng của MySQL sau khi cài đặt ][1]. Bài viết đó viết cho các phiên bản cũ của MySQL như: 5.1, 5.5 và 5.6 (và có thể là phiên bản bây giờ trong tương lai). Trong bài viết này, chúng ta sẽ xem xét cần điều chỉnh những gì trong MYSQL 5.7 (với trọng tâm hướng vào InnoDB).

Một tin tốt đó là MySQL đã có những giá trị mặc định tốt hơn đáng kể. Morgan Tocker đã tạo một [trang với một danh sách đầy đủ các tính năng trong MySQL 5.7][2], và đó cũng là một nguồn tham khảo rất tốt. Ví dụ, các biến sau đây được đặt bởi mặc định:

- innodb_file_per_table=ON
- innodb_stats_on_metadata = OFF
- innodb_buffer_pool_instances = 8 (hoặc 1 nếu innodb_buffer_pool_size < 1GB)
- query_cache_type = 0; query_cache_size = 0; (vô hiệu hóa mutex)


Trong MySQL 5.7. chỉ có duy nhất bốn biến quan trong cần thay đổi. Tuy nhiên, có các biến InnoDB và biến MySQL toàn cục khác có thể cần điều chỉnh cho các khối lượng công việc và phần cứng cụ thể.

Để bắt đầu, thêm các cài đặt sau vào my.cnf, ở dưới phần [mysqld]. Bạn sẽ cần khởi động lại MySQL:

[mysqld] 
- innodb_buffer_pool_size = 1G # (adjust value here, 50%-70% of total RAM)
- innodb_log_file_size = 256M 
- innodb_flush_log_at_trx_commit = 1 # có thể đổi thành 2 hoặc 0 
- innodb_flush_method = O_DIRECT

Mô tả:

| Biến |  Giá trị | 
| ------------- |:-------------:| 
| innodb_buffer_pool_size |  Bắt đầu với 50% 70% tổng RAM. Không nhất thiết phải lớn hơn kích thước cơ sở dữ liệu|  
| innodb_flush_log_at_trx_commit | * 1   (Mặc định) * 0/2 hiệu suất cao hơn, ít tin cậy hơn)|  
| innodb_log_file_size |  128M – 2G (không cần thiết phải lớn hơn vùng đệm) |  
| innodb_flush_method |  O_DIRECT (tránh buffering 2 lần) | 

 

Tiếp theo là gì?

Đó là những xuất phát điểm tốt cho bất kỳ cài đặt mới nào. Bên cạnh đó vẫn còn một số các biến khác có thể cải thiện hiệu năng của MySQL cho các khối lượng công việc khác nhau. Thông thường, chúng ta sẽ thiết lập công cụ giám sát/đồ thị hóa MySQL (ví dụ, [nền tảng giảm sát và quản lý Percona][3]) và sau đó kiểm trang quản lý MySQL để tiếp tục việc điều chỉnh hiệu năng.

Chúng ta có thể điều chỉnh gì thêm từ trên các đồ thị?

Kích thước vùng đệm InnoDB. Hãy xem những đồ thị sau:

![MySQL 5.7 Performance Tuning][4]

![MySQL 5.7 Performance Tuning][5]

Như có thể thấy, chúng ta có thể hưởng lợi từ việc tăng kích thước vùng đệm InnoDB một chút lên thành xấp xỉ ~10G, vì chúng ta có RAM khả dụng và vì số lượng các trang rỗi là nhỏ so với tổng vùng đệm.

Kích thước file log InnoDB. Xem đồ thị sau:

![MySQL 5.7 Performance Tuning][6]


Như có thể thấy, InnoDB thường ghi 2.26 GB dữ liệu mỗi giờ, điều này vượt quá tổng kích thước của các file log (2G). Bây giờ chúng ta có thể tăng biến innodb_log_file_size và khởi động lại MySQL. Một cách khác, sử dụng "hiển thị trạng thái engine InnoDB" để [tính toán kích thước file log InnoDB sao cho hợp lý][7]

Các biến khác

Có một số các biến InnoDB khác bạn có thể điều chỉnh thêm:

innodb_autoinc_lock_mode

Cài đặt [innodb_autoinc_lock_mode][8] = 2 (chế độ interleaved - chèn) có thể sẽ loại bỏ sự phụ thuộc vào khóa AUTO-INC ở mức độ bảng (và có thể gia tăng hiệu năng khi lệnh insert nhiều dòng được sử dụng để thêm các giá trị vào bảng với khóa chính tự tăng). Điều này yêu cầu cài đặt binlog_format=ROW hoặc MIXED (giá trị ROW là mặc định trong MySQL 5.7).

innodb_io_capacity và innodb_io_capacity_max

Đây là cách điều chỉnh tân tiến hơn, nó chỉ có ý nghĩa khi bạn luôn thực hiện việc ghi/chèn quá nhiều (nó không áp dụng cho việc đọc, ví dụ như các lệnh SELECT). Nếu bạn thực sự cần điều chỉnh nó, cách tốt nhất là bạn phải biết có bao nhiêu IOPS mà hệ thống có thể thực hiện. Ví dụ, nếu server có một ổ SSD, chúng ta có thể đặt innodb_io_capacity_max=6000 và innodb_io_capacity=3000 (50% của tối đa). Sẽ là một ý tưởng hay nếu chạy sysbench hay bất cứ công cụ benchmark nào để đánh giá sự lưu thông của ổ đĩa.

Nhưng liệu chúng ta có cần phải lo lắng về cài đặt này? Xem biểu đồ về [những "trang bẩn"][9] của vùng đệm":

![screen-shot-2016-10-03-at-7-19-47-pm][10]


Trong trường hợp này, tổng số lượng các trang bẩn là rất cao, và có vẻ InnoDB không thể duy trì việc xóa chúng. Nếu chúng ta có một hệ thống đĩa đọc nhanh (ví dụ SSD), chúng ta có thể hưởng lợi bằng cách tăng innodb_io_capacity và innodb_io_capacity_max.

Kết luận hoặc phiên bản TL;DR 

Những cài đặt mặc định mới của MySQL 5.7 tốt hơn rất nhiều cho những khối lượng công việc có mục đích cơ bản. Cùng lúc đó, chúng ta vẫn cần cấu hình các biến InnoDB để tận dụng lượng RAM chúng ta có. Sau quá trình cài đặt, thực hiện những bước sau:

1. Thêm các biến InnoDB vào my.cnf(như mô tả bên trên) và khởi động lại MySQL.
2. Cài đặt các hệ thống giám sát, (ví dụ nền tảng giám sát và quản lý Percona).
3. Xem biểu đồ và xác định xem MySQL có cần điều thêm hay không.

[1]: https://www.percona.com/blog/2014/01/28/10-mysql-performance-tuning-settings-after-installation/
[2]: http://www.thecompletelistoffeatures.com/
[3]: http://pmmdemo.percona.com
[4]: https://www.percona.com/blog/wp-content/uploads/2016/10/Screen-Shot-2016-10-03-at-12.49.22-PM.png
[5]: https://www.percona.com/blog/wp-content/uploads/2016/10/Screen-Shot-2016-10-03-at-12.48.13-PM.png
[6]: https://www.percona.com/blog/wp-content/uploads/2016/10/Screen-Shot-2016-10-03-at-12.43.52-PM.png
[7]: https://www.percona.com/blog/2008/11/21/how-to-calculate-a-good-innodb-log-file-size/
[8]: http://dev.mysql.com/doc/refman/5.7/en/innodb-auto-increment-handling.html
[9]: http://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_dirty_page
[10]: https://www.percona.com/blog/wp-content/uploads/2016/10/Screen-Shot-2016-10-03-at-7.19.47-PM.png
[11]: https://secure.gravatar.com/avatar/79877aeedbd68531a30468cd771d5d07?s=84&d=mm&r=g
[12]: https://www.percona.com/blog/author/alexanderrubin/
