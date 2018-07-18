#  Điều gì làm nên một trang web Mobile tốt? | Cơ bản về Web | Google Developers

![Jenny Gove](https://developers.google.com/web/images/contributors/jennygove.jpg)

**Bởi** [ Jenny Gove ](https://developers.google.com/web/resources/contributors/jennygove)

Jenny Gove là một trưởng nhóm nghiên cứu UX tại Google, nơi bà tiến hành nghiên cứu về  trải nghiệm điện thoại thông minh. Bà nhận bằng tiến sĩ từ Đại học Southampton, Vương quốc Anh.

Google và AnswerLab đã tiến hành một [nghiên cứu](https://www.google.com/think/multiscreen/whitepaper-sitedesign.html?utm_source=web-fundamentals&utm_term=chrome&utm_content=ux-landing&utm_campaign=web-fundamentals) để trả lời cho câu hỏi này.

> Người dùng di động có định hướng về mục tiêu. Họ mong đợi có thể có được những gì họ cần, ngay lập tức, và theo mong muốn riêng của họ.

Nghiên cứu của tổ chức được thực hiện trong 119 giờ đồng hồ, với các cuộc thử nghiệm từ những người tham gia trực tiếp ở Mỹ. Những người tham gia được yêu cầu thực hiện các nhiệm vụ chính trên một tập hợp các trang web mobile đa dạng. Bao gồm cả những người dùng IOS và Android, và những người dùng thử nghiệm các trang web trên chính điện thoại của chính họ. Đối với mỗi trang web, người tham gia sẽ được hỏi để nói lên suy nghĩ của họ khi họ hoàn thành các công việc liên quan đến giao tiếp như một giao dịch hay đặt chỗ trước.

Nghiên cứu đã khám phá ra 25 nguyên lý thiết kế trang web mobile, được nhóm thành 5 chủ đề.

## Trang chủ và trang điều hướng.

**Thành công:** Tập trung vào trang chủ trên mobile của bạn để kết nối với người dùng với nội dung họ đang tìm kiếm.

### Giữ các lời gọi hành động ở trước và giữa 

Thưc hiện các tác vụ có sẵn thông qua [menus](https://developers.google.com/web/fundamentals/design-and-ux/responsive/) hoặc “dưới màn hình đầu tiên” (một phần của trang web không thể xem được mà không cần cuộc xuống).

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/hpnav-cta-good.png) 
**Nên Làm**: Tất cả các tác vụ phổ biến nhất của người dùng được thực hiện một cách dễ dàng.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/hpnav-cta-bad.png) 
**Không làm**: Lãng phí không gian quý giá trên màn hình đầu tiên với những lời gọi hành động không rõ ràng như "tìm hiểu thêm".

### Giữ cho các menu ngắn và hấp dẫn.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/hpnav-menus-good.png) 
**Nên Làm**: Giữ cho các menu ngắn và hấp dẫn

Người dùng thiết bị di động không có đủ kiên nhẫn để cuộn qua các danh sách dài tuỳ chọn để tìm điều họ muốn. Sắp xếp lại menu của bạn để sử dụng với ít mục nhất có thể, mà không phải hy sinh khả năng sử dụng.

### Hãy làm dễ dàng việc quay lại trang chủ

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/hpnav-hp-good.png) 
**Nên làm**: Hãy làm dễ dàng việc quay lại trang chủ

Người dùng mong muốn quay lại trang chủ khi họ chạm vào logo trên cùng bên trái của một trang mobile, và họ trở nên thất vọng khi nó không có sẵn hoặc không hoạt động.

### Đừng để quảng cáo chiếm mất phần hiển thị chính.

Các quảng cáo xen kẽ cỡ lớn mà yêu cầu cài đặt app (ví dụ: Quảng cáo tràn ra khỏi trang sẽ làm ẩn đi nội dung và nhắc người dùng cài đặt một ứng dụng) sẽ làm phiền người dùng và khiến việc thực hiện các tác vụ trở nên khó khăn. Ngoài gây phiền nhiễu cho người dùng, [các trang web sử dụng các quảng cáo xen kẽ có thể thấy tác động tiêu cực đến thứ hạng tìm kiếm của họ](https://webmasters.googleblog.com/2016/08/helping-users-easily-access-content-on.html).

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/hpnav-promo-good.png) 
**Nên làm**: Quảng cáo phải dễ dàng được loại bỏ và không làm sao lãng trải nghiệm.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/hpnav-promo-bad.png) 
**Không làm**: Quảng cáo xen kẽ (Đôi khi gọi là sập cửa) thường kiến làm phiền người dùng và làm cho việc sử dụng trang web trở nên không tốt.

