### Đặc tả tên phiên bản của phần mềm (Semantic Versioning)
Những từ khóa như “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, và “OPTIONAL” trong tài liệu này đã được giải thích và mô tả trong RFC 2119.

1. Phần mềm sử dụng Semantic Versioning phải khai báo 1 API công khai. API này nên được khai báo ngay trong code của nó hoặc tồn tại chặt chẽ trong tài liệu. Cho dù được làm như thế nào, nó cần đảm bảo được tính chính xác và toàn diện.

2. Một số phiên bản thông thường phải tuân thủ theo dạng X.Y.Z với X, Y, và Z là các số không âm, và không được bắt đầu bởi các số 0. X là phiên bản chính, Y là phiên bản phụ, và Z là phiên bản vá. Mỗi phẩn tử phải là các số có tính tăng dần. Ví dụ: 1.9.0 -> 1.10.0 -> 1.11.0.

3. Khi một gói phiên bản được phát hành, nội dung của phiên bản đó không được phép sửa đổi. Mọi chỉnh sửa phải được phát hành ở một phiên bản mới.

4. Phiên bản chính với số 0 (0.y.z) là phiên bản đánh dấu sự bắt đầu của việc phát triển sản phẩm. Mọi thứ có thể thay đổi bất cứ lúc nào. Không nên coi API công khai là thứ ổn định. 

5. Phiên bản 1.0.0 định nghĩa 1 API công khai. Cái cách mà số của phiên bản được tăng sau mỗi lần phát hành phụ thuộc vào chính API công khai này và cách nó được sửa đổi ra sao.

6. Phiên bản vá Z(x.y.z | x > 0) phải được tăng lên khi 1 bản sửa lỗi tương thích với các bản trước đó được giới thiệu. Một sửa lỗi được định nghĩa là một sự thay đổi bên trong và nó đã sửa những hành vi không chính xác.

7. Phiên bản phụ Y (x.Y.z | x > 0) phải được tăng, nếu có một chức năng mới nhưng vẫn tương thích với các phiên bản trước đó được giới thiệu trong API công khai. Nó phải được tăng nếu có bất kỳ chức năng nào của API công khai được đánh dấu là không tán thành. Nó có thể tăng nếu 1 chức năng hay cải tiển mới và quan trọng được giới thiệu trong code riêng tư. Nó cũng có thể bao gồm các thay đổi ở mức vá. Phiên bản vá phải được đặt lại về 0 khi phiên bản phụ được tăng.

8. Phiên bản chính X (X.y.z | X > 0) phải được tăng nếu có  bất kỳ sự thay đối nào tạo ra sự không tương thích với các phiên bản trước đó mà được giới thiệu trong API công khai. Nó có thể bao gồm các thay đổi mức độ nhỏ và vá. Phiên bản vá và phụ phải được đặt lại về 0 khi phiên bản chính được tăng.

9. Phiên bản trước khi phát hành có thể được ký hiệu bằng cách thêm vào các dấu gạch ngang (-) và một loạt các dấu chấm chia cách các định danh theo ngay sau phiên bản vá. Các định danh phải bao gồm các ký tự và gạch nối ASCII [0-9A-Za-z-]. Các định danh không được phép rỗng. Các định danh số không được phép bắt đầu bởi các số 0. Các phiên bản trước khi phát hành có ưu tiên thấp hơn so với phiên bản liên kết bình thường. Một phiên bản trước khi phát hành biểu thị rằng phiên bản đó không ổn định và có thể không thỏa mãn các yêu cầu tương thích như dự định của các phiên bản liên kết bình thường. Ví dụ: 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92.

10. Siêu dữ liệu được xây dựng có thể được biểu thị bằng cách thêm các dấu cộng và một loạt các dấu chấm chia cách các định danh và theo ngay sau đó là các phiên bản bản vá hoặc phiên bản trước khi phát hành. Các định danh phải bao gồm chỉ các ký tự và gạch nối ASCII [0-9A-Za-z-]. Các định danh không được phép rỗng. Siêu dữ liệu xây dựng nên được loại bỏ khi xác định phiên bản ưu tiên. Vì thế nếu hai phiên bản chỉ khác nhau ở siêu dữ liệu xây dựng sẽ có cùng độ ưu tiên. Ví dụ: 1.0.0-alpha+001, 1.0.0+20130313144700, 1.0.0-beta+exp.sha.5114f85.

