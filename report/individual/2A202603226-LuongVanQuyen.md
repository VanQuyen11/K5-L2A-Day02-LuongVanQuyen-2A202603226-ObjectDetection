# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Lương Văn Quyến
- MSSV:2A202603226
- Email: 26ai.quyenlv@vinuni.edu.vn
- Lớp: L2A
- Link repo bài làm: https://github.com/VanQuyen11/K5-L2A-Day02-LuongVanQuyen-2A202603226-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/github/VanQuyen11/K5-L2A-Day02-LuongVanQuyen-2A202603226-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb#scrollTo=qF-OTeRcmAxz

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file `.txt`, tương ứng với 50 ảnh trong `dataset/images/train/`.
- Kết quả checker và lỗi đã xử lý: ĐẠT cấu trúc 50/50 file nhãn hợp lệ.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_011,drive_012,drive_014,drive_015,drive_018,drive_019,drive_031,drive_034,drive_042,drive_050.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Sau khi kiểm tra, các box được điều chỉnh để bám sát phần thân xe nhìn thấy, tách riêng các xe bị chồng lấn và giữ nguyên quy tắc không gán người đi bộ, xe máy hoặc vật thể ngoài bốn lớp.
- Commit khóa nhãn ban đầu: Bổ sung mã commit thực tế sau khi commit và push lên GitHub.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |569 | |
| FP |125 | |
| FN |84 | |
| Precision |0.8199 | |
| Recall |0.8714 | |
| F1 |0.8448 | |
| mean_iou_of_matches |0.9428 | |
| passed |true | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, GPU, Ultralytics 8.4.208,torch-2.14.1+cu130
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.5927979448497973 |
| Recall |0.4631205673758865 |
| mAP50 |0.49095500300242595 |
| mAP50–95 |0.329816064896478 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?

Lớp có recall thấp nhất là [điền tên lớp từ confusion matrix]. Ảnh minh họa là [ví dụ drive_055.jpg], trong đó model bỏ sót hoặc nhận diện sai [loại xe cụ thể]. Nguyên nhân có thể do lớp này có ít mẫu hơn car, xe bị che khuất hoặc kích thước vật thể nhỏ.

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?

Điều này thường cho thấy model đã phát hiện đúng vị trí tương đối của vật thể, nhưng bounding box chưa đủ chính xác. Cần kiểm tra box có bị rộng, hẹp, lệch tâm hoặc bao gồm quá nhiều nền hay không. mAP50 dùng ngưỡng IoU thấp hơn, còn mAP50–95 đánh giá với nhiều ngưỡng IoU khắt khe hơn.

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?

Lỗi có thể liên quan đến việc phân biệt car, van và truck, đặc biệt với xe bán tải nhỏ, xe van hoặc xe tải bị che khuất. Guideline quy định xe bán tải/cabin nhỏ thuộc car, xe van chở người hoặc chở hàng thuộc van, còn xe tải có thùng hàng lớn thuộc truck. Dữ kiện hỗ trợ là confusion matrix hoặc ảnh dự đoán cho thấy một số xe bị gán nhầm giữa các lớp này. Ngoài ra, tập dữ liệu bị mất cân bằng vì số lượng car nhiều hơn đáng kể so với truck, bus và van.

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

Không dùng ảnh eval để train để giữ nguyên sự tách biệt giữa dữ liệu huấn luyện và dữ liệu đánh giá. Nếu dùng eval để train, kết quả đánh giá có thể bị cao giả tạo vì model đã nhìn thấy các ảnh đó.
Trong notebook, 10 ảnh eval được dùng làm validation set để theo dõi quá trình huấn luyện, chọn checkpoint tốt nhất best.pt và tính các chỉ số Precision, Recall, mAP50 và mAP50–95. Vì vậy, tập eval trong bài này là validation chứ chưa phải test set độc lập hoàn toàn.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Gán nhãn |Một số ảnh có nhiều xe chồng lấn, xe nhỏ hoặc bị che khuất |Đối chiếu lại schema trong `GUIDELINE_MINI.md`, tách mỗi xe thành một box riêng và chỉ gán phần nhìn thấy |Đã hoàn thành 50 file nhãn train |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

- Hoàn thành định dạng YOLO: đã có 50 file nhãn đúng basename và checker đạt 50/50.
- Đầy đủ vật thể: đã rà lại các ảnh có xe bị che khuất, xe nhỏ và xe ở mép ảnh.
- Hình học bounding box: box được vẽ quanh phần thân xe nhìn thấy, không gộp các xe với nhau.
- Phân lớp: sử dụng cố định bốn class `car`, `truck`, `bus`, `van` theo `data.yaml`.
- Mini guideline: đã định nghĩa bốn class, quy tắc vẽ box và các trường hợp mơ hồ.
- Gold-set annotation QC: đạt Precision = 0.8199, Recall = 0.8714, F1 = 0.8448 và mean IoU = 0.9428.
- Pipeline YOLO: đã có các chỉ số model trong báo cáo; cần bảo đảm notebook Colab đã chạy đầy đủ, có ảnh prediction và hai thư mục assets trước khi nộp chính thức.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- Công cụ AI: ChatGPT được sử dụng để giải thích cấu trúc dự án, hướng dẫn định dạng YOLO, kiểm tra lỗi GitHub/Colab và góp ý cách trình bày báo cáo.
- Phần tự kiểm tra: tự gán nhãn, tự rà ảnh, tự chạy `tools/check_yolo_labels.py`, tự chạy `tools/evaluate_annotations.py` và kiểm tra các kết quả Precision, Recall, F1, mean IoU.
- Tài liệu trong repository đã sử dụng:
  - `README.md`
  - `GUIDE.md`
  - `RULES.md`
  - `RUBRIC.md`
  - `SUBMISSION.md`
  - `CHECKPOINTS.md`
  - `data.yaml`