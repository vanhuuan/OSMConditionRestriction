# OSMConditionRestriction

Tài liệu này dùng để hướng dẫn viết các Tag cấm đường có điều kiện trong Osm 
# Cấu trúc của 1 Tag
- Cấu trúc của tag giống như các tag bình thường khác.
- Kết thúc của tag phải có hậu tố: `:conditional`. Giá trị bao gồm giá trị thực theo sau là ký tự `@` và điều kiện.
Cú pháp chung của thẻ như sau (các trường trong dấu ngoặc vuông `[..]` là tùy chọn):
```
[<restriction-type>][:<transportation mode>][:<direction>]:conditional = <restriction-value> @ <condition>[;<restriction-value> @ <condition>]
```
# Key
- Là phần trước dấu `=`.
## Restriction type:
- Là loại cấm ( cấm tốc tộ, cấm đi, đường 1 chiều ). Hiện tại, chỉ chỗ trợ kiểu: access. 
- Được sử dụng khi muốn cấm tất cả các phương tiện đi vào đường.
- Là phần tùy chọn.
- Ví dụ khi sử dụng 1 mình: `access:conditional=no @ (09:00-17:00)`
## Transpotation mode:
- Là loại phương tiện cần tìm đường. Hiện tại đang hỗ trợ các loại:
+ vehicle/ motor_vehicle: Xe máy và xe ô tô.
+ foot: Đi bộ.
+ bicycle: Đi xe đạp.
+ motorcycle: Đi xe máy.
+ car/ motorcar: Đi xe ô tô
- Ví dụ khi sử dụng 1 mình: `motor_vehicle:conditional=no @ (18:30-07:30)`
- Ví dụ khi sử dụng chung với access: `access:motor_vehicle:conditional=no @ (18:30-07:30)`
- Là tag phần bắt buộc nếu không có phần restriction Type.
## Direction:
-Chưa hỗ trợ, bỏ qua.
# Value
- Là phần đứng sau dấu `=`.
- Giá trị bao gồm giới hạn thực tế, (yes: đi được, no: không đi được), theo sau là ký tự @ và điều kiện. Thêm dấu cách trước và sau ký tự @ để cải thiện khả năng đọc.
Hầu hết chỉ một giá trị với một điều kiện tương ứng được chỉ định. Trong những trường hợp đặc biệt, có thể cần sử dụng hai hoặc nhiều cặp key-value. Các cặp key-value như vậy phải được phân tách bằng dấu chấm phẩy. Một tình huống có thể là giới hạn tốc độ nhất định vào những thời điểm nhất định trong ngày nhưng giới hạn tốc độ khác (thấp hơn) trong trường hợp điều kiện ẩm ướt. Xem Các hạn chế xung đột bên dưới cách sắp xếp nhiều cặp key-value.

## Restriction-value:
- Đây là giá trị thực của hạn chế, ví dụ: yes, no.
- Yes là cho phép đi qua.
- No là không cho phép đi qua.
## Condition:
- Trường này chỉ định điều kiện mà hạn chế hợp lệ. Có thể phân biệt nhiều loại điều kiện khác nhau. Hiện tại đang hỗ trợ điều kiện theo thứ trong tuần và giờ trong ngày.
+ Ngày và giờ 
