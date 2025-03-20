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
Mục tiêu của dự án là cung cấp những insight giá trị giúp tối ưu hóa hiệu suất và thúc đẩy tăng trưởng bền vững cho Olist thông qua trả lời các câu hỏi thuộc các nhóm dưới đây.

### Hiệu suất Bán hàng và Doanh thu
1. Tổng doanh thu của Olist là bao nhiêu? Doanh thu thay đổi như thế nào theo thời gian? Có giai đoạn nào đột biến hay suy giảm rõ rệt không?
2. Số lượng đơn hàng được đặt trên Olist mỗi tháng? Có biến động theo mùa hay xu hướng nào đáng chú ý không?
3. Các danh mục sản phẩm phổ biến nhất là gì? Khối lượng bán của chúng so sánh như thế nào? Có danh mục nào tiềm năng nhưng chưa được khai thác triệt để không?
4. Giá trị đơn hàng trung bình (AOV) là bao nhiêu? Có danh mục nào mang lại giá trị đơn hàng cao vượt trội?
5. Tỉ lệ hủy đơn trung bình là bao nhiêu? Danh mục sản phẩm nào có nhiều đơn hủy nhất?

### Khách hàng và Người bán
1. Có bao nhiêu người bán đang hoạt động trên Olist? Con số này thay đổi ra sao theo thời gian?
2. Doanh thu trung bình của một người bán là bao nhiêu?
3. Có bao nhiêu phần trăm khách hàng quay lại mua sắm? Họ chiếm bao nhiêu phần trăm trên tổng doanh thu?
   
### Thời gian giao hàng và Đánh giá của khách hàng
1. Có bao nhiêu khách hàng quay lại mua sắm? Họ chiếm bao nhiêu phần trăm tổng doanh thu?
2. Danh mục sản phẩm nào có nhiều khách hàng quay lại mua?
3. Đánh giá trung bình của khách hàng là bao nhiêu? Có mối liên hệ nào giữa điểm đánh giá và thời gian giao hàng sản phẩm không?
4. Phương thức thanh toán nào được khách hàng sử dụng nhiều nhất? Điều này thay đổi như thế nào theo từng danh mục sản phẩm và khu vực địa lý? Phương thức nào phổ biến nhưng lại có giá trị đơn hàng trung bình thấp?
5. Đánh giá và nhận xét của khách hàng tác động ra sao đến hiệu suất bán hàng? Có mối liên hệ nào giữa độ dài bình luận và điểm số không?
6. Tỉ lệ giữ chân khách hàng theo khu vực ra sao? Có vùng nào có lượng khách hàng mới cao nhưng tỷ lệ quay lại thấp không?

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

