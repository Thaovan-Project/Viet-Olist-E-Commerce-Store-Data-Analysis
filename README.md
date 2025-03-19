# Phân tích dữ liệu cửa hàng thương mại điện tử Olist

## Giới thiệu
Dự án này phân tích bộ dữ liệu công khai từ Olist - một nền tảng thương mại điện tử của Brazil để rút ra những insight về tình hình kinh doanh, hiệu suất bán hàng, hành vi khách hàng, hiệu suất người bán, và quy trình giao hàng.

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
Để giúp đội ngũ Olist có cái nhìn sâu sắc hơn về hoạt động kinh doanh của nền tảng, tôi sẽ đi sâu phân tích và giải đáp các câu hỏi dưới đây. Mục tiêu là cung cấp những insight giá trị giúp tối ưu hóa hiệu suất và thúc đẩy tăng trưởng bền vững cho Olist.

### Hiệu suất Bán hàng và Doanh thu
1. Tổng doanh thu của Olist là bao nhiêu? Doanh thu thay đổi như thế nào theo thời gian? Có giai đoạn nào đột biến hay suy giảm rõ rệt không?
2. Số lượng đơn hàng được đặt trên Olist mỗi tháng? Có biến động theo mùa hay xu hướng nào đáng chú ý không?
3. Các danh mục sản phẩm phổ biến nhất là gì? Khối lượng bán của chúng so sánh như thế nào? Có danh mục nào tiềm năng nhưng chưa được khai thác triệt để không?
4. Giá trị đơn hàng trung bình (AOV) là bao nhiêu? Có danh mục nào mang lại giá trị đơn hàng cao vượt trội?

### Hiệu suất Người bán
1. Có bao nhiêu người bán đang hoạt động trên Olist? Con số này thay đổi ra sao theo thời gian? Có xu hướng người bán mới gia nhập hay rời khỏi nền tảng không?
2. Phân phối đánh giá của người bán như thế nào? Đánh giá có ảnh hưởng gì đến hiệu suất bán hàng và tỷ lệ đơn hàng hoàn thành không?
3. Tỉ lệ hủy đơn trung bình là bao nhiêu? Điều này ảnh hưởng đến hiệu suất người bán ra sao? Có nhóm người bán nào gặp vấn đề với tỷ lệ hủy đơn cao bất thường không?

### Trải nghiệm và Hành vi Khách hàng
1. Có bao nhiêu khách hàng quay lại mua sắm? Họ chiếm bao nhiêu phần trăm tổng doanh thu? Có nhóm khách hàng nào đặc biệt trung thành với nền tảng không?
2. Đánh giá trung bình của khách hàng là bao nhiêu? Điều này ảnh hưởng gì đến hiệu suất bán hàng? Có mối liên hệ nào giữa điểm đánh giá và doanh thu của sản phẩm không?
3. Phương thức thanh toán nào được khách hàng sử dụng nhiều nhất? Điều này thay đổi như thế nào theo từng danh mục sản phẩm và khu vực địa lý? Phương thức nào phổ biến nhưng lại có giá trị đơn hàng trung bình thấp?
4. Đánh giá và nhận xét của khách hàng tác động ra sao đến hiệu suất bán hàng? Có mối liên hệ nào giữa độ dài bình luận và điểm số không?
5. Tỉ lệ giữ chân khách hàng theo khu vực ra sao? Có vùng nào có lượng khách hàng mới cao nhưng tỷ lệ quay lại thấp không?

