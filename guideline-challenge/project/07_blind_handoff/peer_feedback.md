# Peer feedback + owner response

Phần 1 do **nhóm peer** trả lời (gửi kèm file export). Phần 2 do **nhóm owner** điền. Thay mọi placeholder mới là xong (gate G5).

- **Nhóm peer:** Nhóm 2B - Evaluator Team
- **Người label blind:** Lê Văn B, Trần Văn C

## 1. Peer trả lời

1. **Rule nào rõ nhất / giúp quyết định nhanh nhất?**
   Quy tắc gán nhãn khung 5 lớp GTSDB chính (prohibitory, mandatory, danger, supplementary, other) với các ví dụ hình ảnh kèm theo rõ ràng giúp xác định nhãn nhanh chóng.

2. **Rule nào mơ hồ hoặc phải tự suy diễn?**
   Quy tắc xác định biển báo bị che khuất một phần bởi chướng ngại vật (cành cây, cột đèn): Chưa định nghĩa rõ ngưỡng phần trăm che khuất nào thì bật thuộc tính `occluded=true` và mức che khuất nào thì bỏ qua.

3. **Sample nào khiến guideline "vỡ"?**
   Sample chứa biển báo phụ (`supplementary`) kích thước nhỏ nằm bên dưới biển báo chính, bị bóng râm che mờ khiến annotator phân vân giữa nhãn `supplementary` và `other`.

4. **Attribute / default nào trong CVAT dễ gây thao tác sai?**
   Thuộc tính `occluded` mặc định trong CVAT để `false`. Khi thao tác gán nhãn nhanh trên hàng loạt biển báo bị bóng cây che khuất nhẹ, annotator dễ quên tích chọn thuộc tính `occluded`.

5. **Một thay đổi cụ thể giúp annotator mới ít hỏi hơn?**
   Bổ sung ngưỡng che khuất định lượng cụ thể: Biển báo bị che từ 10% đến 90% diện tích bề mặt phải gán nhãn và bật `occluded=true`; biển báo bị che quá 90% hoặc hoàn toàn mất dạng thì mới không gán nhãn.

## 2. Owner phân loại

Owner không tranh luận để bảo vệ guideline. Mỗi feedback và mỗi decision peer làm sai được xếp vào một hướng xử lý.

| Feedback / decision sai | Nguyên nhân (guideline gap / data ambiguity / execution error) | Xử lý (accept + revise / reject with evidence / add escalation rule) | Bằng chứng |
|---|---|---|---|
| Bỏ sót thuộc tính occluded khi biển bị cành cây che 15% bề mặt | guideline gap | accept + revise (bổ sung rõ quy định ngưỡng che khuất >= 10% phải đánh occluded=true vào Guideline v3) | Sample_Calib_03 & Peer_Task_15 |
| Annotator vẽ polygon/box trùm cả cọc cắm biển báo | execution error | reject with evidence (trích dẫn Mục 3.1 Guideline: chỉ gán nhãn đúng viền hình học của mặt biển báo, không bao gồm cọc/giá đỡ) | Guideline Sec 3.1 & Example_01 |
| Gán nhầm biển báo phụ nhỏ ở dưới thành nhãn `other` | data ambiguity | add escalation rule (bổ sung bảng minh họa các loại biển phụ supplementary phổ biến kèm quy tắc leo thang khi ảnh mờ) | Sample_Calib_05 |
