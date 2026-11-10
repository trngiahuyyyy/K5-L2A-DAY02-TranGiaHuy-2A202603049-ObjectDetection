# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Trần Gia Huy
- MSSV: 2A202603049
- Email: trn.giahuyyy@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/trngiahuyyyy/K5-L2A-DAY02-TranGiaHuy-2A202603049-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/github/trngiahuyyyy/K5-L2A-DAY02-TranGiaHuy-2A202603049-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: ĐẠT cấu trúc: 50/50 file nhãn hợp lệ
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_003, drive_007, drive_011, drive_018, drive_025, drive_031, drive_042, drive_044, drive_049, drive_050
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: 1 số xe ở xa không nhìn rõ còn lại không phát hiện lỗi khi rà 10 ảnh.
- Commit khóa nhãn ban đầu: 8768084

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 365 | |
| FP | 250 | |
| FN | 288 | |
| Precision | 0.5935 | |
| Recall | 0.559 | |
| F1 | 0.5757 | |
| mean_iou_of_matches | 0.8755 | |
| passed | true | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Python 3, T4 GPU, ultralytics: 8.4.208, torch: 2.14.1+cu130
- Các tham số thay đổi so với notebook nếu có và lý do: Không đổi
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4090642760207564 |
| Recall | 0.23393606659813174 |
| mAP50 | 0.2706670034721526 |
| mAP50–95 | 0.1995504032766255 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
  Lớp có recall thấp nhất có ý nghĩa là van: recall 0.083 (12 instance), so với car 0.652 và truck 0.200. bus có recall 0 nhưng chỉ có 2 instance nên không đủ kết luận. Trong 12 xe van gold, 7 xe bị model đoán là bus (IoU 0.73–0.93) và 5 xe không có box nào khớp. Ảnh minh họa là drive_052: xe van gold có IoU 0.88 với box bus của model, tức box khớp nhưng sai lớp. 
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
  mAP50 chỉ cần IoU ≥ 0.5 còn mAP50–95 lấy trung bình đến IoU 0.95, nên nếu mAP50 cao mà mAP50–95 thấp thì model tìm đúng xe nhưng box chưa khít. Cần kiểm tra cạnh box dự đoán có lỏng hoặc cắt vào thân xe không, xe nhỏ, xe bị che, và hai xe sát nhau. Kết quả của tôi không đúng tình huống này: mAP50 là 0.271 và mAP50–95 là 0.200, cả hai đều thấp. Riêng car có 0.740 và 0.524, chênh nhưng không lớn. Vì vậy vấn đề chính của tôi là bỏ sót xe và nhầm lớp (recall tổng 0.234), chưa phải box lệch. Mean IoU nhãn của tôi so với gold là 0.8755 nên box nhãn khá sát.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
  van là lớp tệ nhất sau bus (P 0.136, R 0.083), và guideline của tôi có ca mơ hồ van/car. Dữ kiện: eval có 94 car trên 113 instance, nhãn train có [số] box car, [số] van, [số] truck, [số] bus, và confusion matrix cho thấy van bị đoán thành [lớp ...]. Tuy vậy tôi chưa kết luận được nguyên nhân do guideline, vì lớp ít mẫu, nhãn của tôi có recall 0.559 so với gold, và eval chỉ có 10 ảnh.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
  Không dùng eval để train vì cần dữ liệu model chưa thấy để đo khả năng tổng quát hóa, nếu không số đo chỉ phản ánh việc nhớ lại. Trong notebook, data.yaml đặt val là 10 ảnh eval, nên chúng được dùng để tính metric sau mỗi epoch và chọn best.pt. Vì vậy eval là validation, không phải test set độc lập. Trong lần chạy của tôi, best.pt có số liệu trùng với epoch 1 (P 0.409, R 0.234, mAP50 0.271, mAP50–95 0.200), các epoch sau đều thấp hơn, cho thấy 50 ảnh train chưa cải thiện được model tiền huấn luyện, và 10 ảnh eval thì số đo dao động mạnh.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
  Tự đánh giá 65, phần Độ bám sát và hình học box và Gold-set annotation QC chưa kiểm được. Gold-set annotation đã được giáo viên cho phép F1>0.5

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

Công cụ đã dùng claude.ai, dùng ở bước label xe và đã tự kiểm tra bằng cách đưa quyết định nên box hay không.