## Data Cleaning sử dụng Power Query
Dữ liệu sau khi tải về đã được nhập vào Power BI và xử lý qua Power Query như sau:
- Chế độ "Column profiling" được chuyển từ "Top 1000 rows" sang "Entire dataset"
- bật các tùy chọn "Column quality", "Column profile", và "Column distribution" để có cái nhìn tổng quan về chất lượng dữ liệu từng cột.
- Chuyển hàng đầu tiên thành tiêu đề cột cho các bảng chưa đúng định dạng. (Use First row as Header)
- Kiểm tra và chỉnh sửa định dạng dữ liệu cho từng cột.
- Chuyển đổi cột chứa giá trị tiền tệ (giá sản phẩm, giá thanh toán...) sang định dạng Fixed Decimal.
- Chuẩn hoá dữ liệu: Viết hoa chữ cái đầu của tên riêng, xử lý ký tự bị sai như "são paulo" thành "sao paulo".
- Tách cột ngày giờ thành cột ngày riêng để thuận tiện cho việc phân tích theo thời gian.
- Merge bảng "Product_category_name" với bảng "Olist_products" để tạo bảng dữ liệu hoàn chỉnh về sản phẩm. (Chỉ giữ lại 2 cột Product Id và Product Category Name in English)
- Loại bỏ bản ghi trùng lặp và cột dư thừa không cần thiết.

## Data Modelling
Dữ liệu được chuẩn hóa và xây dựng theo mô hình Star Schema, bao gồm:
- 1 bảng Fact chính: Olist_order_items chứa dữ liệu chi tiết nhất về đơn hàng và các chỉ số có thể tổng hợp.
- 1 bảng Fact phụ (Factless): Olist_orders chứa thông tin ngày tháng và khóa ngoại làm cầu nối giữa các bảng mà không có chỉ số đo lường cụ thể.
- 6 bảng Dimension: Olist_customers, Olist_geolocation, Olist_order_payments, Olist_order_reviews, Olist_products, Olist_sellers mô tả chi tiết khách hàng, sản phẩm, người bán, đánh giá và thanh toán.
- Bảng Date: Tạo bằng query sau:

```dax
Date = 
VAR FirstSalesDate = MIN(olist_orders_dataset[order_purchase_timestamp.1])
VAR YearFirstOrder = YEAR(FirstSalesDate)
VAR Dates = 
 FILTER(CALENDARAUTO(),YEAR([Date]) >= YearFirstOrder) RETURN
    ADDCOLUMNS(
        Dates,
        "Year", YEAR([Date]),
        "Month", FORMAT([Date], "mmm"),
        "Month Number", MONTH([Date]),
        "Quarter", FORMAT([Date], "\QQ"),
        "Day of Week", FORMAT([Date], "ddd"),
        "WeekDayNumber", WEEKDAY([Date]))
```

Mối quan hệ giữa các bảng:

Olist_order_items kết nối với:
- Olist_orders qua order_id
- Olist_sellers qua seller_id
- Olist_products qua product_id

Olist_orders kết nối với:
- Olist_customers qua customer_id
- Olist_order_payments qua order_id
- Olist_order_reviews qua order_id
- DimDate qua order_purchase_date

Olist_geolocation kết nối với Olist_customers qua zip_code_prefix

## Data Analysis & Reporting
### Hiệu suất Bán hàng và Doanh thu
Dưới đây là bức ảnh tổng quan về hiệu suất bán hàng và doanh thu của Olist trong giai đoạn 2016-2019:

<img width="952" alt="Ảnh màn hình 2025-03-18 lúc 16 51 01" src="https://github.com/user-attachments/assets/89fc8efa-6b71-4229-b705-bbe041f98396" />

#### 1. Tổng doanh thu của Olist là bao nhiêu? Doanh thu thay đổi như thế nào theo thời gian? Có giai đoạn nào đột biến hay suy giảm rõ rệt không?
Tổng doanh thu của cửa hàng thương mại điện tử Olist từ tháng 9 năm 2016 đến tháng 9 năm 2018 là ***R$15.422.461,77***. Con số này được tính bằng cách lấy tổng Payment_value của các đơn hàng có trạng thái "delivered" (vì chỉ khi đơn hàng được giao thành công tới khách hàng thì doanh thu của đơn hàng mới được ghi nhận), sử dụng công thức DAX như bên dưới:

