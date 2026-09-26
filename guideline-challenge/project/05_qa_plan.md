# QA plan + quality gates — GTSDB Traffic Sign Segmentation

## Flow

Guideline → Calibration → Production → Self-QC → Review → Rework → Quality Gate.

- **Ai review, review bao nhiêu:** Trưởng nhóm / QA Lead thực hiện review 100% toàn bộ mẫu ảnh trong tập dữ liệu.
- **Chọn sample theo rule nào:** Kiểm tra 100% toàn bộ mẫu ảnh, trong đó ưu tiên kiểm duyệt kỹ các ảnh có tag rủi ro (`critical`, `edge`, `occlusion`, `ambiguity`).
- **Issue được ghi ở đâu, đóng thế nào:** Ghi nhận lỗi trực tiếp trên thuộc tính/comment của object trên CVAT và quản lý trong log QA. Annotator sửa xong thì QA Lead re-check và đóng Issue (chuyển trạng thái CLOSED).
- **Khi phát hiện guideline gap thì update và version ra sao:** Thảo luận nhóm chốt Gold Decision, ghi nhận sửa đổi vào `08_revision_log.md` và nâng phiên bản guideline (v1 → v2 → v3).

## Defect severity

| Severity | Định nghĩa cho project này | Ví dụ | Action mặc định |
|---|---|---|---|
| Critical | Lỗi bỏ sót hoặc phân loại sai nghiêm trọng biển báo làm xe tự hành ra quyết định nguy hiểm. | Bỏ sót biển Cấm (`prohibitory`) / biển Nguy hiểm (`danger`), hoặc gán nhầm biển Cấm thành biển Chỉ dẫn (`mandatory`). | Bắt buộc sửa 100% ngay lập tức trước khi nghiệm thu dữ liệu. |
| Major | Lỗi bỏ qua thuộc tính quan sát hoặc ranh giới polygon sai lệch gây ảnh hưởng đến huấn luyện mô hình. | Quên bật attribute `occluded` khi biển bị cây che khuất 20%, quên bật `truncated` khi biển sát mép ảnh, hoặc ranh giới polygon lệch > 5px. | Trả về Rework cho Annotator sửa lại trong vòng 24h. |
| Minor | Lỗi sai lệch nhỏ không ảnh hưởng lớn đến quyết định của mô hình. | Ranh giới polygon lệch nhẹ 2px-4px trên biển báo ở cự ly xa; quên bật `blurred` trên biển nhỏ mờ. | Ghi chú nhắc nhở Annotator rút kinh nghiệm cho các ảnh sau. |
| Question | Ảnh quá lóa sáng, bị nhòe hoặc biển hiệu lạ không thể đưa ra quyết định chắc chắn. | Biển báo hình dạng lạ chưa quy định trong ontology hoặc bị phản quang lóa trắng hoàn toàn. | Gắn tag `image_escalate` để nhóm họp chốt Gold Decision. |

## Metrics

| Metric | Cách tính | Vì sao phù hợp với bài toán |
|---|---|---|
| Mean IoU | Tỷ lệ diện tích giao trên diện tích hợp (Intersection over Union) ranh giới polygon giữa Annotator và Gold Standard | Đảm bảo mô hình học chính xác ranh giới thực của mặt biển báo (yêu cầu $\ge 85\%$). |
| Class Accuracy | (Số lượng biển báo phân loại đúng class) / (Tổng số biển báo) | Đảm bảo mô hình phân biệt chính xác ý nghĩa 5 nhóm biển báo (yêu cầu $\ge 95\%$). |

Metric high-risk tách riêng: Critical Defect Escape Rate = 0% (Tuyệt đối không để lọt bất kỳ lỗi Critical nào qua bước review).

## Quality gate

```text
PASS if:
  - Critical Defect Escape Rate = 0%
  - Tỷ lệ lỗi Major <= 5%
  - Mean IoU >= 85%
REWORK if: Xuất hiện >= 1 lỗi Critical HOẶC Tỷ lệ lỗi Major > 5% HOẶC Mean IoU < 85%
REJECT / ESCALATE if: Tỷ lệ lỗi Major > 20% HOẶC có mâu thuẫn quy tắc chưa chốt trong guideline
```

Trade-off: Chấp nhận dành 100% thời gian review toàn bộ tập ảnh để đảm bảo rủi ro bằng 0 (Zero-Critical Risk) cho hệ thống xe tự hành, hoàn toàn phù hợp với Downstream Contract.