11. Độ ưu tiên dùng để so sánh các phiên bản với nhau khi sắp xếp. Độ ưu tiên phải được tính toán bằng cách chia phiên bản thành các định danh chính, phụ, vá và tiền phát hành theo thứ tự. (siêu dữ liệu xây dựng không được tính vào độ ưu tiên)
Độ ưu tiên được xác định bằng sự khác nhau đầu tiên khi so sánh mỗi định dang từ trái qua phải như sau: Các phiên bản chính, phụ và vá luôn được so sánh bằng số. Ví dụ  1.0.0 < 2.0.0 < 2.1.0 < 2.1.1. Khi bản chính, phụ và vá bằng nhau, phiên bản tiền phát hành có độ ưu tiên thấp hơn phiên bản bình thường. Ví dụ 1.0.0-alpha < 1.0.0. Độ ưu tiên cho 2 phiên bản tiền phát hành với cùng phiên bản chính, phụ và vá phải được xác định bằng cách so sánh từng dấu chấm chia cách các định danh từ trái quá phải cho đến khi có 1 sự các biệt được tìm thấy như sau : các định danh chỉ bao gồm các chữ số được so sánh một cách số học và các định danh với các chữ và các dấu nối được so sánh theo cách từ vựng dựa trên thứ tự sắp xếp ASCII. Các định danh số học luôn có độ ưu tiên thấp hơn các định danh không phải số. Một tập các trường tiền phát hành lớn có độ ưu tiên cao hơn các tập nhỏ nếu tất cả các định danh phía trước là bằng nhau. Ví dụ : 1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0.

### Tại sao phải sử dụng Semantic Versioning?
Đây không phải là một phát minh hay đột phá nào cả. Trên thực tế, bạn có thể đã thực hiện điều gì gần giống như vậy rồi. Vấn đề là "gần giống" ở đây không đủ tốt. Nếu không tuân thủ theo một tập các đặc điểm kỹ thuật chính thống, các số phiên bản thực chất sẽ vô nghĩa cho việc quản lý các phụ thuộc (dependency). Bằng cách đưa các định nghĩa rõ ràng cho các ý tưởng trên, nó trở nên dễ dàng trong việc truyền tải các mục đích của bạn tới các người dùng phần mềm. Ngay khi những mục tiêu được định nghĩa rõ ràng, các đặc tả về phụ thuộc sẽ được tạo ra một cách linh động (tất nhiên không được quá linh động).

Một ví dụ đơn giản sẽ chứng minh Semantic Versioning giúp chúng ta quản lý các phụ thuộc (dependency) tốt hơn nhiều so với trước đây. Xem xét một thư viện tên là "Firetruck". Nó cần sử dụng package đã được đánh tên phiên bản một cách chuẩn hóa với tên gọi là "Ladder". Tại thời điểm Firetruck được tạo ra, Ladder lúc đó đang ở phiên bản 3.1.0. Vì Firetruck sử dụng một vài chức năng được giới thiệu lần đầu trong phiên bản 3.1.0, bạn có thể yên tâm ghi rằng Firetruck của ta có thể sử dụng phụ thuộc Ladder từ phiên bản 3.1.0 cho đến trước phiên bản 4.0.0. Bây giờ khi Ladder đang ở phiên bản 3.1.1 và 3.2.0 đã khả dụng, bạn có thể khai báo chúng tới trình quản lý gói (package) và yên tâm rằng chúng sẽ tương thích với các phụ thuộc của phần mềm hiện tại.

Với tư cách là một lập trình viên có trách nhiệm, bạn sẽ muốn xác thực rằng bất kỳ sự nâng cấp nào đối với gói, nó sẽ hoạt động đúng như những gì đã quảng cáo. Thế giới là một nơi phức tạp, bạn sẽ không thể làm gì nhiều ngoài việc trở nên thận trọng. Những gì bạn có thể làm là để Semantic Versioning mang đến cho bạn cách để phát hành và nâng cấp sản phẩm một cách chuẩn mực mà tiết kiệm được thời gian và sự rắc rối.

Điều này có vẻ nghe thật lý tưởng, tất cả những gì bạn cần làm là bắt đầu sử dụng Semantic Versioning bằng cách tuyên bố rằng bạn đang làm vậy và sau đó hãy thực hiện theo các luật được nêu trên. Hãy liên kết trang web này từ README cuả bạn để những người khác cũng biết được quy luật này và có thể sử dụng chúng.

