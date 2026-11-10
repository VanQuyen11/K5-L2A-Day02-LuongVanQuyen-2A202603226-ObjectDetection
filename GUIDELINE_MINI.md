# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |xe con, sedan, SUV, taxi, xe bán tải | xe buýt, xe tải, mô tô|
| `truck` |xe tải chở hàng, xe đầu kéo | xe bán tải/cabin nhỏ thuộc car|
| `bus` |xe buýt, minibus chở khách | van chở hàng/chở người thuộc van|
| `van` | xe van, minivan chở hàng hoặc chở người| xe buýt lớn (bus) hoặc xe con thông thường (car)|

## Quy tắc chung

- Box bám sát vật thể nghĩa là: chỉ bao quanh phần thân xe nhìn thấy, không lấy nền hoặc xe bên cạnh.
- Vật thể bị che khuất: vẫn gán box riêng nếu còn đủ đặc điểm để nhận diện lớp, vật thể bị che quá 50% thì bỏ qua.
- Vật thể bị cắt bởi rìa ảnh: box chạm đúng rìa ảnh, không đoán phần nằm ngoài khung.
- Vật thể rất nhỏ/mơ hồ: không tự đoán, ghi lại case và hỏi giảng viên.
- Mỗi xe hợp lệ được gán một bounding box riêng.
- Xe đang đỗ vẫn được gán vì trạng thái đỗ không làm thay đổi lớp xe.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_011` |Nhiều xe chồng lấn, một số xe bị xe buýt che  |Gán box riêng cho từng xe nếu vẫn nhận diện được loại xe |Không gộp các xe thành một box; phần nhìn thấy vẫn đủ để xác định đối tượng |
| `drive_014` |Xuất hiện xe cứu hộ/kéo xe  |Chỉ gán nếu phương tiện phù hợp với định nghĩa `truck`; nếu không chắc thì ghi chú hỏi giảng viên |Schema phân loại theo loại phương tiện, không dựa vào màu hoặc chức năng tạm thời  |
| `drive_042` |Xe tải lớn bị mờ và che một phần |Gán `truck` nếu còn thấy rõ cabin/thùng hàng; box chỉ bao quanh phần nhìn thấy |Xe bị che hoặc mờ vẫn được gán khi đủ đặc điểm nhận diện |