```dax
Total Revenue = 
    CALCULATE(
        SUM(olist_order_payments[payment_value]),
        olist_orders[order_status] = "delivered"
    )
```


Từ năm 2016 đến 2019, doanh thu của Olist có xu hướng tăng dần qua từng năm, đặc biệt ghi nhận mức tăng trưởng mạnh trong giai đoạn 2016-2017.


<img width="493" alt="Ảnh màn hình 2025-03-18 lúc 16 24 30" src="https://github.com/user-attachments/assets/5d7da1d0-ada7-4794-aec5-1edb714f04e5" />


Biểu đồ dưới cho thấy doanh thu có sự tăng đột biến vào ngày 24/11/2017, đạt **R$175K**, nhiều khả năng do Black Friday hoặc một chương trình khuyến mãi lớn. Ngay sau đó, doanh thu giảm mạnh và quay về mức ổn định quanh **R$50K - R$60K**, cho thấy đây chỉ là sự tăng trưởng ngắn hạn. Trừ giai đoạn tăng vọt vào cuối năm 2017 trên, doanh thu thường dao động quanh mức R$50K - R$60K, cho thấy thị trường không có sự tăng trưởng đột phá.  Điều này chỉ ra rằng các chiến dịch khuyến mãi có thể tạo hiệu ứng tốt trong ngắn hạn, nhưng cần chiến lược giữ chân khách hàng để đảm bảo khả năng tăng trưởng được bền vững.


