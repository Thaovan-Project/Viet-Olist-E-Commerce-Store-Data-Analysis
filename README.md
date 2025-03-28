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
- Chuẩn hoá dữ liệu: Viết hoa chữ cái đầu của tên riêng, xử lý ký tự bị sai như "São Paulo" thành "são paulo" thông qua Replace Value.
- Tách cột ngày giờ thành cột ngày riêng để thuận tiện cho việc phân tích theo thời gian.
- Merge bảng "Product_category_name" với bảng "Olist_products" để tạo bảng dữ liệu hoàn chỉnh về sản phẩm. (Chỉ giữ lại 2 cột Product Id và Product Category Name in English trong bảng này, bỏ các cột không liên quan đi, ví dụ: Product Name Length,...)
- Loại bỏ bản ghi trùng lặp và cột dư thừa không cần thiết tại các bảng.

## Data Modelling
Dữ liệu được chuẩn hóa và xây dựng theo mô hình Star Schema, bao gồm:
- 1 bảng Fact chính: Olist_order_items chứa thông tin từng sản phẩm trong mỗi đơn hàng, là bảng giao dịch chi tiết nhất. 
- 1 bảng Fact phụ: Olist_orders chứa thông tin tổng quan về đơn hàng bao gồm trạng thái đơn, thời gian đặt hàng, thời gian giao hàng thực tế/dự kiến.
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
Dưới đây là bức ảnh tổng quan về hiệu suất bán hàng và doanh thu của Olist trong giai đoạn 2016-2018:

