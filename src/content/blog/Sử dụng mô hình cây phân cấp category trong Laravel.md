---
title: "Sử dụng mô hình cây phân cấp category trong Laravel"
meta_title: ""
description: "Sử dụng mô hình cây phân cấp category trong Laravel"
date: 2024-07-31T05:00:00Z
image: "/images/image-placeholder.png"
categories: ["Laravel"]
author: "Nguyễn Thanh Trường"
tags: ["Nested category"]
draft: false
---

Nếu bạn đã từng xây dựng mô hình menu đa cấp hay gì đó đại loại như xây dựng danh sách cấu trúc lồng nhau thì chắc hẳn bạn đã từng dùng đệ quy để làm việc này nhưng đệ qui là phương pháp không được khuyến khích dùng cho lắm vì nó rất hao tốn tài nguyên.Vậy có cách nào khác tối ưu hơn để giải quyết việc này.
Hôm nay mình sẽ giới thiệu bạn 1 phương pháp khác để giải quyết bài toán này đó là ứng dụng của mô hình nested set model để xây dựng danh sách đa cấp

## Bài toán

Giả sử chúng ta có danh sách như sau:
![A mushroom-head robot](/images/truonga7.png "Website đầu tiên")
Vậy khi đó trong database của ta sẽ có dạng như sau:
![A mushroom-head robot](/images/truonga8.png "Website đầu tiên")

con nào (dưới 1 cấp) và ngược lại, một category sẽ thuộc một category cha (trên 1 cấp) nào đó.Nhưng .... Giả sử bây h yêu cầu của bài toán phức tạp hơn. Bạn cần select các category là cha của 1 category bất kì. hay đơn giản hơn là liệt kê các category con của 1 category cha bất kì nào đó. Để truy xuất chúng, thông thường bạn sẽ áp dụng kĩ thuật đệ quy để duyệt cây- một kĩ thuật đơn giản và dễ viết. Tuy nhiên, cái nào cũng có hạn chế của nó, bởi lẽ nếu dữ liệu của bạn càng lớn thì tốc độ duyệt cây sẽ càng chậm.

## Thuật toán Nested Set Model

Nested sets model là một kỹ thuật dùng để thể hiện nested sets (các tập hợp lồng nhau) trong cơ sở dữ liệu. Thuật toán Nested Set Model sẽ chuyển cây thành một danh sách (list) các category và lưu xuống data storage. Nếu bạn muốn truy xuất các category con của 1 category nào đó, bạn chỉ cần biết được left và right của category cha và bạn dễ dàng truy suất nhanh chóng các category con đó mà không cần phải đệ quy.
Dữ liệu thực tế cho ví dụ trên sẽ là:
![A mushroom-head robot](/images/truonga9.png "Website đầu tiên")
Lưu trữ dữ liệu với cấu trúc như trên trong database rất tiện lợi cho việc truy vấn, vì ta dễ dàng tìm được tập hợp các nút thuộc vùng bất kỳ. Ví dụ ta muốn lấy tất cả các nút thuộc về nút Men’s:

```
SELECT * FROM category WHERE left>2 AND right<9;
```

hoặc lấy các category cha của nó

```
SELECT * FROM category WHERE left<2 AND right>9;
```

với phương pháp này có thể tiết kiệm 1 lượng lớn truy vấn với các ứng dụng như breadcrum, danh sách category con.

## Thêm category mới

Giả sử cần category cà vạt thuộc category Suits ta sẽ phải cập nhật lại dữ liệu như hình bên dưới. Tổng quát, ta cần làm 2 thao tác:
1.Tạo ra vùng trống mới trong category Suit tăng từ [3, 8] => [3, 10]
2.Thêm category cà vạt [8, 9] vào category Suit. Giá trị của category cà vạt sẽ có left = right category cha( Suit)và right = right category cha + 1. Sau đó ta tăng giá trị left, right phía sau lên 2 đơn vị (chính là vùng trống dành cho category mới) bằng điều kiện:

```
UPDATE categories SET Left = Left + 2 WHERE Left >= 9UPDATE categories SET Right = Right + 2 WHERE Right >= 9
```

## Xóa category

Ở đây mình sẽ trình bày trong trường hợp là khi xóa category, các category bên trong nó cũng sẽ bị xóa theo.Ở đây mình sẽ thực hiện xóa category Suit (và các category con của nó).
Tổng quát ta sẽ làm hai thao tác:

1. Xóa category và những category con của nó. 2.Cập nhật lại left, right cho các category bằng điều kiện:

```
UPDATE categories SET Left = Left – 6
```

6 được tính bằng số category con + cha bị xóa có thể tính bằng công thức = right - left + 1

## Move Category

Trong Ví dụ này mình sẽ thực hiện move category dress[15,20] thành con của category Suit[3,8]
Tổng quát ta sẽ làm hai thao tác:

1. 1.update left, right cho các category bị move
2. 2.Cập nhật lại left, right cho các category nằm trên suit

```
UPDATE categories SET Left = Left – 7 WHERE Left >=15 AND Right <= 20UPDATE categories SET Right = Right – 7 WHERE Left >=15 AND Right <= 20UPDATE categories SET Right = Right + 6 WHERE Left <=3 AND Right >= 8UPDATE categories SET Right = Right + 6 WHERE Right >15 AND Right <20UPDATE categories SET Left = Left + 6 WHERE Left >15 AND Left <20
```

## Tổng kết

Tuy đã trình bày vấn đề sửa xóa, move như vậy nhưng trường hợp move nếu phân tích kĩ thì rất phức tạp, mình trình bày vấn đề này ở mức cơ bản đủ để bạn hiểu rằng đối với nested set model ta hoàn toàn có thể xây dựng 1 danh sách đa cấp mà không cần dùng đến đệ qui( ít ra là hạn chế số lần sử dụng), tiết kiệm rất nhiều thời gian cho truy vấn. Tuy nhiên, hạn chế của thuật toán này là chi phí tốc độ cho việc lưu trữ sẽ chậm hơn nhiều so với cách đệ quy truyền thống vì nó cần tính tóan lại chỉ số left và right của một category. Do vậy, bạn chỉ nên áp dụng thuật toán này với những dữ liệu viết một(vài) lần và đọc rất nhiều lần. Chúc các bạn thành công.