![Ảnh chụp màn hình 2025-03-18 192717](https://github.com/user-attachments/assets/f4704c02-1cb4-40a9-9945-9c571aec474a)

#### 2. Số lượng đơn hàng được đặt trên Olist mỗi tháng? Có biến động theo mùa hay xu hướng nào đáng chú ý không?
Trong giai đoạn 2016-2019, Olist có tổng cộng 99,44K đơn đặt hàng. Các đơn đã được đặt hàng thành công và hoàn thành giao cho khách hàng (Success Order) có tổng cộng 96K đơn hàng, và được định nghĩa bởi công thức:

```dax
Success Order = 
CALCULATE(
    COUNT('olist_orders_dataset'[order_id]), 
    'olist_orders_dataset'[order_status] = "delivered"
)
```

![Ảnh chụp màn hình 2025-03-18 200051](https://github.com/user-attachments/assets/785420e7-f898-4fee-95fd-0520f6bfe8d4)

Tổng cộng có 625 đơn hàng bị hủy trong giai đoạn 2016-2019. Số lượng đơn hủy tăng dần từ 2016, đạt đỉnh vào tháng 8/2017 (84 đơn) và tháng 2/2018 (73 đơn), cho thấy có thể có vấn đề về vận hành, chính sách bán hàng hoặc dịch vụ khách hàng. Sau đó, số hủy giảm nhưng vẫn dao động ở mức 18 - 41 đơn/tháng. Một số tháng như 4/2017 và 9/2017 có mức hủy thấp bất thường, có thể do ít chương trình khuyến mãi hoặc cải thiện dịch vụ. Cần phân tích nguyên nhân hủy đơn trong giai đoạn cao điểm và so sánh với tổng số đơn đặt để tối ưu chính sách bán hàng và trải nghiệm khách hàng.
![Ảnh chụp màn hình 2025-03-18 200315](https://github.com/user-attachments/assets/3563d7da-bebb-48da-866a-4da70e2adcf0)

#### 3. Các danh mục sản phẩm phổ biến nhất là gì? Khối lượng bán của chúng so sánh như thế nào? Có danh mục nào tiềm năng nhưng chưa được khai thác triệt để không?
Biểu đồ cho thấy sự chênh lệch giữa số lượng đơn hàng và tổng doanh thu theo danh mục sản phẩm. Một số danh mục như *bed_bath_table* và *health_beauty* có số lượng đơn cao trong top nhưng doanh thu không tương ứng, cho thấy giá trị trung bình mỗi đơn thấp. Ngược lại, các danh mục như *watches_gifts* và *cool_stuff* có doanh thu cao dù số lượng đơn ít hơn, cho thấy đây là các sản phẩm có giá trị cao. Như vậy, để tối ưu danh mục có đơn hàng cao nhưng doanh thu thấp, Olist có thể cân nhắc tập trung tăng giá trị trung bình mỗi đơn bằng chiến lược upsell/cross-sell. Đồng thời, với các danh mục có doanh thu cao nhưng số lượng đơn ít, nên đẩy mạnh marketing để thu hút thêm khách hàng, từ đó cân bằng giữa số lượng đơn hàng và tổng doanh thu.

![Ảnh chụp màn hình 2025-03-18 230134](https://github.com/user-attachments/assets/68bf9239-a7e6-48c4-ac2b-89e56474a954)

#### 4. Giá trị đơn hàng trung bình (AOV) là bao nhiêu? Có danh mục nào mang lại giá trị đơn hàng cao vượt trội?
Giá trị một đơn hàng trung bình của Olist là R$ 159.85, và được tính theo công thức dưới đây:
```dax
Average Order Value = 
                  DIVIDE ([Total Revenue], 
                      CALCULATE(COUNTROWS('olist_orders_dataset'), 
                         olist_orders_dataset[order_status] IN {"delivered"})
```

![Ảnh chụp màn hình 2025-03-18 225233](https://github.com/user-attachments/assets/08dd5e10-afb2-4e67-8267-28ad678c97ef)

Biểu đồ cho thấy *computers* có giá trị đơn hàng trung bình cao nhất (R$ 1.300,8), gần gấp đôi so với danh mục thứ hai (*small_appliances* – R$ 684,3), cho thấy đây là mặt hàng có giá trị cao và đóng góp lớn vào doanh thu dù số lượng đơn có thể không nhiều. Tuy nhiên, phần lớn các danh mục còn lại có giá trị đơn hàng trung bình dao động từ R$ 200 - R$ 500, đặc biệt nhóm nội thất và gia dụng như *furniture_living* có giá trị trung bình thấp nhất (R$ 211,6), cho thấy khách hàng có thể mua lẻ thay vì theo bộ. Do đó, cần đẩy mạnh các danh mục có giá trị đơn hàng cao bằng cách tối ưu trải nghiệm mua sắm và tạo chương trình ưu đãi, đồng thời áp dụng chiến lược *bundling* hoặc upsell/cross-sell để tăng giá trị đơn hàng ở các danh mục thấp, từ đó tối ưu doanh thu và lợi nhuận.

![Ảnh chụp màn hình 2025-03-18 225739](https://github.com/user-attachments/assets/2704741e-9a20-4566-867c-9f30a93133d6)

### Hiệu suất Người bán
#### 1. Có bao nhiêu người bán đang hoạt động trên Olist? Con số này thay đổi ra sao theo thời gian?
<img width="546" alt="Ảnh màn hình 2025-03-19 lúc 09 42 26" src="https://github.com/user-attachments/assets/4c84f5d6-271b-41dd-8050-27d2f96edb29" />

#### 2. Phân phối đánh giá của người bán như thế nào? Đánh giá có ảnh hưởng gì đến hiệu suất bán hàng và tỷ lệ đơn hàng hoàn thành không?

#### 3. Tỉ lệ hủy đơn trung bình là bao nhiêu? Điều này ảnh hưởng đến hiệu suất người bán ra sao? 

### Trải nghiệm và Hành vi Khách hàng
#### 1. Có bao nhiêu khách hàng quay lại mua sắm? Họ chiếm bao nhiêu phần trăm tổng doanh thu? Có nhóm khách hàng nào đặc biệt trung thành với nền tảng không?
<img width="380" alt="Ảnh màn hình 2025-03-19 lúc 09 14 26" src="https://github.com/user-attachments/assets/e59dcaaa-5cce-4952-be17-dfc4c47d983f" />

#### 2. Đánh giá trung bình của khách hàng là bao nhiêu? Có mối liên hệ nào giữa điểm đánh giá và thời gian giao hàng không?

<img width="242" alt="Ảnh màn hình 2025-03-19 lúc 09 40 23" src="https://github.com/user-attachments/assets/7ba2ea81-28ca-4b24-86c7-4a86dc6c0d92" />

Dựa trên hai biểu đồ trên, có thể thấy rõ rằng thời gian giao hàng là yếu tố tác động trực tiếp đến trải nghiệm khách hàng và điểm đánh giá.

Biểu đồ đầu tiên cho thấy một mối quan hệ nghịch đảo giữa thời gian giao hàng trung bình và điểm đánh giá. Cụ thể, những đơn hàng có thời gian giao lâu nhất (21.25 ngày) chỉ nhận được điểm đánh giá trung bình là 1, trong khi những đơn hàng được giao nhanh hơn (10.62 ngày) lại đạt điểm đánh giá 5. Điều này phản ánh rõ ràng rằng khách hàng càng được phục vụ nhanh chóng, họ càng hài lòng và đánh giá cao.

Xuyên suốt từ năm 2016 đến năm 2018, có thể nhận thấy nỗ lực tối ưu hóa vận hành của Olist để cải thiện thời gian giao hàng và nâng cao trải nghiệm khách hàng.

Ở giai đoạn đầu tiên (Q3 2016), thời gian giao hàng lên đến 55 ngày, dẫn đến điểm đánh giá cực kỳ thấp, chỉ đạt 1.00 điểm. Tuy nhiên, nhờ sự cải tiến liên tục, đến Q4 2018, thời gian giao hàng đã giảm xuống 2.25 ngày, trong khi điểm đánh giá tăng đáng kể lên 4.26 — gần chạm ngưỡng lý tưởng 5.0 điểm. Đây là minh chứng rõ ràng cho thấy việc rút ngắn thời gian giao hàng không chỉ giúp Olist cải thiện hiệu suất vận hành, mà còn xây dựng được lòng tin và sự hài lòng từ phía khách hàng.

Tuy nhiên, không phải toàn bộ quá trình đều diễn ra thuận lợi. Năm 2017 đánh dấu một giai đoạn có sự biến động, mặc dù thời gian giao hàng đã giảm đáng kể (xuống khoảng 11-14 ngày), nhưng điểm đánh giá vẫn chỉ dao động quanh mức 4.0 - 4.12 điểm và không tăng trưởng đáng kể. Điều này cho thấy, bên cạnh việc rút ngắn thời gian giao hàng, chất lượng sản phẩm và dịch vụ cũng là yếu tố quan trọng để cải thiện sự hài lòng toàn diện của khách hàng

Như vậy có thể thấy rằng, thời gian giao hàng nhanh là yếu tố hàng đầu tác động đến điểm đánh giá. Vì vậy, Olist cần tiếp tục đầu tư vào cải thiện chuỗi cung ứng, nâng cấp hệ thống logistics và hợp tác với các đơn vị vận chuyển hiệu quả. Tuy nhiên, cũng phải để ý rằng năm 2017 là minh chứng cho việc giao hàng nhanh không đồng nghĩa với việc khách hàng hoàn toàn hài lòng. Olist cần đảm bảo rằng sản phẩm đúng mô tả, đóng gói cẩn thận và dịch vụ khách hàng chuyên nghiệp để giữ vững điểm đánh giá.

#### 3. Phương thức thanh toán nào được khách hàng sử dụng nhiều nhất? Điều này thay đổi như thế nào theo từng danh mục sản phẩm và khu vực địa lý? Phương thức nào phổ biến nhưng lại có giá trị đơn hàng trung bình thấp?
<img width="322" alt="Ảnh màn hình 2025-03-19 lúc 09 15 02" src="https://github.com/user-attachments/assets/603c170e-5b38-451b-8e76-3fa0dea830a4" />

Dựa trên biểu đồ "Orders Count by Geolocation City and Payment Type", có thể thấy Sao Paulo là thành phố dẫn đầu vượt trội về số lượng đơn hàng so với các khu vực còn lại. Điều này cho thấy đây là thị trường trọng điểm, đóng góp phần lớn vào doanh số của doanh nghiệp. Việc tập trung tối ưu hóa logistics, triển khai chiến dịch khuyến mãi hoặc cá nhân hóa trải nghiệm người dùng tại Sao Paulo sẽ giúp doanh nghiệp khai thác tối đa tiềm năng tại khu vực này.

Bên cạnh đó, thẻ tín dụng (credit_card) đang chiếm ưu thế rõ rệt trong phương thức thanh toán, đặc biệt là ở các thành phố lớn như Sao Paulo và Rio de Janeiro. Xu hướng này phản ánh hành vi tiêu dùng hiện đại, nơi khách hàng ưu tiên sự tiện lợi và linh hoạt. Doanh nghiệp nên tận dụng điều này bằng cách triển khai các chương trình ưu đãi, hoàn tiền hoặc tích điểm khi thanh toán bằng thẻ tín dụng, từ đó thúc đẩy thêm doanh số và tăng mức độ gắn bó của khách hàng.

Tuy nhiên, không thể bỏ qua "boleto" – phương thức thanh toán truyền thống vẫn chiếm tỷ trọng nhất định, đặc biệt là ở Sao Paulo. Điều này cho thấy một nhóm khách hàng vẫn giữ thói quen sử dụng phương thức thanh toán này, có thể do tâm lý muốn kiểm soát chi tiêu hoặc chưa quen với các hình thức thanh toán điện tử. Để không mất đi nhóm khách hàng này, doanh nghiệp nên cân nhắc giữ lại và đơn giản hóa quy trình thanh toán bằng boleto, đồng thời truyền thông rõ ràng về các lợi ích, độ an toàn của các phương thức hiện đại để từng bước thay đổi hành vi tiêu dùng.

Ngoài ra, Rio de Janeiro và Belo Horizonte cũng nổi lên như hai thị trường tiềm năng với lượng đơn hàng đáng kể chỉ đứng sau Sao Paulo. Doanh nghiệp có thể mở rộng thêm hoạt động marketing, khuyến mãi hoặc cải thiện dịch vụ giao hàng tại hai thành phố này để chiếm lĩnh thị phần. Những khu vực khác như Curitiba, Brasilia và Campinas tuy có lượng đơn hàng thấp hơn nhưng không nên bỏ qua, bởi đây có thể là những "ngôi sao đang lên" nếu doanh nghiệp biết cách tiếp cận đúng đối tượng và giải quyết các rào cản về chi phí vận chuyển hoặc tốc độ giao hàng.

Như vậy, để tối ưu hóa hoạt động kinh doanh, doanh nghiệp cần có chiến lược tập trung vào ba hướng chính: tối đa hóa thị phần tại Sao Paulo, thúc đẩy xu hướng thanh toán qua thẻ tín dụng để bắt kịp hành vi tiêu dùng hiện đại, và mở rộng khai thác các thị trường tiềm năng như Rio de Janeiro, Belo Horizonte và những thành phố mới nổi khác. Một chiến lược linh hoạt, cân bằng giữa việc giữ chân khách hàng truyền thống và chinh phục khách hàng hiện đại sẽ giúp doanh nghiệp phát triển bền vững trong dài hạn.

#### 4. Đánh giá và nhận xét của khách hàng tác động ra sao đến hiệu suất bán hàng? Có mối liên hệ nào giữa độ dài bình luận và điểm số không?

#### 5. Tỉ lệ giữ chân khách hàng theo khu vực ra sao? Có vùng nào có lượng khách hàng mới cao nhưng tỷ lệ quay lại thấp không?

## Insight

## Đề xuất

## Kết luận