![Ảnh chụp màn hình 2025-03-26 212300](https://github.com/user-attachments/assets/41e3273d-c31a-4770-a386-0baec3dad645)


#### 1. Tổng doanh thu của Olist là bao nhiêu? Doanh thu thay đổi như thế nào theo thời gian? Có giai đoạn nào đột biến hay suy giảm rõ rệt không?
Tổng doanh thu của cửa hàng thương mại điện tử Olist từ năm 2016 đến năm 2018 là **R$15.422.461,77**. Con số này được tính bằng cách lấy tổng Payment_value của các đơn hàng có trạng thái "delivered" (vì chỉ khi đơn hàng được giao thành công tới khách hàng thì doanh thu của đơn hàng mới được ghi nhận), sử dụng công thức DAX như bên dưới:

```dax
Total Revenue = 
    CALCULATE(
        SUM(olist_order_payments[payment_value]),
        olist_orders[order_status] = "delivered"
    )
```

Từ năm 2016 đến 2018, doanh thu của Olist có xu hướng tăng dần qua từng năm, đặc biệt ghi nhận mức tăng trưởng mạnh mẽ nhất trong giai đoạn 2016-2017.

![Ảnh chụp màn hình 2025-03-19 222734](https://github.com/user-attachments/assets/f2aad862-bf7b-46b2-8f6a-c17733ea957b)

Biểu đồ dưới cho thấy doanh thu có sự tăng đột biến vào ngày 24/11/2017, đạt **R$175K**, nhiều khả năng do Black Friday hoặc một chương trình khuyến mãi lớn trong thời gian ngắn. Ngay sau đó, doanh thu giảm mạnh và quay về mức ổn định quanh **R$50K - R$60K**, cho thấy đây chỉ là sự tăng trưởng ngắn hạn và thị trường không có sự tăng trưởng đột phá. Tuy nhiên, điều này chỉ ra rằng các chiến dịch khuyến mãi ngắn hạn có thể tạo hiệu ứng tốt trong một khoảng thời gian, nhưng cũng cần có thêm chiến lược giữ chân khách hàng để đảm bảo khả năng tăng trưởng được bền vững.

![Ảnh chụp màn hình 2025-03-18 192717](https://github.com/user-attachments/assets/f4704c02-1cb4-40a9-9945-9c571aec474a)

#### 2. Số lượng đơn hàng được đặt trên Olist mỗi tháng? Có biến động theo mùa hay xu hướng nào đáng chú ý không?
Trong giai đoạn 2016-2018, Olist có tổng cộng **99,44K** đơn đặt hàng. Các đơn đã được đặt hàng thành công và hoàn thành giao cho khách hàng (Success Order) có tổng cộng **96K** đơn hàng, và được định nghĩa bởi công thức:

```dax
Success Order = 
CALCULATE(
    COUNT('olist_orders_dataset'[order_id]), 
    'olist_orders_dataset'[order_status] = "delivered"
)
```

![Ảnh chụp màn hình 2025-03-18 200051](https://github.com/user-attachments/assets/785420e7-f898-4fee-95fd-0520f6bfe8d4)

Nhìn chung thì xu hướng phát sinh đơn hàng theo thời gian sẽ tỷ lệ thuận và không có biến đổi đáng kể so với biểu đồ xu hướng của doanh thu đã được nêu ở trên.

#### 3. Các danh mục sản phẩm phổ biến nhất là gì? Khối lượng bán của chúng như thế nào? Có danh mục sản phẩm nào tiềm năng nhưng chưa được khai thác triệt để không?
Biểu đồ cho thấy các danh mục *bed_bath_table*, *health_beauty* và *sports_leisure* có số lượng đơn hàng cao nhất, phản ánh nhu cầu lớn từ khách hàng. Ngược lại, một số danh mục như *watches_gifts* có doanh thu cao nhưng số lượng đơn hàng thấp, điều này có thể chỉ ra rằng các sản phẩm trong danh mục này có giá trị cao, dù không được mua với số lượng lớn. Ngược lại, danh mục *telephony* có số lượng đơn hàng cao nhưng doanh thu thấp, cho thấy giá trị trung bình mỗi đơn hàng của nhóm này khá thấp. Điều này có thể do phần lớn sản phẩm trong danh mục này có giá trị nhỏ, như phụ kiện điện thoại hoặc linh kiện thay thế, thay vì các sản phẩm có giá trị cao như điện thoại di động. Một số danh mục cuối bảng như *home_appliances* có cả doanh thu lẫn số lượng đơn hàng đều thấp, đặt ra câu hỏi về mức độ cạnh tranh và nhu cầu thực tế của khách hàng.

![Ảnh chụp màn hình 2025-03-18 230134](https://github.com/user-attachments/assets/68bf9239-a7e6-48c4-ac2b-89e56474a954)

Để tối ưu hiệu suất, Olist nên tăng cường quảng bá các danh mục có nhu cầu cao, đồng thời áp dụng chiến lược upsell và cross-sell cho các danh mục có giá trị đơn hàng cao nhưng số đơn ít. Với các danh mục có số đơn hàng lớn nhưng doanh thu chưa tối ưu, có thể cân nhắc việc triển khai combo sản phẩm và chương trình giảm giá khi mua số lượng nhiều sẽ giúp cải thiện giá trị trung bình mỗi đơn hàng. Và cuối cùng, cần đánh giá lại danh mục có hiệu suất thấp nhằm xác định nguyên nhân để điều chỉnh lại chiến lược bán hoặc loại bỏ khỏi danh mục kinh doanh nếu cần thiết.

#### 4. Giá trị đơn hàng trung bình (AOV) là bao nhiêu? Có danh mục nào mang lại giá trị đơn hàng cao vượt trội?
Giá trị một đơn hàng trung bình của Olist là **R$ 159.85**, và được tính theo công thức dưới đây:

![Ảnh chụp màn hình 2025-03-18 225233](https://github.com/user-attachments/assets/08dd5e10-afb2-4e67-8267-28ad678c97ef)

```dax
Average Order Value = 
AVERAGEX(
    ALLSELECTED(olist_orders_dataset[order_id]), 
    [Total Revenue]
)
```


Biểu đồ cho thấy *computers* có giá trị đơn hàng trung bình cao nhất **(R$ 1.300,8)**, và cao gần gấp đôi so với danh mục đứng thứ hai (*small_appliances* – R$ 684,3), cho thấy đây là mặt hàng có giá trị cao và đóng góp lớn vào doanh thu dù số lượng đơn có thể không nhiều. Tuy nhiên, phần lớn các danh mục sản phẩm còn lại có giá trị đơn hàng trung bình dao động từ **R$ 200 - R$ 500**. 

![Ảnh chụp màn hình 2025-03-18 225739](https://github.com/user-attachments/assets/2704741e-9a20-4566-867c-9f30a93133d6)

#### 5. Tỉ lệ hủy đơn trung bình là bao nhiêu? Danh mục sản phẩm nào có nhiều đơn hủy nhất?
Tổng cộng có **625** đơn hàng bị hủy trong giai đoạn 2016-2018. Biểu đồ cho thấy số lượng đơn hàng bị hủy tăng dần từ năm 2016 và đạt đỉnh vào **Q3 2018 với 140 đơn**. Tuy nhiên, đến **Q4 2018**, số đơn hủy giảm mạnh xuống chỉ còn **4 đơn**, cho thấy có thể đã có sự điều chỉnh về chính sách, quy trình hoặc nhu cầu thị trường suy giảm. Xu hướng gia tăng hủy đơn từ Q1 2017 đến Q3 2018 đặt ra dấu hỏi về trải nghiệm khách hàng, chất lượng sản phẩm hoặc quy trình xử lý đơn hàng. Để tối ưu, cần phân tích nguyên nhân dẫn đến con số hủy đơn đạt đỉnh trong Q3 2018, đồng thời đánh giá những yếu tố giúp giảm mạnh đơn huỷ trong Q4 2018 để áp dụng vào các giai đoạn tiếp theo.

![Ảnh chụp màn hình 2025-03-19 220307](https://github.com/user-attachments/assets/52dab1a9-6740-45bf-921a-5d63d7470ac6)

Biểu đồ bên dưới cho thấy danh mục *sports_leisure* có số lượng đơn hủy cao nhất (47 đơn), tiếp theo là *housewares* (37 đơn) và *health_beauty* (36 đơn). Đây có thể là những nhóm sản phẩm có nhu cầu mua cao nhưng cũng dễ bị hủy, nguyên nhân có thể do thay đổi quyết định của khách hàng hoặc các vấn đề về chất lượng, giá cả của sản phẩm. Để tối ưu, cần tìm hiểu nguyên nhân hủy đơn ở các danh mục có tỷ lệ huỷ cao, từ đó nhằm điều chỉnh chiến lược bán hàng, chính sách đổi trả hoặc kiểm soát chất lượng sản phẩm.

![Ảnh chụp màn hình 2025-03-19 223836](https://github.com/user-attachments/assets/edf725ee-5aa1-4a22-a3c6-d157341d7a55)

### Hiệu suất Người bán và Trải nghiệm/ Hành vi Khách hàng
#### 1. Có bao nhiêu người bán đang hoạt động trên Olist? Con số này thay đổi ra sao theo thời gian?
Dữ liệu cho thấy tổng số người bán đạt **3.095**, với xu hướng tăng trưởng mạnh từ 2016-2018. Cụ thể, số lượng người bán tăng đều qua các quý, từ mức gần như không có người bán ở Q3 2016 lên 1.7K người bán ở Q2 2018, chứng tỏ theo thời gian nền tảng thu hút ngày càng nhiều người bán tham gia. Tuy nhiên, đến Q3 2018, số lượng người bán giảm nhẹ còn 1.6K, cho thấy có thể có yếu tố ảnh hưởng như cạnh tranh gia tăng, chính sách thay đổi hoặc thị trường bão hòa. Để Olist có thể duy trì và phát triển, nền tảng cần phân tích nguyên nhân giảm và đưa ra chiến lược thu hút cũng như giữ chân người bán hiệu quả hơn.

<img width="546" alt="Ảnh màn hình 2025-03-19 lúc 09 42 26" src="https://github.com/user-attachments/assets/4c84f5d6-271b-41dd-8050-27d2f96edb29" />

#### 2. Phương thức thanh toán nào được khách hàng sử dụng nhiều nhất? Điều này thay đổi như thế nào theo từng danh mục sản phẩm và khu vực địa lý?

<img width="322" alt="Ảnh màn hình 2025-03-19 lúc 09 15 02" src="https://github.com/user-attachments/assets/603c170e-5b38-451b-8e76-3fa0dea830a4" />

Dựa trên biểu đồ, São Paulo dẫn đầu về số lượng đơn hàng cho thấy đây là thị trường trọng điểm cần tập trung khai thác. Thẻ tín dụng chiếm ưu thế vượt trội trong các phương thức thanh toán, đặc biệt ở các thành phố lớn như São Paulo và Rio de Janeiro, phản ánh xu hướng tiêu dùng linh hoạt tại các thành phố lớn. Doanh nghiệp nên tận dụng bằng cách triển khai ưu đãi khi thanh toán qua thẻ tín dụng để thúc đẩy doanh số.

Tuy nhiên, phương thức "boleto" (phương pháp thanh toán truyền thống) vẫn chiếm tỷ trọng đáng kể, đặc biệt ở São Paulo, cho thấy một nhóm khách hàng vẫn giữ thói quen thanh toán truyền thống. Nguyên nhân có thể do tâm lý muốn kiểm soát chi tiêu hoặc chưa quen với các hình thức thanh toán điện tử hiện đại mới. Để không mất đi nhóm khách hàng này, doanh nghiệp nên cân nhắc đơn giản hóa quy trình thanh toán bằng boleto, đồng thời có thể truyền thông về các lợi ích cũng như độ an toàn của các phương thức hiện đại để từng bước thay đổi hành vi tiêu dùng.

#### 3. Có bao nhiêu khách hàng quay lại mua sắm? Họ chiếm bao nhiêu phần trăm tổng doanh thu? Và họ quay lại mua sắm danh mục sản phẩm nào nhiều nhất?
Để tính toán được Retention rate, làm lần lượt các bước như sau:

Đầu tiên, xác định khách hàng có mua nhiều hơn 1 đơn hàng:

Câu lệnh Repeat_Purchases kiểm tra từng khách hàng (customer_unique_id) xem họ có đặt hơn 1 đơn hàng không. Nếu có, đánh dấu là 1, nếu không, là 0.

```dax
Repeat_Purchases = CALCULATE(
                      IF(COUNT('olist_orders_dataset'[order_id])>1,1,0),
                          ALLEXCEPT(olist_customers_dataset,olist_customers_dataset[customer_unique_id]))
```


Tiếp theo, đếm số khách hàng mua hàng lặp lại:

Câu lệnh tính Repeat_Purchase_Customers đếm số khách hàng duy nhất (customer_unique_id) có ít nhất 1 đơn hàng giao thành công (order_status = "delivered") và có giá trị Repeat_Purchases = 1 (tức là họ đã đặt hàng ít nhất 2 lần).

``` dax
Repeat_Purchase_Customers = 
CALCULATE(
    DISTINCTCOUNT(olist_customers_dataset[customer_unique_id]),
    olist_orders_dataset[order_status] = "delivered",
    FILTER(
        VALUES(olist_customers_dataset[customer_unique_id]), 
        [Repeat_Purchases] = 1 
    )
)
```
Và cuối cùng, tính toán tỷ lệ khách hàng quay lại mua hàng (Retention Rate):

Retention Rate được tính bằng cách lấy số khách hàng mua hàng lặp lại chia cho tổng số khách hàng đã từng mua hàng (DISTINCTCOUNT(customer_id)).
Nếu mẫu số bằng 0, kết quả trả về 0 để tránh lỗi chia cho 0.

```dax
Retention Rate = 
DIVIDE(
    [Repeat_Purchase_Customers], 
    DISTINCTCOUNT(olist_orders_dataset[customer_id]), 
    0
)
```

<img width="380" alt="Ảnh màn hình 2025-03-19 lúc 09 14 26" src="https://github.com/user-attachments/assets/e59dcaaa-5cce-4952-be17-dfc4c47d983f" />

Dữ liệu cho thấy Olist có tổng số khách hàng là **99.44K**, nhưng tỷ lệ giữ chân khách hàng chỉ ở mức **3.0%**, cho thấy hầu hết khách hàng chỉ mua một lần mà không quay lại. Xét theo danh mục sản phẩm, *bed_bath_table*, *sports_leisure*, *health_beauty* và *computers_accessories* là những danh mục có số lượng khách hàng quay lại mua nhiều nhất, cho thấy đây có thể là các mặt hàng có nhu cầu mua lại cao hoặc mang lại trải nghiệm tốt nên khách hàng muốn mua thêm lần nữa. Tuy nhiên, tỷ lệ khách hàng quay lại ở các danh mục khác lại rất thấp, ví dụ như *books_general_interest* và *luggage_accessories* chỉ có **6 khách hàng quay lại**, cho thấy cần có chiến lược cải thiện trải nghiệm khách hàng và các chương trình khuyến khích khách hàng mua lại, chẳng hạn như ưu đãi thành viên, hoặc cân nhắc thực hiện cá nhân hóa khuyến mãi dành cho khách hàng và cải thiện các dịch vụ hậu mãi.

![Ảnh chụp màn hình 2025-03-19 225326](https://github.com/user-attachments/assets/b2a03016-2240-4b24-b4b4-79c688727d96)

Biểu đồ trên cho thấy phần lớn doanh thu (**94,48%** – tương đương **15,42M**) đến từ khách hàng lần đầu mua sắm (*First-Time Customers*), trong khi khách hàng quay lại mua nhiều hơn một lần (*Repeat Customers*) chỉ đóng góp **5,52%** doanh thu (**0,90M**). Điều này cho thấy doanh nghiệp đang phụ thuộc nhiều vào việc thu hút khách hàng mới thay vì duy trì mối quan hệ và khai thác triệt để tệp khách hàng cũ. Với tỷ lệ khách hàng quay lại thấp, cần xem xét các chiến lược để giúp gia tăng customer retention rate, ví dụ như chương trình khách hàng thân thiết, cá nhân hóa ưu đãi, hoặc chăm sóc sau bán hàng, để tận dụng triệt để tệp khách hàng cũ cũng như tối ưu hóa doanh thu trong dài hạn.

#### 4. Tình trạng giao hàng đang như thế nào? Các đơn hàng giao đúng hẹn chiếm bao nhiêu phần trăm?
Dưới đây là bức tranh tổng quan về tình trạng giao hàng của Olist. Biểu đồ trên cho thấy *tỷ lệ giao hàng đúng hạn (On-Time Delivery Rate) đạt 93.4%*, nghĩa là phần lớn đơn hàng được giao đúng thời gian dự kiến, chỉ có khoảng *6.6% đơn hàng bị trễ*. Bên cạnh đó, *thời gian giao hàng trung bình (AVG Delivery Day) là 12.5 ngày*. 

<img width="596" alt="Ảnh màn hình 2025-03-20 lúc 14 07 22" src="https://github.com/user-attachments/assets/e785568b-e024-4bbe-8b00-22a99ce24482" />

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

Như vậy, có thể thấy hệ thống vận hành và giao hàng đang hoạt động khá hiệu quả, đảm bảo phần lớn đơn hàng được giao đúng thời gian cam kết. Tuy nhiên, vẫn còn một tỷ lệ nhỏ (6,6%) đơn hàng bị trễ, có thể làm ảnh hưởng đến trải nghiệm của khách hàng. Cần phân tích nguyên nhân gây ra sự chậm trễ, chẳng hạn như vấn đề kho vận, năng lực nhà cung cấp dịch vụ giao hàng hoặc tình trạng hàng hóa để có biện pháp cải thiện và tối ưu hơn nữa.

#### 5. Đánh giá trung bình của khách hàng là bao nhiêu? Có mối liên hệ nào giữa điểm đánh giá và thời gian giao hàng không?

<img width="499" alt="Ảnh màn hình 2025-03-21 lúc 14 37 08" src="https://github.com/user-attachments/assets/02136bf7-8979-4d6f-99c1-8c64ca45af64" />

Điểm đánh giá trung bình đạt **4,09/5**, cho thấy phần lớn khách hàng có trải nghiệm tích cực với sản phẩm/dịch vụ. Mặc dù điểm số này khá cao và gần mức tối đa, nhưng vẫn tồn tại một số đánh giá thấp hơn, thể hiện rặng vẫn còn một bộ phận khách hàng chưa thực sự hài lòng về sản phẩm/ dịch vụ mình nhận được. Việc phân tích sâu hơn các yếu tố ảnh hưởng, chẳng hạn như nhóm sản phẩm có điểm số thấp hoặc các phản hồi tiêu cực phổ biến, sẽ giúp xác định chính xác những điểm cần tối ưu để nâng cao trải nghiệm khách hàng.

![Ảnh chụp màn hình 2025-03-27 223807](https://github.com/user-attachments/assets/c947b807-7272-4326-9361-02328135ea9c)

Biểu đồ trên cho thấy mối quan hệ giữa *thời gian giao hàng trung bình* và *điểm đánh giá của khách hàng* từ 2016 đến 2018. Ban đầu, thời gian giao hàng rất lâu (**55 ngày vào Q3/2016**) nhưng đã giảm mạnh xuống **dưới 15 ngày từ 2017 trở đi**, cho thấy Olist đã có sự cải thiện trong quy trình vận chuyển.  

Song song đó, *điểm đánh giá trung bình tăng từ 1,00 lên hơn 4,00*, phản ánh sự hài lòng của khách hàng khi thời gian giao hàng được rút ngắn. Tuy nhiên, đến Q4/2018, cả hai chỉ số đều giảm, đặc biệt điểm đánh giá giảm xuống còn **2,25**.  

![Ảnh chụp màn hình 2025-03-19 221736](https://github.com/user-attachments/assets/9b74250f-1a30-4364-86a3-37cb0e7b19e6)

Hai biểu đồ trên thể hiện mối quan hệ giữa thời gian giao hàng, điểm đánh giá và số lượng đơn hàng.  

Biểu đồ bên trái cho thấy *thời gian giao hàng càng ngắn, điểm đánh giá càng cao*. Cụ thể, đơn hàng có điểm đánh giá **1 sao** có thời gian giao trung bình **21,25 ngày**, trong khi đơn hàng **5 sao** chỉ mất **10,62 ngày**. Điều này khẳng định *tốc độ giao hàng ảnh hưởng trực tiếp đến mức độ hài lòng của khách hàng*.  

Biểu đồ bên phải cho thấy *đơn hàng có điểm 5 sao chiếm tỷ lệ lớn nhất*, lên tới **57K đơn**, trong khi đơn hàng có điểm thấp như **1 sao (11K đơn) hoặc 2 sao (3K đơn) thấp hơn đáng kể**. Điều này gợi ý rằng chất lượng giao hàng và trải nghiệm tốt sẽ thúc đẩy số lượng đơn hàng, còn trải nghiệm kém có thể khiến khách hàng không quay lại và đánh giá không tốt về sản phẩm/ dịch vụ nhận được.  

Tóm lại, doanh nghiệp nên tập trung tối ưu tốc độ giao hàng để không chỉ nâng cao trải nghiệm khách hàng mà còn gia tăng số lượng đơn đặt hàng trong tương lai.

#### 6. Có danh mục sản phẩm nào có điểm đánh giá đáng chú ý không?

![Ảnh chụp màn hình 2025-03-19 222027](https://github.com/user-attachments/assets/083671b4-2b2d-4c10-8ddf-cb3999c7c3cc)

Biểu đồ cho thấy danh mục *security_and_services* có điểm đánh giá trung bình thấp nhất (**2.50**), cho thấy khách hàng không hài lòng với chất lượng sản phẩm này, có thể do tốc độ phản hồi chậm của người bán, giao hàng chậm hoặc sản phẩm dịch vụ không đáp ứng kỳ vọng của khách hàng. Trong khi đó, các danh mục còn lại có điểm đánh giá trung bình dao động từ **3.62 đến 3.93**, phản ánh mức độ hài lòng của khách hàng khá ổn định trong khoảng 3 - 4 điểm, nhưng vẫn nên cải thiện để đạt mức đánh giá tuyệt đối. Cần lưu ý rằng, có một mục "(Blank)" với điểm đánh giá **3.90**, có thể do dữ liệu chưa được gán đúng danh mục sản phẩm, gây ảnh hưởng đến tính chính xác của báo cáo. Để cải thiện, cần phân tích kỹ nguyên nhân khiến *security_and_services* có điểm thấp, kết hợp thu thập phản hồi chi tiết từ khách hàng để nâng cao trải nghiệm mua hàng, đồng thời kiểm tra và xử lý dữ liệu trống để đảm bảo độ chính xác trong đánh giá.

## Insight nổi bật
- Tổng doanh thu cao & có xu hướng tăng, đặc biệt vào Black Friday/ hoặc các chương trình khuyến mãi ngắn hạn theo mùa, cho thấy ́́*sức mua tăng mạnh vào mùa cao điểm.* 
- Một số danh mục có giá trị đơn hàng trung bình (AOV) cao đóng góp lớn vào doanh thu có thể tập trung upsell.
- Tỷ lệ hủy đơn thấp nhưng có xu hướng tăng vào một số giai đoạn. Một số danh mục bị hủy nhiều có thể do chất lượng hoặc thời gian giao hàng.
- Thanh toán chủ yếu bằng thẻ tín dụng & boleto (truyền thống).
- Thời gian giao hàng trung bình 12 ngày, trong đó 6.6% đơn bị giao trễ, việc giao hàng nhanh chóng có thể cải thiện đáng kể trải nghiệm & đánh giá của khách hàng.
- São Paulo là thị trường trọng điểm với doanh thu và số đơn hàng cao nhất do vậy nên tập trung các ưu đãi và cải thiện logistics tại khu vực này.

## Đề xuất
- Tăng trưởng doanh thu bằng cách tập trung vào danh mục có doanh thu cao (như bed_bath_table, health_beauty,..) và mở rộng thị trường tại các thành phố có doanh thu lớn như São Paulo. Triển khai chương trình giảm giá theo quý/mùa/đợt để kích thích nhu cầu mua sắm của khách hàng
- Đảm bảo kiểm soát chất lượng sản phẩm, và điều chỉnh giá hợp lý để giảm tỷ lệ hủy đơn, tăng sự hài lòng của khách hàng.
- Tối ưu phương thức thanh toán cũng như khuyến khích thanh toán qua thẻ tín dụng (phổ biến nhất) bằng các chương trình giảm giá hoặc ưu đãi hoàn tiền.
- Tăng số lượng khách hàng trung thành bằng cách xây dựng chương trình ưu đãi cho khách hàng cũ (ví dụ như chương trình khách hàng thân thiết) để tăng số lượng đơn hàng mua lại, đặc biệt tại các khu vực có nhiều khách hàng.

## Kết luận
Olist là một nền tảng thương mại điện tử có doanh thu cao và đang tăng trưởng tốt, đặc biệt khi có các đợt khuyến mãi lớn. Tuy nhiên, vẫn còn tiềm năng tối ưu hóa để tăng lợi nhuận và mở rộng thị trường thông qua tối ưu hoá thời gian giao hàng, trải nghiệm khách hàng và các danh mục sản phẩm. 

