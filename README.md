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
3. Các danh mục sản phẩm phổ biến nhất là gì? Khối lượng bán của chúng so sánh như thế nào? Có danh mục nào tiềm năng nhưng chưa được khai thác triệt để không?
4. Giá trị đơn hàng trung bình (AOV) là bao nhiêu? Có danh mục nào mang lại giá trị đơn hàng cao vượt trội?
5. Tỉ lệ hủy đơn trung bình là bao nhiêu? Danh mục sản phẩm nào có nhiều đơn hủy nhất?

### Hiệu suất Người bán và Hành vi/ Trải nghiệm khách hàng
1. Có bao nhiêu người bán đang hoạt động trên Olist? Con số này thay đổi ra sao theo thời gian?
2. Phương thức thanh toán nào được khách hàng sử dụng nhiều nhất? Điều này thay đổi như thế nào theo từng danh mục sản phẩm và khu vực địa lý? Phương thức nào phổ biến nhưng lại có giá trị đơn hàng trung bình thấp?
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
- Chuẩn hoá dữ liệu: Viết hoa chữ cái đầu của tên riêng, xử lý ký tự bị sai như "são paulo" thành "sao paulo" thông qua Replace Value.
- Tách cột ngày giờ thành cột ngày riêng để thuận tiện cho việc phân tích theo thời gian.
- Merge bảng "Product_category_name" với bảng "Olist_products" để tạo bảng dữ liệu hoàn chỉnh về sản phẩm. (Cdưới cho thấy *tỷ lệ giao hàng đúng hạn (On-Time Delivery Rate) đạt 93.4%*, nghĩa là phần lớn đơn hàng được giao đúng thời gian dự kiến, chỉ có khoảng *6.6% đơn hàng bị trễ*. Bên cạnh đó, *thời gian giao hàng trung bình (AVG Delivery Day) là 12.5 ngày*. 

<img width="596" alt="Ảnh màn hình 2025-03-20 lúc 14 07 22" src="https://github.com/user-attachments/assets/e785568b-e024-4bbe-8b00-22a99ce24482" />

Để tính toán ra được Ontime Delivery Rate, thực hiện lần lượt từng bước sau:

Đầu tiên, tính thời gian giao hàng thực tế:

Tạo cột Deliver time để tính số ngày giao hàng bằng cách lấy khoảng cách ngày giữa thời điểm đặt hàng (order_purchase_timestamp.1) và thời điểm khách hàng nhận được hàng (order_delivered_customer_date.1).

```dax
Deliver time = DATEDIFF(olist_orders_dataset[order_purchase_timestamp.1],olist_orders_dataset[order_delivered_customer_date.1],DAY)
```

Sau đó, phân loại đơn hàng giao đúng hạn hay trễ hạn:

Tạo cột Delivery_On_Time, kiểm tra nếu ngày giao hàng thực tế (order_delivered_customer_date.1) sớm hơn hoặc bằng ngày dự kiến (order_estimated_delivery_date.1) thì đơn hàng "On Time", ngược lại là "Late".

```dax
Delivery_On_Time = 
IF(
    olist_orders_dataset[order_delivered_customer_date.1] <= olist_orders_dataset[order_estimated_delivery_date.1],
    "On Time", 
    "Late"
)
```

![Ảnh chụp màn hình 2025-03-19 221834](https://github.com/user-attachments/assets/ad27c041-ebf8-499c-a5c5-50c9cf70cd29)

Như vậy, có thể thấy hệ thống vận hành và giao hàng đang hoạt động khá hiệu quả, đảm bảo phần lớn đơn hàng được giao đúng thời gian cam kết. Tuy nhiên, vẫn còn một tỷ lệ nhỏ (6,6%) đơn hàng bị trễ, có thể ảnh hưởng đến trải nghiệm của khách hàng. Cần phân tích nguyên nhân gây ra sự chậm trễ, chẳng hạn như vấn đề kho vận, năng lực nhà cung cấp dịch vụ giao hàng hoặc tình trạng hàng hóa, để có biện pháp cải thiện và tối ưu hơn nữa.

#### 5. Đánh giá trung bình của khách hàng là bao nhiêu? Có mối liên hệ nào giữa điểm đánh giá và thời gian giao hàng không?

<img width="499" alt="Ảnh màn hình 2025-03-21 lúc 14 37 08" src="https://github.com/user-attachments/assets/02136bf7-8979-4d6f-99c1-8c64ca45af64" />

Điểm đánh giá trung bình đạt **4,09/5**, cho thấy phần lớn khách hàng có trải nghiệm tích cực với sản phẩm/dịch vụ. Mặc dù điểm số này khá cao và gần mức tối đa, nhưng vẫn tồn tại một số đánh giá thấp hơn, thể hiện rặng vẫn còn một bộ phận khách hàng chưa thực sự hài lòng về sản phẩm/ dịch vụ mình nhận được. Việc phân tích sâu hơn các yếu tố ảnh hưởng, chẳng hạn như nhóm sản phẩm có điểm số thấp hoặc các phản hồi tiêu cực phổ biến, sẽ giúp xác định chính xác những điểm cần tối ưu để nâng cao trải nghiệm khách hàng.

![Ảnh chụp màn hình 2025-03-19 222148](https://github.com/user-attachments/assets/ba959182-a36c-4f3c-b43d-bb153816d5ee)

Biểu đồ trên cho thấy mối quan hệ giữa *thời gian giao hàng trung bình* và *điểm đánh giá của khách hàng* từ 2016 đến 2018. Ban đầu, thời gian giao hàng rất lâu (**55 ngày vào Q3/2016**) nhưng đã giảm mạnh xuống **dưới 15 ngày từ 2017 trở đi**, cho thấy Olist đã có sự cải thiện trong quy trình vận chuyển.  

Song song đó, *điểm đánh giá trung bình tăng từ 1,00 lên hơn 4,00*, phản ánh sự hài lòng của khách hàng khi thời gian giao hàng được rút ngắn. Tuy nhiên, đến Q4/2018, cả hai chỉ số đều giảm, đặc biệt điểm đánh giá giảm xuống còn **2,25**.  

![Ảnh chụp màn hình 2025-03-19 221736](https://github.com/user-attachments/assets/9b74250f-1a30-4364-86a3-37cb0e7b19e6)

Hai biểu đồ trên thể hiện mối quan hệ giữa thời gian giao hàng, điểm đánh giá và số lượng đơn hàng.  

Biểu đồ bên trái cho thấy *thời gian giao hàng càng ngắn, điểm đánh giá càng cao*. Cụ thể, đơn hàng có điểm đánh giá **1 sao** có thời gian giao trung bình **21,25 ngày**, trong khi đơn hàng **5 sao** chỉ mất **10,62 ngày**. Điều này khẳng định *tốc độ giao hàng ảnh hưởng trực tiếp đến mức độ hài lòng của khách hàng*.  

Biểu đồ bên phải cho thấy *đơn hàng có điểm 5 sao chiếm tỷ lệ lớn nhất*, lên tới **57K đơn**, trong khi đơn hàng có điểm thấp như **1 sao (11K đơn) hoặc 2 sao (3K đơn) thấp hơn đáng kể**. Điều này gợi ý rằng chất lượng giao hàng và trải nghiệm tốt sẽ thúc đẩy số lượng đơn hàng, còn trải nghiệm kém có thể khiến khách hàng không quay lại và đánh giá không tốt về sản phẩm/ dịch vụ nhận được.  

Tóm lại, doanh nghiệp nên tập trung tối ưu tốc độ giao hàng để không chỉ nâng cao trải nghiệm khách hàng mà còn gia tăng số lượng đơn đặt hàng trong tương lai.

#### 6. Có danh mục sản phẩm nào có điểm đánh giá đáng chú ý không?

![Ảnh chụp màn hình 2025-03-19 222027](https://github.com/user-attachments/assets/083671b4-2b2d-4c10-8ddf-cb3999c7c3cc)

Biểu đồ cho thấy danh mục *security_and_services* có điểm đánh giá trung bình thấp nhất (**2.50**), cho thấy khách hàng không hài lòng với chất lượng dịch vụ này, có thể do tốc độ phản hồi chậm, giao hàng chậm hoặc sản phẩm dịch vụ không đáp ứng kỳ vọng của khách hàng. Trong khi đó, các danh mục còn lại có điểm đánh giá trung bình dao động từ **3.62 đến 3.93**, phản ánh mức độ hài lòng của khách hàng khá ổn định strong khoảng 3 - 4 điểm, nhưng vẫn cần cải thiện để đạt mức đánh giá tuyệt đối. Cần lưu ý rằng, có một mục "(Blank)" với điểm đánh giá **3.90**, có thể do dữ liệu chưa được gán đúng danh mục sản phẩm, gây ảnh hưởng đến tính chính xác của báo cáo. Để cải thiện, cần phân tích kỹ nguyên nhân khiến *security_and_services* có điểm thấp, kết hợp thu thập phản hồi chi tiết từ khách hàng để nâng cao trải nghiệm mua hàng, đồng thời kiểm tra và xử lý dữ liệu trống để đảm bảo độ chính xác trong đánh giá.

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