## Trang tìm kiếm

**Thành công:** Giúp người dùng thiết bị di động tìm thấy những gì họ tìm kiếm một cách nhanh chóng.

### Hiển thị thanh tìm kiếm trang web 

Người dùng tìm kiếm thông tin luôn luôn mở thanh tìm kiếm, vì vậy thanh tìm kiếm nên là thứ đầu tiên họ thấy trên các trang của bạn. Đừng ẩn thanh tìm kiếm trong menu.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/ss-search-good.jpg) 
**Nên làm**: Hiển thị thanh tìm kiếm

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/ss-search-bad.jpg) 
**Không làm**: Ẩn thanh tìm kiếm ông các menu tràn ra

### Đảm bảo kết quả tìm kiếm có liên quan

Người dùng không muốn phải duyệt qua nhiều trang kết qủa để tìm kiếm điều mà họ đang tìm. Giúp người dùng dễ dàng hơn bằng các câu truy vấn gợi ý, sửa lỗi chính tả, và gợi ý các truy vấn liên quan. Thay vì làm lại những gì đã có, hãy xem xét các sản phẩm mạnh mẽ như [Google Custom Search](https://cse.google.com/cse/).

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/ss-relevant-good.png) 
**Nên làm**: Macy chỉ trả về các sản phẩn trẻ em.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/ss-relevant-bad.png) 
**Không làm**: Trả về kết quả cho bất kì thứ gì có từ kid trong đó.

### Triển khai bộ lọc để thu hẹp kết quả.

Các người tham gia nghiên cứu dựa vào các [bộ lọc](https://developers.google.com/custom-search/docs/structured_search) để tìm kiếm điều mà họ đang tìm, và từ bỏ các trang web không có bộ lọc hiểu quả. Đặt bộ lọc ở phía trên kết quả tìm kiếm và giúp người dùng hiển thị số lượng kết quả sẽ được trả lại khi một bộ lọc được áp dụng.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/ss-filters-good.jpg) 
**Nên làm**: Tạo bộ lọc dễ dàng hơn.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/ss-filters-bad.jpg) 
**Không làm**: Ẩn chức năng lọc.

### Hướng dẫn người dùng để có kết quả tìm kiếm trên trang web tốt hơn.

Đối với các trang web có phân đoạn khách hàng đa dạng, hãy hỏi một số câu hỏi trước khi bắt đầu tìm kiếm và sử dụng câu trả lời của khách hàng làm câu truy vấn tìm kiếm để đảm bảo rằng người dùng lấy được các kết quả từ phân đoạn có liên quan nhất.

![Zappos guides users by asking them what they're looking for.](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/ss-guide-good.png) 
**Nên làm**: Giúp người dùng tìm kiếm điều mà họ đang tìm bằng cách hướng dẫn họ đi đúng hướng.

## Thương mại và chuyển đổi.

**Thành công:** Hiểu được hành trình của khách hàng và để người dùng chuyển thành các điều khoản của riêng họ.

### Để người dùng khám phá trước khi họ cam kết.

Người tham gia nghiên cứu thất vọng bởi các trang web yêu cầu đăng kí trả trước để xem trang, đặc biệt với những trang không quá quen thuộc. Mặc dù thông tin khách hàng có thể là không tách rời với doanh nghiệp của bạn, yều cầu quá sớm có thể dẫn đến ít đăng kí hơn.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/cc-gates-good.png) 
**Nên làm**: Cho phép người dùng xem trang web mà không yêu cầu đăng nhập.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/cc-gates-bad.png) 
**Không làm**: Đặt đăng nhập hoặc đăng kí quá sớm trong một trang web

### Để người dùng mua với tư cách khách hàng

Người tham gia nghiên cứu đã xem các đánh giá của khách hàng là "thuận tiện", "đơn giản", "dễ  dàng" và "nhanh chóng". Người dùng cảm thấy khó chịu bởi các trang web buộc họ đăng kí tài khoản khi mua hàng, đặc biệt khi lợi ích tài khoản là không rõ ràng. 

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/cc-purchase-guest-good.png) 
**Nên làm**: Cho phép người dùng mua với tài khoản khách hàng


### Sử dụng thông tin hiện có để tối đa hoá sự tiện lợi

