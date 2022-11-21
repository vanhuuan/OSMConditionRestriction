# OSMConditionRestriction

Tài liệu này dùng để hướng dẫn viết các Tag cấm đường có điều kiện trong Osm 
# Cấu trúc của 1 Tag
- Cấu trúc của tag giống như các tag bình thường khác.
- Kết thúc của tag phải có hậu tố: `:conditional`. Giá trị bao gồm giá trị thực theo sau là ký tự `@` và điều kiện.
Cú pháp chung của thẻ như sau (các trường trong dấu ngoặc vuông `[..]` là tùy chọn):
```
[<restriction-type>][:<transportation mode>][:<direction>]:conditional = <restriction-value> @ <condition>
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
- Không khuyến khích việc thêm trường restriction type khi đã có trường transpotation mode.
## Direction:
-Chưa hỗ trợ, bỏ qua.
# Value
- Là phần đứng sau dấu `=`.
- Giá trị bao gồm giới hạn thực tế, (yes: đi được, no: không đi được), theo sau là ký tự @ và điều kiện. Thêm dấu cách trước và sau ký tự @ để cải thiện khả năng đọc.
Hầu hết chỉ một giá trị với một điều kiện tương ứng được chỉ định. Trong những trường hợp đặc biệt, có thể cần sử dụng hai hoặc nhiều cặp key-value. Các cặp key-value như vậy phải được phân tách bằng dấu chấm phẩy. Một tình huống có thể là giới hạn tốc độ nhất định vào những thời điểm nhất định trong ngày nhưng giới hạn tốc độ khác (thấp hơn) trong trường hợp điều kiện ẩm ướt. Xem Các hạn chế xung đột bên dưới cách sắp xếp nhiều cặp key-value.

## Restriction-value:
- Đây là giá trị thực của hạn chế, ví dụ: `yes`, `no`.
- `Yes` là cho phép đi qua.
- `No` là không cho phép đi qua.
## Condition:
- Trường này chỉ định điều kiện mà hạn chế hợp lệ. Có thể phân biệt nhiều loại điều kiện khác nhau. Hiện tại đang hỗ trợ điều kiện theo thứ trong tuần và giờ trong ngày.
+ Ngày và giờ: Sử dụng cú pháp chuẩn của giá trị * của thẻ opens_hours = *. Nếu giá trị đó bao gồm dấu chấm phẩy (";"), thì điều kiện đó phải được bao bởi dấu ngoặc tròn (ngoặc đơn), ví dụ: (Fr 06: 00-24: 00; Tu-Fr 00: 00-24: 00; Sa 00: 00-13: 00), Mo-Fr 07: 00-19: 00.
+ Các từ viết tắt: Mo(thứ 2), Tu(thứ 3), We(thứ 4), Th(thứ 5), Fr(thứ 6), Sa(thứ 7), Su(chủ nhật).
+ Chưa hỗ trợ đọc nhiều điều kiện trên 1 tag (ví dụ: `access:conditional=no @ (17:00-20:00);yes @ (06:00-08:00)`)
+ Chưa hỗ trợ value là `yes`. Mếu bạn muốn chỉ cho phép đi trong 1 khoảng thời gian, bạn phải cấm các khoảng thời gian không được đi.
+ Điều kiện có thể gồm thứ hoặc không. Nếu không có trường thứ thì mặc định điều kiện thời gian sẽ được áp dụng cho tất cả các ngày. Ví dụ `motor_vehicle:conditional=no @ (18:30-07:30)`
+ Điều kiện có thể có thời gian hoặc không. Nếu không có trường thời gian thì được thời gian cấm sẽ được cho là cả ngày. Ví dụ `	motorcycle:conditional=no @ (Sa,Su)`
+ Có thể áp dụng chung 1 điều kiện thời gian cho các thứ liên tiếp trong tuần bằng cách thêm dấu `-` ở giữa 2 thứ bắt đầu và kết thúc. Ví dụ `access:conditional=no @ (Mo-Fr 07:00-10:00)`. Có thể bỏ điều kiện thời gian để coi như cấm tất cả các thứ. 
+ Chưa hỗ trợ việc cấm liên ngày ( từ ngày này sang ngày khác ) mà phải chia làm 2 rule để cấm. Ví dụ cấm từ 23h00 thứ 2 đến 03h00 thứ 3 :  `access:conditional=no @ (Mo 23:00-24:00; Tu 00:00-03:00)`
+ Có thể có nhiều khoảng thời gian trong 1 điều kiện. Giữa các khoảng thời gian được định nghĩa có bao gồm thời gian trong ngày được chia bằng dấu `;`
+ Đọc thêm tại đây: https://wiki.openstreetmap.org/wiki/Key:opening_hours.

# Ví dụ
- `bicycle:conditional=no @ (Sa 08:00-16:00)` : Cấm xe đạp vào thứ 7 từ 8 giờ đến 16 giờ.
- `motor_vehicle:conditional=delivery @ (Mo-Fr 06:00-11:00,17:00-19:00;Sa 03:30-19:00)` : Cấm ô tô và xe máy từ thứ 2 đến thứ 6 từ 6 giờ đến 11 giờ và 17 giờ đến 19 giờ và vào thứ 7 từ 3 giờ 30 đến 19 giờ.
- `motorcycle:conditional=no @ (Sa,Su)` : Cấm xe máy vào cả ngày thứ 7 và chủ nhật.
- `access:conditional=no @ (Mo-Fr 07:00-10:00)` : Cấm tất cả các loại xe từ thứ 2 đến thứ 6 mỗi ngày từ 7 giờ đến 10 giờ.
