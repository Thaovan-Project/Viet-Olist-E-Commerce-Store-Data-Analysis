# Olist E-Commerce Store Data Analysis

## Giới thiệu
Dự án này phân tích bộ dữ liệu công khai từ Olist - một nền tảng thương mại điện tử của Brazil nhằm mục đích rút ra những insight về tình hình kinh doanh, hiệu suất bán hàng, hành vi khách hàng, hiệu suất người bán, và quy trình giao hàng của nền tảng này giai đoạn 2016-2018.

## Về bộ dữ liệu
Dữ liệu thô được được public và lấy từ Kaggle, bao gồm 9 bảng dưới đây: 
- Olist_customers
- Olist_geolocation
- Olist_order_items
- Olist_order_payments
- Olist_order_reviews
- Olist_orders
- Olist_products
- Olist_sellers
- Product_category_name.

Mỗi bảng lưu trữ một loại thông tin chi tiết về đơn hàng, bao gồm ngày đặt hàng, thông tin sản phẩm, thanh toán, giao hàng, đánh giá từ khách hàng, thông tin người bán và dữ liệu hành vi cũng như nhân khẩu học của khách hàng. Bộ dữ liệu này cho phép phân tích toàn diện quy trình từ lúc khách hàng đặt hàng đến khi nhận được sản phẩm và để lại đánh giá.

## Mục tiêu của dự án
Mục tiêu của dự án là cung cấp những insight giá trị giúp tối ưu hóa hiệu suất và thúc đẩy tăng trưởng bền vững cho Olist thông qua trả lời các câu hỏi kinh doanh (business questions) thuộc các nhóm dưới đây.

### Hiệu suất Bán hàng và Doanh thu
1. Tổng doanh thu của Olist là bao nhiêu? Doanh thu thay đổi như thế nào theo thời gian? Có giai đoạn nào đột biến hay suy giảm rõ rệt không?
2. Số lượng đơn hàng được đặt trên Olist mỗi tháng? Có biến động theo mùa hay xu hướng nào đáng chú ý không?
3. Các danh mục sản phẩm phổ biến nhất là gì? Khối lượng bán của chúng như thế nào? Có danh mục sản phẩm nào tiềm năng nhưng chưa được khai thác triệt để không?
4. Giá trị đơn hàng trung bình (AOV) là bao nhiêu? Có danh mục nào mang lại giá trị đơn hàng cao vượt trội?
5. Tỉ lệ hủy đơn trung bình là bao nhiêu? Danh mục sản phẩm nào có nhiều đơn hủy nhất?

### Hiệu suất người bán và hành vi/ trải nghiệm khách hàng
1. Có bao nhiêu người bán đang hoạt động trên Olist? Con số này thay đổi ra sao theo thời gian?
2. Phương thức thanh toán nào được khách hàng sử dụng nhiều nhất? Điều này thay đổi như thế nào theo từng danh mục sản phẩm và khu vực địa lý?
3. Có bao nhiêu khách hàng quay lại mua sắm? Họ chiếm bao nhiêu phần trăm tổng doanh thu? Và họ quay lại mua sắm danh mục sản phẩm nào nhiều nhất?
4. Tình trạng giao hàng đang như thế nào? Các đơn hàng giao đúng hẹn chiếm bao nhiêu phần trăm?
5. Đánh giá trung bình của khách hàng là bao nhiêu? Có mối liên hệ nào giữa điểm đánh giá và thời gian giao hàng không?
6. Có danh mục sản phẩm nào có điểm đánh giá đáng chú ý không?

## Data Cleaning sử dụng Power Query
Dữ liệu sau khi tải về đã được nhập vào Power BI và xử lý qua Power Query như sau:
- Chế độ "Column profiling" được chuyển từ "Top 1000 rows" sang "Entire dataset"
- Bật các tùy chọn "Column quality", "Column profile", và "Column distribution" để có cái nhìn tổng quan về chất lượng dữ liệu từng cột.
- Chuyển hàng đầu tiên thành tiêu đề cột cho các bảng chưa đúng định dạng. (Use First row as Header)
- Kiểm tra và chỉnh sửa định dạng dữ liệu cho từng cột.
- Chuyển đổi cột chứa giá trị tiền tệ (giá sản phẩm, giá thanh toán...) sang định dạng Fixed Decimal.
- Chuẩn hoá dữ liệu: Viết hoa chữ cái đầu của tên riêng, xử lý ký tự bị sai như "nhằm thể cải thiện để đạt mức đánh giá tuyệt đối. Cần lưu ý rằng, có một mục "(Blank)" với điểm đánh giá **3.90**, có thể do dữ liệu chưa được gán đúng danh mục sản phẩm, gây ảnh hưởng đến tính chính xác của báo cáo. Để cải thiện, cần phân tích kỹ nguyên nhân khiến *security_and_services* có điểm thấp, kết hợp thu thập phản hồi chi tiết từ khách hàng để nâng cao trải nghiệm mua hàng, đồng thời kiểm tra và xử lý dữ liệu trống để đảm bảo độ chính xác trong đánh giá.

## Insight nổi bật
- Tổng doanh thu cao & có xu hướng tăng, đặc biệt vào Black Friday/ hoặc các chương trình khuyến mãi ngắn hạn theo mùa, cho thấy sức mua tăng mạnh vào mùa cao điểm. 
- Một số danh mục có giá trị đơn hàng trung bình (AOV) cao đóng góp lớn vào doanh thu có thể tập trung upsell.
- Tỷ lệ hủy đơn thấp nhưng có xu hướng tăng vào một số giai đoạn. Một số danh mục bị hủy nhiều có thể do chất lượng hoặc thời gian giao hàng.
- Thanh toán chủ yếu bằng thẻ tín dụng & boleto (truyền thống).
- Thời gian giao hàng trung bình 12 ngày, trong đó 25% đơn bị giao trễ, giao hàng nhanh chóng có thể cải thiện đáng kể trải nghiệm & đánh giá của khách hàng.
- São Paulo là thị trường trọng điểm với doanh thu và số đơn hàng cao nhất do vậy nên tập trung các ưu đãi và cải thiện logistics tại khu vực này.

## Đề xuất
- Tăng trưởng doanh thu bằng cách tập trung vào danh mục có doanh thu cao (như bed_bath_table, health_beauty,..) và mở rộng thị phần tại các thành phố có doanh thu lớn như São Paulo. Triển khai chương trình giảm giá theo quý/mùa/đợt để kích thích nhu cầu mua sắm của khách hàng
- Đảm bảo nguồn cung ổn định, kiểm soát chất lượng sản phẩm, và điều chỉnh giá hợp lý để giảm tỷ lệ hủy đơn, tăng sự hài lòng của khách hàng.
- Tối ưu phương thức thanh toán cũng như khuyến khích thanh toán qua thẻ tín dụng (phổ biến nhất) bằng các chương trình giảm giá hoặc ưu đãi hoàn tiền.
- Tăng số lượng khách hàng trung thành bằng cách xây dựng chương trình ưu đãi cho khách hàng cũ (ví dụ như chương trình khách hàng thân thiết) để tăng số lượng đơn hàng mua lại, đặc biệt tại các khu vực có nhiều khách hàng.

## Kết luận
Olist là một nền tảng thương mại điện tử có doanh thu cao và đang tăng trưởng tốt, đặc biệt khi có các đợt khuyến mãi lớn. Tuy nhiên, vẫn còn tiềm năng tối ưu hóa để tăng lợi nhuận và mở rộng thị trường thông qua tối ưu hoá thời gian giao hàng, trải nghiệm khách hàng và các danh mục sản phẩm. 