Ghi nhớ và [điền trước các tuy chọn](https://developers.google.com/web/fundamentals/design-and-ux/input/forms/#label-and-name-inputs-properly) cho người dùng đăng kí. Đề xuất dịch vụ quen thuộc,dịch vụ thanh toán của bên thứ ba ho người dùng mới.

### Sử dụng các nút "click" để gọi cho các công việc phức tạp.

Trên các thiết bị có khả năng gọi điện, [liên kết "click" để gọi](https://developers.google.com/web/fundamentals/native-hardware/click-to-call/) cho phép người dùng thực hiện một cuộc gọi điện đơn giản bằng cách chỉ cần nhấp vào liên kết. Trên hầu hết các thiết bị mobile , người dùng sẽ xác nhận trước khi số được gọi hoặc một danh sách sẽ hiện để yêu cầu người dùng cách mà số sẽ được xử lý.

### Dễ dàng hoàn hiện trên cách thiết bị khác.

Người dùng hầu như muốn hoàn thành các tác vụ trên một thiết bị khác. Ví dụ, họ có thể  sẽ muốn xem một mục trên một màn hình lớn hơn. Hoặc họ có thể bận rộn và muốn hoàn thành sau. Hỗ trợ khách hàng bằng cách cho phép người dùng[chia sẻ các mục trên mạng xã hội](https://developers.google.com/web/fundamentals/discovery-and-monetization/social-discovery/), hoặc bằng cách cho phép người dùng tự gửi email liên kết trực tiếp từ trang web.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/cc-other-device-good.png) 
**Nên làm**: Cung cấp các cách dễ dàng cho người dùng để tiếp tục duyệt hoặc mua sắm trên một thiết bị khác.


## Đầu vào form 

**Thành công:** Cung cấp trải nghiệm chuyển đổi liền mạch, mượt mà với các form sử dụng được.

### Hợp lý hoá đầu vào của thông tin.

Tự động chuyển sang trường tiếp theo khi người dùng nhấn Return. Thông thường, càng ít thao tác thực hiện càng tốt.

### Chọn đầu vào đơn giản nhất.

Sự dụng [đầu vào phù hợp nhất](https://developers.google.com/web/fundamentals/design-and-ux/input/forms/choose-the-best-input-type) cho mỗi trường hợp. Sử dụng phần tử giống như[`datalist`](https://developers.google.com/web/fundamentals/design-and-ux/input/forms/choose-the-best-input-type#offer-suggestions-during-input-with-datalist) để cung cấp giá trị gợi ý cho một trường.

### Cung cấp lịch trực quan để chọn ngày.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/forms-calendar-good.png) 
**Nên làm**: Sử dụng lịch khi có thể.

Ghi rõ ngày bắt đầu và kết thúc. Người dùng không cần phải dời trang web và kiểm tra một ứng dụng lịch khác chỉ để lên lịch cho một ngày.


### Giảm thiểu lỗi form với các nhãn và các xác thực thời gian thực.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/forms-multipart-good.png) 
**Nên làm**: Chuẩn bị trước nội dung nếu có thể

Nhập nhãn dán đúng cách và xác thức đầu vào trong thời gian thực.

### Thiết kế biểu mẫu hiệu quả.

Tận dụng tính năng [tự động điền](https://developers.google.com/web/fundamentals/design-and-ux/input/forms/#label-and-name-inputs-properly#use-metadata-to-enable-auto-complete) để người dùng có thể dễ dàng hoàn thành form với dữ liệu đã được điền trước.
Điền sẵn các trường có các thông tin bạn đã biết. Ví dụ, khi lấy địa chỉ chuyển hàng và thanh toán, cố gắng sử dụng [`requestAutocomplete`](https://developers.google.com/web/fundamentals/design-and-ux/input/forms/use-request-auto-complete) hoặc cho phép người dùng sao chép địa chỉ chuyển hàng của họ vào địa chỉ thanh toán (hoặc ngược lại).

## Tính sử dụng và yếu tố hình thức

**Thành công:** Làm hài lòng người dùng thiết bị di động với những thứ nhỏ giúp nâng cao trải nghiệm của họ.

### Tối ưu hoá toàn bộ trang web của bạn cho mobile

Sử dụng một [responsive layout](https://developers.google.com/web/fundamentals/design-and-ux/responsive/) thay đổi dựa trên kích thước và khả năng của thiết bị của người dùng. Người tham gia nghiên cứu đã tìm thấy trang web có sự kết hợp giữa các trang cho desktop và tối ưu hoá cho mobile, thậm chí còn khó sử dụng hơn các trang web trên desktop.

### Không làm cho người dùng phải zoom

Người dùng cảm thấy thoải mái với các trang web cuộn theo chiều dọc nhưng không phải theo chiều ngang. Tránh các phần tử có chiều rộng cố định lớn. Sử dụng [CSS media queries](https://developers.google.com/web/fundamentals/design-and-ux/responsive/#use-css-media-queries-for-responsiveness) để áp dụng các dịnh dạng khác nhau cho các màn hình khác nhau. Đừng tạo nội dung mà chỉ hiển thị tốt ở màn hình cá nhân [theo chiều rộng hiện thị](https://developers.google.com/web/fundamentals/design-and-ux/responsive/#set-the-viewport). Các trang web buộc người dùng cuộn theo chiều ngang sẽ không thành công ở [Google Mobile-Friendly Test](https://search.google.com/test/mobile-friendly), điều này có thể tác động tiêu cực đến xếp hạng tìm kiếm của họ.

### Làm cho các hình ảnh sản phẩm có thể được mở rộng

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/sw-make-images-expandable-good.png) 
**Nên làm**: Làm cho các hình ảnh sản phẩm có thể được mở rộng và dễ dàng xem chi tiết.

Khách hàng bán lẻ mong đợi các trang web sẽ cho họ [xem cận cảnh có độ phân giải cao](https://developers.google.com/web/fundamentals/design-and-ux/responsive/images#make-product-images-expandable) của sản phẩm. Những người tham gia nghiên cứu cảm thấy thất vọng khi không thể thấy những gì họ đang mua.

### Cho người dùng biết định hướng nào hoạt động tốt nhất

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/us-orientation.jpg) 
**Nên làm**: Cho người dùng biết định hướng nào hoạt động tốt nhất.

Những người tham gia nghiên cứu có khuynh hướng ở cùng một hướng màn hình cho đến khi có một cái gì đó nhắc họ chuyển đổi. Thiết kế theo cả 2 chiều ngang và dọc, hoặc khuyến khích người dùng chuyển sang hướng tối ưu. Hãy chắc chắn rằng các lời gọị hành động quan trọng có thể được hoàn thành ngay cả khi người dùng bỏ qua đề xuất để chuyển hướng.

### Giữ người dùng của bạn trong một trình duyệt duy nhất.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/sw-single-browser-good.png) 
**Nên làm**: Macy giữ người dùng trên trang web của họ bằng cách cung cấp phiếu giảm giá trên trang web.

Người dùng có thể gặp vấn đề khi chuyển đổi giữa các cửa sổ và có thể không tìm được đường trở lại trang web. Tránh gọi hành động mà khởi động một cửa sổ mới. Xác định bất kì hành trình nào mà có thể khiến người dùng điều hướng ra ngoài trang của bạn và cung cấp các tính năng để giữ họ ở lại trang của bạn. Ví dụ, nếu bạn chấp nhận phiếu giảm giá, cung cấp cho họ trực tiếp trên trang web, thay vì buộc người dùng tìm kiếm các trang web khác cho giao dịch.

### Tránh đặt nhãn "full site"

Khi những người tham gia nghiên cứu thấy tuỳ chọn "full site" (i.e., desktop site) so với "mobile site", họ cho thằng các trang web dành cho mobile thiếu nội dung và chọn "full", điều hướng họ đến trang web cho desktop. 

### Hãy rõ ràng lý do bạn cần vị trí của người dùng 

Người dùng luôn luôn hiểu tại sao bạn hỏi [địa chỉ](https://developers.google.com/web/fundamentals/native-hardware/user-location/)của họ. Những người tham gia nghiên cứu cố gắng đặt một khách sạn ở thành phố khác trở nên bối rối khi trang web du lịch đã phát hiện ra vị trí của họ và đề xuất các khách sạn trong thành phố hiện tại thay vì ở thành phố khác. Để trống trường vị trí như mặc định, và để người dùng chọn nơi phổ biến của họ thông qua hành động rõ ràng như "Tìm gần tôi".

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/sw-navigation-good.png) 
**Nên làm**: Luôn luôn yêu cầu truy cập vị trí dựa trên địa chỉ của người dùng.

![](https://developers.google.com/web/fundamentals/design-and-ux/principles/images/sw-navigation-bad.png) 
**Không làm**: Yêu cầu nó ngay lập tức trên homepage trong khi trang đang được tải sẽ dẫn đến trải nghiệm người dùng kém.