![Ảnh chụp màn hình 2025-03-19 220729](https://github.com/user-attachments/assets/8882e6c0-4bf5-406e-bb31-fca09fcdd66b)

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

![Ảnh chụp màn hình 2025-03-19 222734](https://github.com/user-attachments/assets/f2aad862-bf7b-46b2-8f6a-c17733ea957b)

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

Nhìn chung thì xu hướng phát sinh đơn hàng theo thời gian sẽ tỷ lệ thuận và không có biến đổi đáng kể so với xu hướng của doanh thu đã được nêu ở trên.

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

#### 5. Tỉ lệ hủy đơn trung bình là bao nhiêu? Danh mục sản phẩm nào có nhiều đơn hủy nhất?
Tổng cộng có 625 đơn hàng bị hủy trong giai đoạn 2016-2018. Biểu đồ cho thấy số lượng đơn hàng bị hủy tăng dần từ năm 2016 và đạt đỉnh vào **Q3 2018 với 140 đơn**, có thể phản ánh sự gia tăng tổng số đơn hàng hoặc vấn đề trong quy trình bán hàng và giao nhận. Tuy nhiên, đến **Q4 2018, số đơn hủy giảm mạnh xuống chỉ còn 4 đơn**, cho thấy có thể đã có sự điều chỉnh về chính sách, quy trình hoặc nhu cầu thị trường suy giảm. Xu hướng gia tăng hủy đơn từ **Q1 2017 đến Q3 2018** đặt ra dấu hỏi về trải nghiệm khách hàng, chất lượng sản phẩm hoặc quy trình xử lý đơn hàng. Để tối ưu, cần phân tích nguyên nhân dẫn đến đỉnh hủy đơn trong Q3 2018, đồng thời đánh giá những yếu tố giúp giảm mạnh trong Q4 2018 để áp dụng vào các giai đoạn tiếp theo, cũng như xem xét tỷ lệ hủy theo danh mục sản phẩm để tìm ra nguyên nhân cụ thể và có giải pháp phù hợp.

![Ảnh chụp màn hình 2025-03-19 220307](https://github.com/user-attachments/assets/52dab1a9-6740-45bf-921a-5d63d7470ac6)

Biểu đồ bên dưới cho thấy danh mục **sports_leisure (thể thao & giải trí)** có số lượng đơn hủy cao nhất (47 đơn), tiếp theo là **housewares (đồ gia dụng - 37 đơn)** và **health_beauty (sức khỏe & làm đẹp - 36 đơn)**. Đây có thể là những nhóm sản phẩm có nhu cầu cao nhưng cũng dễ bị hủy do thay đổi quyết định của khách hàng hoặc vấn đề về chất lượng, giá cả. Ngoài ra, các danh mục như **computers_accessories (máy tính & phụ kiện - 35 đơn) và toys (đồ chơi - 31 đơn)** cũng có tỷ lệ hủy đáng kể, có thể liên quan đến thời gian giao hàng hoặc sự thay đổi ưu tiên của khách hàng. Để tối ưu, cần tìm hiểu nguyên nhân hủy đơn ở các danh mục có tỷ lệ cao, đặc biệt với sports_leisure và housewares, nhằm điều chỉnh chiến lược bán hàng, chính sách đổi trả hoặc kiểm soát chất lượng sản phẩm.

![Ảnh chụp màn hình 2025-03-19 223836](https://github.com/user-attachments/assets/edf725ee-5aa1-4a22-a3c6-d157341d7a55)

### Hiệu suất Người bán
#### 1. Có bao nhiêu người bán đang hoạt động trên Olist? Con số này thay đổi ra sao theo thời gian?
Dữ liệu cho thấy tổng số người bán đạt **3.095**, với xu hướng tăng trưởng mạnh từ **Q3 2016** đến **Q2 2018**. Cụ thể, số lượng người bán tăng đều qua các quý, từ gần như **0K ở Q3 2016** lên **1.7K ở Q2 2018**, chứng tỏ nền tảng thu hút ngày càng nhiều người bán tham gia. Tuy nhiên, đến **Q3 2018**, số lượng người bán giảm nhẹ còn **1.6K**, cho thấy có thể có yếu tố ảnh hưởng như cạnh tranh gia tăng, chính sách thay đổi hoặc thị trường bão hòa. Để duy trì và phát triển hệ sinh thái, nền tảng cần phân tích nguyên nhân giảm và đưa ra chiến lược thu hút cũng như giữ chân người bán hiệu quả hơn.

<img width="546" alt="Ảnh màn hình 2025-03-19 lúc 09 42 26" src="https://github.com/user-attachments/assets/4c84f5d6-271b-41dd-8050-27d2f96edb29" />

### Hành vi mua hàng và Trải nghiệm khách hàng 
#### 1. Phương thức thanh toán nào được khách hàng sử dụng nhiều nhất? Điều này thay đổi như thế nào theo từng danh mục sản phẩm và khu vực địa lý? Phương thức nào phổ biến nhưng lại có giá trị đơn hàng trung bình thấp?
<img width="322" alt="Ảnh màn hình 2025-03-19 lúc 09 15 02" src="https://github.com/user-attachments/assets/603c170e-5b38-451b-8e76-3fa0dea830a4" />

Dựa trên biểu đồ, Sao Paulo dẫn đầu về số lượng đơn hàng cho thấy đây là thị trường trọng điểm cần tập trung khai thác. Thẻ tín dụng chiếm ưu thế vượt trội trong các phương thức thanh toán, đặc biệt ở các thành phố lớn như Sao Paulo và Rio de Janeiro, phản ánh xu hướng tiêu dùng linh hoạt tại các thành phố lớn. Doanh nghiệp nên tận dụng bằng cách triển khai ưu đãi khi thanh toán qua thẻ tín dụng để thúc đẩy doanh số.

Tuy nhiên, phương thức "boleto" vẫn chiếm tỷ trọng đáng kể, đặc biệt ở Sao Paulo, cho thấy một nhóm khách hàng vẫn giữ thói quen thanh toán truyền thống.  Nguyên nhân có thể do tâm lý muốn kiểm soát chi tiêu hoặc chưa quen với các hình thức thanh toán điện tử. Để không mất đi nhóm khách hàng này, doanh nghiệp nên cân nhắc giữ lại và đơn giản hóa quy trình thanh toán bằng boleto, đồng thời có thể truyền thông về các lợi ích cũng như độ an toàn của các phương thức hiện đại để từng bước thay đổi hành vi tiêu dùng.

Ngoài ra, Rio de Janeiro và Belo Horizonte cũng nổi lên như hai thị trường tiềm năng với lượng đơn hàng đáng kể chỉ đứng sau Sao Paulo. Doanh nghiệp có thể mở rộng marketing và cải thiện dịch vụ tại đây. Những thành phố như Curitiba, Brasilia và Campinas tuy có lượng đơn hàng thấp hơn nhưng vẫn đáng chú ý để phát triển trong tương lai. 

Tóm lại, doanh nghiệp nên tập trung khai thác triệt để Sao Paulo, đẩy mạnh thanh toán thẻ tín dụng và mở rộng các thành phố tiềm năng để phát triển bền vững.

#### 2. Có bao nhiêu khách hàng quay lại mua sắm? Họ chiếm bao nhiêu phần trăm tổng doanh thu? Và họ quay lại mua sắm danh mục sản phẩm nào nhiều nhất?
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

<img width="380" alt="Ảnh màn hình 2025-03-19 lúc 09 14 26" src="https://github.com/user-attachments/assets/e59dcaaa-5cce-4952-be17-dfc4c47d983f" />

Dữ liệu cho thấy tổng số khách hàng là **99.44K**, nhưng tỷ lệ giữ chân khách hàng chỉ ở mức **3.0%**, cho thấy hầu hết khách hàng chỉ mua một lần mà không quay lại. Xét theo danh mục sản phẩm, **bed_bath_table**, **sports_leisure**, **health_beauty** và **computers_accessories** là những danh mục có số lượng khách hàng quay lại mua nhiều nhất, cho thấy đây có thể là các mặt hàng có nhu cầu tái mua cao hoặc mang lại trải nghiệm tốt. Tuy nhiên, tỷ lệ khách hàng quay lại ở các danh mục khác rất thấp, đặc biệt là **books_general_interest** và **luggage_accessories** chỉ có **6 khách hàng quay lại**, cho thấy cần có chiến lược cải thiện trải nghiệm khách hàng và các chương trình khuyến khích mua lại, chẳng hạn như ưu đãi thành viên, cá nhân hóa khuyến mãi hoặc cải thiện dịch vụ hậu mãi.

![Ảnh chụp màn hình 2025-03-19 225326](https://github.com/user-attachments/assets/b2a03016-2240-4b24-b4b4-79c688727d96)

Biểu đồ trên cho thấy phần lớn doanh thu (**94,48%** – tương đương **15,42M**) đến từ khách hàng lần đầu mua sắm (**First-Time Customers**), trong khi khách hàng quay lại (**Repeat Customers**) chỉ đóng góp **5,52%** doanh thu (**0,90M**). Điều này cho thấy doanh nghiệp đang phụ thuộc nhiều vào việc thu hút khách hàng mới thay vì duy trì và khai thác khách hàng cũ. Với tỷ lệ khách hàng quay lại thấp, cần xem xét các chiến lược tăng cường **customer retention**, như chương trình khách hàng thân thiết, cá nhân hóa ưu đãi, hoặc chăm sóc sau bán hàng, để nâng cao giá trị trọn đời của khách hàng và tối ưu hóa doanh thu dài hạn.

#### 4. Các đơn hàng giao đúng hẹn chiếm bao nhiêu phần trăm?

Để xác định thời gian giao hàng có đúng hạn hay không, chúng ta thực hiện hai bước sau:
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

Dữ liệu cho thấy **93,43%** đơn hàng được giao đúng hạn (**92,91K đơn hàng**), trong khi **6,57%** đơn hàng bị giao trễ (**6,54K đơn hàng**). Điều này cho thấy hệ thống vận hành và giao hàng đang hoạt động khá hiệu quả, đảm bảo phần lớn đơn hàng được giao đúng thời gian cam kết. Tuy nhiên, vẫn còn một tỷ lệ nhỏ đơn hàng bị trễ, có thể ảnh hưởng đến trải nghiệm khách hàng. Cần phân tích nguyên nhân gây chậm trễ, chẳng hạn như vấn đề kho vận, năng lực nhà cung cấp dịch vụ giao hàng hoặc tình trạng hàng hóa, để có biện pháp cải thiện và tối ưu hơn nữa.

![Ảnh chụp màn hình 2025-03-19 221834](https://github.com/user-attachments/assets/ad27c041-ebf8-499c-a5c5-50c9cf70cd29)


#### 5. Đánh giá trung bình của khách hàng là bao nhiêu? Có mối liên hệ nào giữa điểm đánh giá và thời gian giao hàng không?
![Ảnh chụp màn hình 2025-03-19 222248](https://github.com/user-attachments/assets/1db26458-ac06-43bb-88cf-f1171bbe8ad0)

<img width="242" alt="Ảnh màn hình 2025-03-19 lúc 09 40 23" src="https://github.com/user-attachments/assets/7ba2ea81-28ca-4b24-86c7-4a86dc6c0d92" />

Dựa trên hai biểu đồ trên, có thể thấy rõ rằng thời gian giao hàng là yếu tố tác động trực tiếp đến trải nghiệm khách hàng và điểm đánh giá.

Biểu đồ đầu tiên cho thấy một mối quan hệ nghịch đảo giữa thời gian giao hàng trung bình và điểm đánh giá. Cụ thể, những đơn hàng có thời gian giao lâu nhất (21.25 ngày) chỉ nhận được điểm đánh giá trung bình là 1, trong khi những đơn hàng được giao nhanh hơn (10.62 ngày) lại đạt điểm đánh giá 5. Điều này phản ánh rõ ràng rằng khách hàng càng được phục vụ nhanh chóng, họ càng hài lòng và đánh giá cao.

Xuyên suốt từ năm 2016 đến năm 2018, có thể nhận thấy nỗ lực tối ưu hóa vận hành của Olist để cải thiện thời gian giao hàng và nâng cao trải nghiệm khách hàng.

Ở giai đoạn đầu tiên (Q3 2016), thời gian giao hàng lên đến 55 ngày, dẫn đến điểm đánh giá cực kỳ thấp, chỉ đạt 1.00 điểm. Tuy nhiên, nhờ sự cải tiến liên tục, đến Q4 2018, thời gian giao hàng đã giảm xuống 2.25 ngày, trong khi điểm đánh giá tăng đáng kể lên 4.26 — gần chạm ngưỡng lý tưởng 5.0 điểm. Đây là minh chứng rõ ràng cho thấy việc rút ngắn thời gian giao hàng không chỉ giúp Olist cải thiện hiệu suất vận hành, mà còn xây dựng được lòng tin và sự hài lòng từ phía khách hàng.

Tuy nhiên, không phải toàn bộ quá trình đều diễn ra thuận lợi. Năm 2017 đánh dấu một giai đoạn có sự biến động, mặc dù thời gian giao hàng đã giảm đáng kể (xuống khoảng 11-14 ngày), nhưng điểm đánh giá vẫn chỉ dao động quanh mức 4.0 - 4.12 điểm và không tăng trưởng đáng kể. Điều này cho thấy, bên cạnh việc rút ngắn thời gian giao hàng, chất lượng sản phẩm và dịch vụ cũng là yếu tố quan trọng để cải thiện sự hài lòng toàn diện của khách hàng

Như vậy có thể thấy rằng, thời gian giao hàng nhanh là yếu tố hàng đầu tác động đến điểm đánh giá. Vì vậy, Olist cần tiếp tục đầu tư vào cải thiện chuỗi cung ứng, nâng cấp hệ thống logistics và hợp tác với các đơn vị vận chuyển hiệu quả. Tuy nhiên, cũng phải để ý rằng năm 2017 là minh chứng cho việc giao hàng nhanh không đồng nghĩa với việc khách hàng hoàn toàn hài lòng. Olist cần đảm bảo rằng sản phẩm đúng mô tả, đóng gói cẩn thận và dịch vụ khách hàng chuyên nghiệp để giữ vững điểm đánh giá.

Biểu đồ này thể hiện phần trăm số đơn hàng giao đúng hẹn và trễ
![Ảnh chụp màn hình 2025-03-19 221834](https://github.com/user-attachments/assets/6cfb0f41-17b6-4b86-9740-9634fd6dae38)

Điểm thấp hơn hẳn so với category khác
![Ảnh chụp màn hình 2025-03-19 222027](https://github.com/user-attachments/assets/083671b4-2b2d-4c10-8ddf-cb3999c7c3cc)

Mối liên hệ giữa thời gian giao hàng và điểm đánh giá theo thời gian
![Ảnh chụp màn hình 2025-03-19 222148](https://github.com/user-attachments/assets/ba959182-a36c-4f3c-b43d-bb153816d5ee)


#### 4. Đánh giá và nhận xét của khách hàng tác động ra sao đến hiệu suất bán hàng? 
![Ảnh chụp màn hình 2025-03-19 221736](https://github.com/user-attachments/assets/9b74250f-1a30-4364-86a3-37cb0e7b19e6)

#### 5. Tỉ lệ giữ chân khách hàng theo khu vực ra sao? Có vùng nào có lượng khách hàng mới cao nhưng tỷ lệ quay lại thấp không?

## Insight

## Đề xuất

## Kết luận


