# Revision log

| Version | Đổi gì | Vì sao | Bằng chứng |
|---|---|---|---|
| v1 | Bản nháp đầu tiên | Khởi tạo bài toán Segmentation 5 class biển báo GTSDB với 3 attribute boolean | `01_problem_statement.md` v1, `03_cvat_labels.json` |
| v2 | Bổ sung 2 Edge Cases: (1) Biển chỉ hướng di chuyển tới Quận A/Quận B; (2) Biển hình thoi màu vàng (Đường ưu tiên) | Giải quyết bất đồng calibration giữa annotator về phân loại biển chỉ đường và biển ưu tiên | GTS09, GTS11, `06_calibration_report.csv` dòng 1-3 |
| v2.1 | Thắt chặt quy tắc chống bỏ sót thuộc tính `occluded`: Thêm ngưỡng che khuất >= 5%, bổ sung Checklist 3s, bổ sung EC09 & EC10 và đưa vào QA Major Defect | Khắc phục triệt để lỗi bỏ sót thuộc tính occluded do annotator quên bật khi vật cản nhỏ/mỏng | EC09, EC10, `02_guideline.md` Mục 10 Lỗi 4, `05_qa_plan.md` |
| v3 | Chuẩn hóa quy định che khuất (10%-90%) và hướng dẫn gán nhãn chi tiết cho nhóm biển báo phụ (`supplementary`) | Phản hồi cải tiến từ đợt Blind Handoff và Peer Feedback của Nhóm 2B | `peer_feedback.md`, `clarification_log.csv`, `02_guideline.md` |
