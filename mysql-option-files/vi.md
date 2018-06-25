
[Source](https://dev.mysql.com/doc/refman/5.7/en/option-files.html "Permalink to MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files")

# MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files

### 4.2.6 Sử dụng file cấu hình

Hầu hết các chương trình MySQL có thể đọc các tùy chọn (option) khởi động từ các file tùy chọn (file cấu hình). File cấu hình mang lại một phương pháp tiện lợi trong việc định rõ các cấu hình mà ta thường xuyên sử dụng. Điều này giúp chúng ta không phải nhập các cấu hình ấy vào dòng lệnh mỗi lần khởi chạy một chương trình.

Để xác định xem liệu một chương trình có đọc từ các file cấu hình hay không, hãy chạy nó với cấu hình `\--help`. (Đối với [**mysqld**][1], sử dụng [`\--verbose`][2] and [`\--help`][3].) Nếu chương trình có đọc từ các file cấu hình, kết quả từ cấu hình help cho ta biết về file mà nó đang tìm kiếm và các nhóm cấu hình nó nhận biết được.

Lưu ý 

Một chương trình MySQL bắt đầu với cấu hình `\--no-defaults` sẽ chỉ đọc cấu hình từ file `.mylogin.cnf`.

Nhiều file cấu hình là các file thuần text và được tạo bởi bất kỳ trình soạn thảo nào. Có một ngoại lệ là file `.mylogin.cnf`, nó chứa cấu hình về đường dẫn đăng nhập. Đây là một file mã hóa được tạo bởi tiện ích [**mysql_config_editor**][4]. Một "đường dẫn đăng nhập" ("login path") là một nhóm cấu hình chỉ chấp nhận các cấu hình sau: `host`, `user`, `password`, `port` và `socket`. Chương trình client sử dụng cấu hình [`\--login-path`][5] để xác định xem sẽ đọc đường dẫn đăng nhập nào từ `.mylogin.cnf`.

Để định rõ một tên file đường dẫn đăng nhập thay thế, khai báo biến môi trường `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng bởi tiện ích kiểm thử **mysql-test-run.pl**, nhưng cũng được nhận biết bởi [**mysql_config_editor**][4] và các MySQL client như [**mysql**][6], [**mysqladmin**][7], ...

MySQL sẽ tìm các file cấu hình theo thứ tự được mô tả như dưới và đọc tất cả các file có tồn tại. Nếu một file cấu hình bạn muốn dùng không tồn tại, hãy tạo nó theo phương pháp thích hợp như đã được đề cập.
    
Trên Windows, các chương trình MySQL đọc các cấu hình khởi động từ các file như trong bảng dưới đây theo đúng thự tự (file được liệt kê trước sẽ đọc trước, các file theo sau được đọc lần lượt).

**Bảng 4.1 Các file cấu hình được đọc trên hệ thống Windows**

| Tên file                                                                                  | Ý nghĩa                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | Tùy chọn toàn cục                                                    |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | Tùy chọn toàn cục                                                    |  
| `C:my.ini`, `C:my.cnf`                                                                     | Tùy chọn toàn cục                                                    |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | Tùy chọn toàn cục                                                    |  
| `defaults-extra-file`                                                                      | File được chỉ định bởi [`\--defaults-extra-file`][8], nếu tồn tại |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | cấu hình đường dẫn đăng nhập (chỉ cho client)                             |  


Theo bảng trên, `%PROGRAMDATA%` biểu thị thư mục file hệ thống mà chứa dữ liệu ứng dụng cho tất cả các users trên host. Đường dẫn mặc định của nó là `C:ProgramData` trong các phiên bản từ Microsoft Windows Vista trở đi và `C:Documents and SettingsAll UsersApplication Data` trong các phiên bản cũ hơn của Microsoft Windows.

`%WINDIR%` biểu thị vị trí của thư mục Windows của bạn. Thường sẽ là `C:WINDOWS`. Sử dụng câu lệnh sau để xác định vị trí chính xác của nó từ giá trị của biến môi trường `WINDIR`: 
    
    
    C:> echo %WINDIR%
 
`%APPDATA%` biểu thị giá trị của thư mục dữ liệu ứng dụng của Windows. Sử dụng câu lệnh sau để xác định vị trí chính xác của nó từ giá trị của biến môi trường `APPDATA`:
    
    
    C:> echo %APPDATA%

_`BASEDIR`_ biểu thị thư mục cài đặt gốc của MySQL. Nếu MySQL 5.7 được cài đặt bằng MySQL Installer, giá trị của nó thường sẽ là `C:_`PROGRAMDIR`_MySQLMySQL 5.7 Server` với _`PROGRAMDIR`_ biểu thị thư mục của các chương trình (thường là `Program Files` trên phiên bản Windows sử dụng tiếng Anh), xem thêm tại [Section 2.3.3, "MySQL Installer for Windows"][9].

Trên hệ thống Unix hoặc giống với Unix, các chương trình MySQL đọc các cấu hình khởi động từ các file được trình bày trong bảng dưới đây, theo thứ tự các file được liệt kê trước sẽ đọc trước, các file theo sau được đọc lần lượt.

Lưu ý 

Trên nền tảng Unix, MySQL bỏ qua các file cấu hình mà ai cũng có thể ghi (world-writable). Điều này là chủ đích vì đây là một biện pháp an ninh.

**Bảng 4.2 Các file cấu hình được đọc trên hệ thống Unix và hệ thống giống Unix**

| Tên file                | Ý nghĩa                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Tùy chọn toàn cục                                             |  
| `/etc/mysql/my.cnf`     | Tùy chọn toàn cục                                             |  
| `_`SYSCONFDIR`_/my.cnf` | Tùy chọn toàn cục                                             |  
| `$MYSQL_HOME/my.cnf`    | Cấu hình cho đặc tả của server                                |  
| `defaults-extra-file`   | File được định rõ bởi[`\--defaults-extra-file`][8]nếu tồn tại |  
| `~/.my.cnf`             | Cấu hình cho đặc tả của user                                  |  
| `~/.mylogin.cnf`        | Cấu hình đường dẫn đăng nhập đặc tả của user                  |  

Theo bảng trên, `~` biểu thị thư mục home của user hiện tại (giá trị của `$HOME`).
 
_`SYSCONFDIR`_ biểu thị thư mục được định rõ với cấu hình [`SYSCONFDIR`][10] tới **CMake** khi MySQL được tạo. Theo mặc định, đây là thư mục `etc` nằm trong thư mục cài đặt được biên dịch.

`MYSQL_HOME` là biến môi trường chứa đường dẫn tới thư mục mà tại đó file đặc tả server `my.cnf` tồn tại. Nếu `MYSQL_HOME` không được cài đặt và bạn khởi động server sử dụng chương trình [**mysqld_safe**][11], [**mysqld_safe**][11] gán nó cho _`BASEDIR`_, thư mục cài đặt gốc của MySQL.

_`DATADIR`_ thường có giá trị là `/usr/local/mysql/data`, dù giá trị này có thể khác đối với các nền tảng hoặc với các phương pháp cài đặt khác nhau. Giá trị là đường dẫn thư mục khi MySQL được biên dịch, chứ không phải là đường dẫn được định rõ với cấu hình [`\--datadir`][12] khi [**mysqld**][1] khởi chạy. Sử dụng [`\--datadir`][12] lúc runtime sẽ không có tác dụng tại nơi mà server tìm kiếm các file cấu hình để đọc trước khi nó xử lý bất kỳ cấu hình nào. 

Nếu nhiều thực thể của một cấu hình được tìm thấy, thực thể cuối cùng sẽ được ưu tiên với một ngoại lệ: Với **[mysqld**][1], thực thể _đầu tiên_ của cấu hình `[\--user`][13] sẽ được sử dụng như một biện pháp an ninh phòng ngừa để ngăn chặn người dùng được chỉ định trong một file cấu hình nào đó bị ghi đề trên dòng lệnh.

Mô tả về cú pháp của file cấu hình sau đây áp dụng với những file mà bạn chỉnh sửa thủ công. Điều này không áp dụng với `.mylogin.cnf` vì nó được tạo bởi **[mysql_config_editor**][4] và đã được mã hóa.

Bất kỳ cấu hình nào được đưa trên dòng lệnh khi đang chạy một chương trình MySQL cũng có thể đưa được vào file cấu hình. Để lấy ra danh sách các cấu hình có thể sử dụng cho một chương trình, hãy chạy nó với cấu hình `\--help`. (Với **[mysqld**][1], sử dụng `[\--verbose`][2] và `[\--help`][3].)

Cú pháp để định rõ các cấu hình trong một file cấu hình cũng giống với cú pháp dòng lệnh (xem [Section 4.2.4, "Using Options on the Command Line"][14]). Tuy nhiên, trong một file cấu hình, bạn sẽ bỏ đi hai dấu gạch ngang khỏi tên của cấu hình và bạn sẽ định rõ từng cấu hình trên từng dòng. Ví dụ, `\--quick` và `\--host=localhost` đối với dòng lệnh thì trong file cấu hình chúng cần được định rõ là `quick` và `host=localhost` trên các dòng riêng biệt. Để định rõ một cấu hình của kiểu `\--loose-_`opt_name`_` trong file cấu hình, hãy viết thành `loose-_`opt_name`_`.

Các dòng trống trong file cấu hình sẽ được bỏ qua. Các dòng không trống sẽ tuân theo một trong các kiểu sau:

* `#_`bình luận`_`, `;_`bình luận`_`

Các dòng bình luận bắt đầu bằng `#` hoặc `;`. Một bình luận `#` cũng có thể bắt đầu ở giữa một dòng. 

* `[_`nhóm`_]`

_`nhóm`_ là tên của chương trình hoặc nhóm mà bạn muốn cấu hình. Sau dòng khai báo của một nhóm, các dòng cấu hình tiếp theo sẽ được áp dụng cho tên nhóm đó cho tới khi kết thúc file cấu hình hoặc một nhóm khác được khai báo. Tên của của nhóm cấu hình không ảnh hưởng bởi chữ viết hoa hay viết thường.

* `_`tên_cấu hình`_`

Điều này tương đương với `\--_`tên_cấu hình`_` trên dòng lệnh.

* `_`tên_cấu hình`_=_`giá trị`_`

Điều này tương đương với `\--_`tên_cấu hình`_=_`giá trị`_` trên dòng lệnh. Trong file cấu hình, bạn có thể có dấu cách xung quanh ký tự `=`, điều này vốn không đúng khi làm việc trên dòng lệnh. Ngoài ra, phần giá trị có thể tùy ý được đặt trong cặp nháy đơn hoặc nháy kép, điều này sẽ có ích nếu giá trị ta làm việc có chứa ký tự bình luận `#`.

Dấu cách ở đầu hoặc cuối sẽ được tự động xóa khỏi tên và giá trị cấu hình.

Bạn có thể sử dụng chuỗi kết thúc `b`, `t`, `n`, `r`, `\`, và `s` trong giá trị của cấu hình để biểu thị cho dấu xóa, tab, xuống dòng, enter, gạch chéo và dấu cách. Trong file cấu hình, quy tắc của chuỗi kết thúc được áp dụng như sau:

* Theo sao dấu gạch chéo là một ký tự kết thúc hợp lệ, chúng sẽ được đổi thành ký tự biểu thị bởi chuỗi. Ví dụ, `\s` sẽ được biến thành dấu cách.
* Theo sau dấu gạch chéo không phải là một ký tự kết thúc hợp lệ thì chúng sẽ được giữ nguyên. Ví dụ, `S` sẽ được giữ nguyên.

Luật phía trước có nghĩa rằng dấu gạch chéo có nghĩa có thể được viết thành `\\` hoặc `\` nếu theo sau nó không phải là một ký tự kết thúc hợp lệ.

Quy tắc cho chuỗi kết thúc trong file cấu hình có thể khác một chút so với quy tắc trong các câu lệnh SQL. Trong ngữ cảnh sau, nếu "_`x`_" không phải là một ký tự kết thúc hợp lệ, `_`\x`_` trở thành "_`x`_" chứ không phải là `_`\x`_`. Xem [Section 9.1.1, "String Literals"][15]. 

Quy tắc kết thúc cho file cấu hình rất thích hợp với tên đường dẫn trong Windows khi nó sử dụng `\` làm ký tự chia cách giữa các tên đường dẫn. Ký tự chia cách trong tên đường dẫn của Windows phải được viết thành `\\` nếu nó được theo sau bởi ký tự kết thúc. Nó có thể được viết thành `\\` hoặc `\` nếu nó không phải. Tuy nhiên, `/` cũng có thể được sử dụng trong đường dẫn Windows và được coi như là `\`. Giả sử bạn muốn định rõ một thư mục gốc `C:Program FilesMySQLMySQL Server 5.7` trong file cấu hình. Điều này có thể đạt được theo nhiều cách. Ví dụ: 
    
    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu tên một nhóm cấu hình giống với tên chương trình, các cấu hình trong nhóm sẽ áp dụng riêng cho chương trình đó. Ví dụ, nhóm `[mysqld]` và `[mysql]` áp dụng cho **[mysqld**][1] server và chương trình **[mysql**][6] client tương ứng. 

Nhóm `[client]` được đọc bởi tất cả các chương trình client có trong bản phân phối của MySQL (ngoại trừ **[mysqld**][1]). Để hiểu cách các chương trình client bên thứ 3 mà sử dụng C API có thể sử dụng file cấu hình, tham khảo tài liệu C API tại [Section 27.8.7.50, "mysql_options()"][16]. 

Nhóm `[client]` cho phép bạn định rõ các cấu hình bạn muốn áp dụng cho tất cả các clients. Ví dụ, `[client]` là nhóm phù hợp để sử dụng để định rõ mật khẩu dùng trong việc kết nối tới server. (Hãy đảm bảo rằng file cấu hình chỉ có thể truy cập được bởi bạn, nhờ đó người khác sẽ không thể tìm ra mật khẩu của bạn.) Hãy đảm bảo bạn không chèn cấu hình nào vào trong nhóm `[client]` trừ khi nó được nhận biết bởi _tất cả_ các chương trình client mà bạn sử dụng. Các chương trình không hiểu cấu hình sẽ kết thúc sau khi hiển thị thông báo lỗi nếu bạn cố chạy chúng.

Liệt kê các nhóm cấu hình có tính khái quát trước và các nhóm có tính chi tiết sau. Ví dụ, một nhóm `[client]` có tính khái quát hơn vì nó được đọc bởi tất cả các chương trình client, trong khi đó nhóm `[mysqldump]` chỉ được đọc bởi **[mysqldump**][17]. Cấu hình được định rõ sau này sẽ ghi đè lên các cấu hình trước đó, vì thế đặt các nhóm cấu hình theo thứ tự `[client]`, `[mysqldump]` sẽ cho phép cấu hình chi tiết **[mysqldump**][17] ghi đè lên cấu hình `[client]`.


Đây là file cấu hình toàn cục tiêu biểu: 
    
    
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    port=3306
    socket=/tmp/mysql.sock
    key_buffer_size=16M
    max_allowed_packet=8M
    
    [mysqldump]
    quick

Đây là một file cấu hình người dùng tiêu biểu: 
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

Để tạo nhóm cấu hình được đọc bởi mỗi **[mysqld**][1] servers từ bản phát hành MySQL cụ thể nào đó, sử dụng nhóm với tên của `[mysqld-5.6]`, `[mysqld-5.7]` v.v. Các nhóm sau biểu thị rằng cài đặt `[sql_mode`][18] được sử dụng chỉ bởi MySQL servers với số phiên bản 5.7.x:
    
    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Ta có thể sử dụng chỉ thị `!include` trong file cấu hình để bao hàm các file cấu hình khác và `!includedir` để tìm kiếm thư mục cụ thể cho file cấu hình. Ví dụ, để bao hàm file `/home/mydir/myopt.cnf`, sử dụng chỉ thị sau: 
    
    
    !include /home/mydir/myopt.cnf

Để tìm thư mục `/home/mydir` và đọc file cấu hình tìm thấy tại đó, sử dụng chỉ thị:
    
    
    !includedir /home/mydir

MySQL không đảm bảo về thứ tự mà file cấu hình được đọc trong thư mục.

Lưu ý 

Bất kể file nào được tìm thấy và được bao hàm bằng cách sử dụng chỉ thị `!includedir` trên hệ điều hành Unix _phải_ có tên file kết thúc với đuôi `.cnf`. Trên Windows, chỉ thị này kiểm tra các file với đuôi `.ini` hoặc `.cnf`.

Viết nội dung của file cấu hình được bao hàm giống như những file cấu hình khác. Đó là, chúng cần chứa các nhóm cấu hình, mỗi nhóm có dòng `[_`nhóm`_]` đặt ở trước biểu thị rằng chương trình nào áp dụng cấu hình này.

Trong khi một file được bao hàm đang được xử lý, chỉ những cấu hình trong nhóm mà chương trình hiện tại tìm kiếm sẽ được sử dụng. Các nhóm khác sẽ được lờ đi. Với giả thiết rằng file `my.cnf` chứa dòng sau:
    
    
    !include /home/mydir/myopt.cnf

Và giả sử rằng `/home/mydir/myopt.cnf` trông sẽ như sau: 
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu `my.cnf` được xử lý bởi **[mysqld**][1], chỉ nhóm `[mysqld]` trong `/home/mydir/myopt.cnf` sẽ được sử dụng. Nếu file được xử lý bởi **[mysqladmin**][7], chỉ nhóm `[mysqladmin]` sẽ được sử dụng. Nếu file được xử lý bởi chương trình nào khác, không cấu hình nào trong `/home/mydir/myopt.cnf` được sử dụng.

Chỉ thị `!includedir` được xử lý tương tự ngoại trừ việc tất cả các file cấu hình trong thư mục sẽ được đọc.

Nếu một file cấu hình chứa chỉ thị `!include` hoặc `!includedir`, các file được đặt tên bởi chỉ thị được xử lý bất cứ khi nào file cấu hình được xử lý, không kể chúng xuất hiện ở đâu trong file.
  
