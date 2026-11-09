# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con | Thường là xe 4 chỗ, có mũi xe và cốp riêng |
| `truck` | Xe tải | Có cabin tách biệt thùng hàng, thân dài và cao hơn car |
| `bus` | Xe buýt | Thân dài và nhiều cửa sổ, nhiều hàng ghế |
| `van` | Xe Van | Thân hộp liền khối, cao hơn car, ngắn hơn bus |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bao gồm cả hình dáng, bánh xe, chạm sát thân xe, tính cả gương chiếu hậu. Với xe chạy nhanh tính cả phần mờ.
- Vật thể bị che khuất: Bao gồm cả những xe bị cột điện che khuất, trừ những xe không thể xác định hoặc bị che quá nửa xe.
- Vật thể bị cắt bởi rìa ảnh: Vẽ box đến rìa, trừ khi xe chỉ hở phần đầu hoặc phần đít không xác định được loại gì.
- Vật thể rất nhỏ/mơ hồ: Không vẽ box vì không chắc chắn đây là xe gì.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_007` |1 xe con bị khuất sau cột điện |Quyết định vẽ box kể cả bị cột điện đè |Schema mô tả loại vật thể, không mô tả trạng thái. Xe bị che một phần vẫn cần box nếu phần nhìn thấy đủ để nhận ra là car. Cột điện mảnh nên vẫn thấy rõ hai bên, và mỗi xe chỉ một box. |
| `drive_025` |Xe con hoặc xe van bị khuất sau cây không nhìn rõ |Quyết định không box |Phần nhìn thấy không đủ để xác định chắc lớp (car hay van). GUIDE.md quy định không tự đoán lớp trong trường hợp này, nên không gán nhãn thay vì gán bừa. |
| `drive_042` |Xe tải chở quá nhiều đồ nên không biết vẽ box bao gồm hàng không |Quyết định vẽ box có cả hàng |Xe có cabin riêng và thùng hàng nên thuộc truck. Schema và GUIDE.md không nói riêng về hàng chất cao. Tôi chọn tính hàng vì hàng di chuyển cùng xe, và giữ cách này nhất quán cho mọi xe tương tự. |
